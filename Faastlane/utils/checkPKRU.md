This C program demonstrates the use of Intel's Memory Protection Keys (MPK) for userspace (PKEYs), a feature available in modern Intel processors for controlling access to pages of memory. It includes functions for reading and writing the PKRU (Protection Key Rights Register), as well as a function for testing memory protection. Let's go through each function and the `main` function:

### `__rdpkru` Function

```c
void __rdpkru(void) { ... }
```

- Reads the current value of the PKRU register using the `RDPKRU` instruction.
- `pkru`: Holds the value of the PKRU register.
- `sanityCheck`: Used to check if the `EDX` register is set to 0 as expected after the `RDPKRU` instruction.
- Prints the PKRU value if `sanityCheck` is 0.

The function `void __rdpkru(void)` in your code is a C function that demonstrates how to read the Protection Key Rights for Users (PKRU) register on a system that supports Intel's Memory Protection Extensions (MPX). The PKRU register is used for controlling access rights to pages of memory based on protection keys. This function specifically uses inline assembly to perform the operation. Let's break down the code:

1. **Function Declaration**:
   - `void __rdpkru(void)`: Declares a function named `__rdpkru` that takes no parameters and returns no value (void).

2. **Variable Declaration**:
   - `int pkru, sanityCheck;`: Declares two integer variables `pkru` and `sanityCheck`. The `pkru` variable is intended to store the value of the PKRU register, and `sanityCheck` is used to verify the operation.

3. **Printing a Message**:
   - `printf("Reading PKRU..\n");`: Prints a message to the console indicating that the PKRU register is being read.

4. **Inline Assembly**:
   - The `asm` block is where the actual reading of the PKRU register takes place.
     - `"mov $0x0, %%ecx\n\t"`: Moves 0 into the ECX register. RDPKRU uses ECX as an input, and here it is set to 0.
     - `"RDPKRU\n\t"`: The actual assembly instruction to read the PKRU register.
     - `"mov %%eax, %0\n\t"`: Moves the contents of the EAX register (which now contains the lower 32 bits of the PKRU value) into the `pkru` variable.
     - `"mov %%edx, %1\n\t"`: Moves the contents of the EDX register into the `sanityCheck` variable. Normally, after executing RDPKRU, EDX should be zero.
   - The `: "=gm" (pkru), "=gm" (sanityCheck)` part is the output operand list. It tells the compiler where to store the results (`pkru` and `sanityCheck`).

5. **Checking the Result**:
   - `if(!sanityCheck)`: Checks if `sanityCheck` is zero. If it is, it means the RDPKRU operation was successful and EDX was set as expected.
   - `printf("PKRU is set to %d\n", pkru);`: If successful, it prints the value of the PKRU register.
   - `else printf("EDX not set to 0 by RDPKRU\n");`: If `sanityCheck` is not zero, it prints an error message indicating that EDX was not set to 0 by RDPKRU as expected.

### `__resetpkru` Function

```c
void __resetpkru(int regVal) { ... }
```

- Resets the PKRU register to a specified value (`regVal`).
- Uses the `WRPKRU` instruction to write to the PKRU register.
- The function currently ignores the `regVal` parameter and sets PKRU to 0.

The function `void __resetpkru(int regVal)` is designed to reset the Protection Key Rights for Users (PKRU) register to a specified value. The PKRU register is part of Intel's Memory Protection Extensions (MPX) and is used for controlling access rights to memory pages based on protection keys. Let's break down the code:

1. **Function Declaration**:
   - `void __resetpkru(int regVal)`: Declares a function named `__resetpkru` that takes an integer parameter `regVal` and returns no value (void).

2. **Printing Initial Message**:
   - `printf("Resetting PKRU with value %d..\n", regVal);`: This line prints a message to the console indicating that the PKRU register is about to be reset with the value provided in `regVal`.

3. **Inline Assembly**:
   - The `asm` block contains the assembly code to reset the PKRU register.
     - `"mov $0x0,  %eax\n\t"`: Moves the value 0 into the EAX register. When using the WRPKRU instruction, EAX should contain the new value for the PKRU register. However, this code seems to ignore the `regVal` parameter and just uses 0.
     - `"mov $0x0,  %ecx\n\t"`: Moves 0 into the ECX register. ECX is also required for the WRPKRU instruction, but its purpose depends on the specific usage and is set to 0 here.
     - `"mov $0x0,  %edx\n\t"`: Moves 0 into the EDX register. For WRPKRU, EDX also needs to be set, typically to 0.
     - `"WRPKRU\n\t"`: The WRPKRU instruction is executed to write the value in EAX (which is 0) into the PKRU register.
   - This code effectively resets the PKRU register to 0, but it does not use the `regVal` parameter. This seems like an oversight or error in the code.

4. **Printing Completion Message**:
   - `printf("Finished resetting PKRU.\n");`: After executing the inline assembly, this line prints a message indicating that the PKRU register has been reset.

