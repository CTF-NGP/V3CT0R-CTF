# Vault Door 0x5A - Solution

**Flag:** V3CT0R{ST4RT3D_R3V_3NG}

## Solution
**Category:** Reverse Engineering (Beginner)
**Flag:** `V3CT0R{ST4RT3D_R3V_3NG}`
**Password:** `V3CT0R{ST4RT3D_R3V_3NG}` (the password and the flag are the same string)

---

## Overview

`crackme` is a small C program that prompts for a password, strips the trailing
newline, and compares the input against a hardcoded string using `strcmp`.
If the strings match it prints `Access granted!`, otherwise `Access denied.`

Because the comparison string is stored as a plain literal in the binary's
`__cstring` section, it can be recovered with basic static analysis, or
dynamically by catching the `strcmp` call in a debugger.

Intended skills: `file`, `strings`, a first look at a debugger, and a first look
at Ghidra.

---

## Method 1: `strings` (fastest)

```bash
file crackme
strings crackme
```

`file` reports a Mach-O 64-bit arm64 executable. `strings` output includes:

```
Enter password:
V3CT0R{ST4RT3D_R3V_3NG}
Access granted!
Access denied.
```

Apart from the prompt and the two result messages, only one string is left:
`V3CT0R{ST4RT3D_R3V_3NG}`. That is the value being compared. Run it to confirm:

```bash
./crackme
Enter password: V3CT0R{ST4RT3D_R3V_3NG}
Access granted!
```

---

## Method 2: Dynamic analysis with `lldb` (macOS)

`ltrace` is not available on macOS, but a breakpoint on `strcmp` does the same
job.

```bash
lldb ./crackme
(lldb) breakpoint set --name strcmp
(lldb) run
Enter password: test
```

When the breakpoint hits, `strcmp(input, secret)` has been called. On arm64 the
first two arguments are in `x0` and `x1`:

```
(lldb) memory read -f s $x0     # your input -> "test"
(lldb) memory read -f s $x1     # the real password
```

On an Intel Mac (x86-64) use `$rdi` and `$rsi` instead.

---

## Method 3: Static analysis in Ghidra

1. Create a new project and import `crackme` (let Ghidra auto-detect Mach-O, arm64).
2. Run auto-analysis with the defaults.
3. In the Symbol Tree, open `_main` (or `entry` and follow it to `main`).
4. In the decompiler window you will see the flow:
   - `printf("Enter password: ")`
   - `fgets(buf, 0x40, stdin)`
   - `strcspn(buf, "\n")` used to strip the newline
   - `strcmp(buf, "V3CT0R{ST4RT3D_R3V_3NG}")`
   - `puts("Access granted!")` if the result is 0, else `puts("Access denied.")`
5. The string literal in the `strcmp` call is the password.

---

## Takeaways

- Anything stored as a plain string literal can be recovered with `strings`.
- A call to `strcmp` exposes both operands to anyone who can watch it, via a
  debugger breakpoint or `ltrace` on Linux.
- Decompilers like Ghidra show string literals directly in the call.

**Next challenge idea:** store the password XOR-encoded in the data section and
compare it with a manual loop, so that `strings` finds nothing and there is no
`strcmp` call to intercept.