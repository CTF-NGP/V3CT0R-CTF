# Vault Door 0xF3 - Solution

**Flag:** V3CT0R{M4ST3R_0F_R3V3R5E_3NG}

## Solution
The binary accepts an 8-byte serial, interprets it as two big-endian 32-bit words, runs those words through a five-round Feistel network implemented by a custom bytecode VM, and compares the result with two constants. If the comparison succeeds, the program decrypts the flag using the serial as a repeating XOR key.

The solution has three parts:

1. Understand the VM instructions and reconstruct the Feistel round function.
2. Reverse the five rounds from the stored target words to recover the serial.
3. Use the recovered serial to undo the flag's XOR encryption.

## 1. Identify the input format

The program requires exactly eight characters:

```c
#define SERIAL_LEN 8
```

The first four bytes form the initial left word and the final four bytes form the initial right word. Both are assembled in big-endian order:

```c
L = (serial[0] << 24) | (serial[1] << 16) |
    (serial[2] << 8)  | serial[3];

R = (serial[4] << 24) | (serial[5] << 16) |
    (serial[6] << 8)  | serial[7];
```

The expected final words are:

```c
TARGET_L = 0x018C4E01
TARGET_R = 0xC71532FB
```

## 2. Reconstruct the VM's round function

Reading the VM program shows that each round performs these operations:

```text
x = R XOR round_constant
x = x * 0x9E3779B1        (modulo 2^32)
x = ROTL32(x, 11)
new_L = R
new_R = L XOR x
```

Therefore:

\[
F(R,RC)=\operatorname{ROTL}_{32}((R\oplus RC)\cdot 0x9E3779B1,11)
\]

and each Feistel round is:

\[
(L',R')=(R,\;L\oplus F(R,RC))
\]

The five round constants, in order, are:

```text
A5A5A5A5
3C3C5A5A
1234ABCD
DEADBEEF
0BADF00D
```

The multiplication is performed with 32-bit unsigned arithmetic, so values wrap modulo \(2^{32}\).

## 3. Reverse the Feistel network

A Feistel round is reversible even when its round function is not itself inverted. Given the output pair `(new_L, new_R)`:

```text
old_R = new_L
old_L = new_R XOR F(new_L, round_constant)
```

So the inverse of one round is:

```text
old_L = new_R XOR F(new_L, RC)
old_R = new_L
```

Apply this inverse to the target words using the round constants in **reverse order**. The resulting two 32-bit words are the original serial's first and second four-byte halves.

## 4. Recovery script

Save this as `solve.py`. It reverses the target pair and then decrypts the embedded flag.

```python
MASK = 0xFFFFFFFF
TARGET_L = 0x018C4E01
TARGET_R = 0xC71532FB
ROUND_CONSTANTS = [
    0xA5A5A5A5,
    0x3C3C5A5A,
    0x1234ABCD,
    0xDEADBEEF,
    0x0BADF00D,
]
ODD_CONST = 0x9E3779B1

ENC_FLAG = bytes([
    0x11, 0x48, 0x4e, 0xc5, 0xa8, 0x8b, 0x40, 0x1b, 0x5e, 0xa7,
    0xec, 0x69, 0x57, 0x5e, 0xfe, 0xd4, 0xfd, 0x4e, 0x61, 0x3a,
    0xcd, 0xeb, 0x09, 0x7c, 0x09, 0xfb, 0xe6, 0xab, 0x0b
])

def rotl32(x, r):
    r &= 31
    return ((x << r) | (x >> ((32 - r) & 31))) & MASK

def F(r, rc):
    x = (r ^ rc) & MASK
    x = (x * ODD_CONST) & MASK
    return rotl32(x, 11)

# Undo the rounds in reverse order.
left, right = TARGET_L, TARGET_R
for rc in reversed(ROUND_CONSTANTS):
    left, right = (right ^ F(left, rc)) & MASK, left

serial = left.to_bytes(4, "big") + right.to_bytes(4, "big")
print("Recovered serial:", serial.decode("ascii"))

# The flag byte at index i was XORed with:
# serial[i % 8] XOR ((i * 0x2F) & 0xFF)
flag = bytes(
    enc ^ (serial[i % len(serial)] ^ ((i * 0x2F) & 0xFF))
    for i, enc in enumerate(ENC_FLAG)
)
print("Flag:", flag.decode("ascii"))
```

Run it with:

```bash
python3 solve.py
```

The intended serial is `R3V3RS3!`, and the intended flag is:

```text
V3CT0R{M4ST3R_0F_R3V3R5E_3NG}
```

## 5. Why the flag encryption is reversible

The flag loop computes a byte-wise XOR:

```c
k = serial[i % 8] ^ ((i * 0x2F) & 0xFF);
plaintext = enc_flag[i] ^ k;
```

XOR is its own inverse. Since the recovered serial is known, the key byte can be reproduced for each position and XORed with the corresponding encrypted byte to recover the flag.

## Key takeaways

- A custom bytecode VM does not prevent reverse engineering; decode its instruction semantics and trace the register operations.
- Feistel networks can be inverted by applying the round transformation in reverse order.
- Big-endian byte packing matters when reconstructing the original input.
- Unsigned 32-bit arithmetic must wrap modulo \(2^{32}\).
- XOR obfuscation is reversible when the key-generation rule and key material are known.