**Note**: The function as written does not actually use the `regVal` parameter to set the PKRU register, which might not be the intended behavior. If the goal is to set the PKRU register to a value provided by `regVal`, the code should move `regVal` into the EAX register instead of 0. Additionally, this function uses inline assembly, which is specific to the CPU architecture and platform it is running on (in this case, Intel CPUs with support for MPX).

### `__wrpkru` Function

```c
void __wrpkru(int regVal) { ... }
```

- Sets the PKRU register to a specific value (in this case, `0xc`).
- The function's parameter `regVal` is not used.

The function `void __wrpkru(int regVal)` is designed to write a specific value to the Protection Key Rights for Users (PKRU) register. The PKRU register is a part of the Intel CPU architecture and is used to control access to protected memory pages. Here's a breakdown of the function:

1. **Function Declaration**:
   - `void __wrpkru(int regVal)`: Declares a function named `__wrpkru` that takes an integer parameter `regVal` and returns no value (void).

2. **Printing Initial Message**:
   - `printf("Writing PKRU with value fffffffc\n");`: This line prints a message to the console. It states that the PKRU is being written with the value `fffffffc`. However, this message is misleading because the actual value written to the PKRU (as per the assembly code) is `0xc`, not `fffffffc`.

3. **Inline Assembly**:
   - The `asm` block contains the assembly code to write to the PKRU register.
     - `"mov $0xc,  %eax\n\t"`: Moves the value `0xc` (12 in decimal) into the EAX register. The EAX register is used to specify the value to be written to the PKRU register.
     - `"mov $0x0,  %ecx\n\t"` and `"mov $0x0,  %edx\n\t"`: These lines move the value 0 into the ECX and EDX registers, respectively. These registers must be set to 0 when using the WRPKRU instruction.
     - `"WRPKRU\n\t"`: Executes the WRPKRU instruction, which writes the value in EAX (12) to the PKRU register.

4. **Printing Completion Message**:
   - `printf("Finished writing PKRU.\n");`: After executing the inline assembly, this line prints a message indicating that the PKRU register has been written with the new value.

**Important Notes**:
- The function name and the printed message suggest that it writes the value provided in the `regVal` parameter to the PKRU register. However, the actual implementation hardcodes the value `0xc` and does not use the `regVal` parameter. This is likely an error or oversight in the function.
- The usage of inline assembly means this function is highly platform-specific. It will only work on platforms where the PKRU register and the WRPKRU instruction are supported (i.e., certain Intel CPUs).
- The hardcoded message "Writing PKRU with value fffffffc" does not reflect the actual value written to the register (`0xc`), which could lead to confusion. It's important for the printed messages to accurately reflect the operation being performed.

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

The `protectAddr` function in C demonstrates the use of memory protection keys (pkeys) for Linux, a feature that allows controlling access to pages of memory using protection keys. Here's a breakdown of its functionality:

1. **Function Declaration**:
   - `void protectAddr(int untrusted, int size)`: Declares a function named `protectAddr` that takes two integer parameters, `untrusted` and `size`, and returns no value (void).

2. **Variable Declarations**:
   - Various variables (`map`, `pkey`, `pkru`, `dompret`, `virtAddr`) are declared for use in memory operations and system calls.

3. **Memory Allocation**:
   - `virtAddr  = mmap(NULL, size, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0)`: Allocates a memory region of the specified `size`, with read and write permissions. The memory is private and anonymous (not backed by a file).
   - `map  = (int *) virtAddr`: Casts the allocated memory to an integer pointer.
   - `*map = 100`: Writes the value 100 to the allocated memory.

4. **Allocating a Protection Key**:
   - `pkey = syscall(330,0,0)`: Calls the Linux system call (number 330) to allocate a new protection key.

5. **Applying Memory Protection**:
   - `dompret = syscall(329, virtAddr, size, 3, pkey)`: Uses the `mprotect_pkey` system call (number 329) to apply memory protection to the allocated region with the allocated key.

6. **Setting and Resetting PKRU**:
   - `pkru = (int) 3 << 2*(pkey-1)`: Calculates the PKRU value to be set.
   - `__resetpkru(0)`: Calls a custom function to reset the PKRU register to allow trusted code execution.

7. **Trusted Code Execution**:
   - Executes trusted code by modifying the value in the allocated memory.

8. **Reading PKRU State**:
   - `__rdpkru()`: Calls a custom function to read the current state of the PKRU register.

9. **Untrusted Code Execution**:
   - Depending on the `untrusted` parameter, it either modifies the memory value or prints it. This simulates the behavior of untrusted code trying to access the protected memory.

10. **Finishing Up**:
   - Prints a message indicating the end of the function execution.

**Key Points**:
- The function demonstrates the use of memory protection keys in Linux for secure memory management. 
- It shows how to allocate memory, assign a protection key, and then use system calls to manage memory access permissions.
- The use of `__resetpkru` and `__rdpkru` (custom functions) is for handling the PKRU register, which controls access to memory based on the assigned protection keys.
- This code is specific to Linux environments that support memory protection keys and involves low-level memory and register operations.

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