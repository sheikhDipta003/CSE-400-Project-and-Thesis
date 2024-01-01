This C program demonstrates the use of Intel's Memory Protection Keys (MPK) for userspace (PKEYs), a feature available in modern Intel processors for controlling access to pages of memory. It includes functions for reading and writing the PKRU (Protection Key Rights Register), as well as a function for testing memory protection. Let's go through each function and the `main` function:

### `__rdpkru` Function

```c
void __rdpkru(void) { ... }
```

- Reads the current value of the PKRU register using the `RDPKRU` instruction.
- `pkru`: Holds the value of the PKRU register.
- `sanityCheck`: Used to check if the `EDX` register is set to 0 as expected after the `RDPKRU` instruction.
- Prints the PKRU value if `sanityCheck` is 0.

### `__resetpkru` Function

```c
void __resetpkru(int regVal) { ... }
```

- Resets the PKRU register to a specified value (`regVal`).
- Uses the `WRPKRU` instruction to write to the PKRU register.
- The function currently ignores the `regVal` parameter and sets PKRU to 0.

### `__wrpkru` Function

```c
void __wrpkru(int regVal) { ... }
```

- Sets the PKRU register to a specific value (in this case, `0xc`).
- The function's parameter `regVal` is not used.

### `protectAddr` Function

```c
void protectAddr(int untrusted, int size) { ... }
```

- Demonstrates the use of memory protection keys.
- `untrusted`: Indicates whether the code is running in a trusted or untrusted mode.
- `size`: The size of the memory region to protect.
- Allocates memory using `mmap` and assigns it a protection key using the `pkey_alloc` and `pkey_mprotect` system calls (syscalls 330 and 329, respectively).
- Adjusts the PKRU register to allow or deny access to the protected memory based on the current mode (trusted or untrusted).
- In trusted mode, the memory is accessible, whereas in untrusted mode, attempting to access the memory should result in a segmentation fault (if the PKRU is set correctly).

### `main` Function

```c
int main(void) { ... }
```

- Calls `protectAddr` twice, first in untrusted mode (`1`) and then in a mode that checks the value (`2`).
- Resets the PKRU to 0 at the end.

### Example Usage

This program is designed to test the functionality of Intel's MPK in user space. When run on a compatible system, it should:

1. Allocate memory and assign a protection key.
2. Modify the memory in trusted mode.
3. Attempt to modify or access the memory in untrusted mode, which should fail if PKRU is set to deny access.

This program is a demonstration of how MPK can be used to protect sensitive data in memory from being accessed or modified by untrusted parts of the same application. It's particularly useful in scenarios where an application needs to handle both trusted and untrusted code or data.