# P-Scanf

**P-Scanf** is a lightweight C library that provides safer and more convenient input handling compared to the standard `scanf`.  

### Features
- Validates input type: if a user enters a character when an integer is expected, an error is raised.
- Automatically clears the input buffer when using P-Scanf macros.
- Provides safe string handling without the need to predefine buffer sizes (strings are dynamically allocated).
- Automatically frees memory if a memory allocation failure occurs.

---

## Installation

### Using **cl.exe** (Visual Studio Compiler)
1. Copy the static library `libpscanf.lib` to:
   ```
   C:\Program Files\Microsoft Visual Studio 12.0\VC\lib
   ```
   To avoid the linker warning:
   ```
   warning LNK4099: PDB 'libpscanf.pdb' was not found with 'libpscanf.lib' or at 'libpscanf.pdb'; linking object as if no debug info
   ```
   Place the `libpscanf.pdb` file in the same folder as `libpscanf.lib`.

2. Add the library in **Visual Studio**:  
   Go to `Project -> Properties -> Configuration Properties -> Linker -> Input -> Additional Dependencies` and add:
   ```
   libpscanf.lib
   ```

3. Copy the header file `pscanf.h` to:
   ```
   C:\Program Files\Microsoft Visual Studio 12.0\VC\include
   ```

4. Include it in your source code:
   ```c
   #include <pscanf.h>
   ```

---

### Using **Dev-C++**
1. Copy the static library `libpscanf.a` to:
   ```
   C:\Program Files\Dev-Cpp\lib
   ```

2. In **Dev-C++**, go to `Project -> Project Options -> Parameters -> Linker` and add:
   ```
   -lpscanf
   ```

3. Copy the header file `pscanf.h` to:
   ```
   C:\Program Files\Dev-Cpp\include
   ```

Download the library and header file here: [PScanf Release v2.0](https://github.com/DevD4v3/P-Scanf/releases/tag/v2.0)

---

## Macros

- `dataread(_format, _var, ...)`  
  Reads any type of data from standard input.

- `strread(_var, ...)`  
  Reads a string from standard input. Memory is dynamically allocated.  
  Returns `1` if memory allocation fails.

---

## Functions

- `pause()`  
  Pauses the program and prints:  
  ```
  Press enter to continue...
  ```

- `sfree()`  
  Frees all memory allocated by the `strread` macro.

---

## Usage Examples

### Reading an Integer
```c
#include <stdio.h>
#include <pscanf.h>

int main(void) {
    int a;
    dataread("%d", &a, "Enter an int: ");
    printf("%d\n", a);
    return 0;
}
```
If the user enters an invalid value (e.g., a character instead of an integer), the error message will be:  
```
Error: Enter an integer value:
```

---

### Reading a String
```c
#include <stdio.h>
#include <pscanf.h>

int main(void) {
    string name = { NULL }; 
    strread(&name, "Enter a string: ");
    printf("String: %s - Length: %d\n", name, name.length);
    sfree();
    pause();
    return 0;
}
```
- `name.length` gives the string length.

---

### Iterating Over a String
```c
#include <stdio.h>
#include <pscanf.h>

int main(void) {
    int i;
    string name = { NULL }; 
    strread(&name, "Enter a string: ");
    for (i = 0; i != name.length; ++i)
        printf("%c\n", name.s[i]);
    sfree();
    pause();
    return 0;
}
```
- `name.s` gives access to the characters.

---

### Storing Multiple Strings
```c
#include <stdio.h>
#include <stdint.h>
#include <pscanf.h>
#define MAX_STRINGS (5)

/* Returns 0 if memory was successfully allocated, otherwise 1. */
uint8_t DataEntry(string* name) {
    int i;
    for (i = 0; i != MAX_STRINGS; ++i) {
        /* Braces are required since strread expands into 2 lines of code. */
        strread(&name[i], "Enter string %d: ", i + 1);
    }
    return 0;
}

void PrintData(string* name) {
    int i, j;
    for (i = 0; i != MAX_STRINGS; ++i)
        printf("%s\n", name[i]);
    printf("\n");
    for (i = 0; i != MAX_STRINGS; ++i) {
        for (j = 0; j != name[i].length; ++j)
            printf("%c", name[i].s[j]);
        printf("\n");
    }
}

int main(void) {
    string name[MAX_STRINGS] = { NULL };
    if (DataEntry(name)) return 1;
    PrintData(name);
    sfree();
    pause();
    return 0;
}
```

**Note:** Always initialize `string` variables to `NULL`.

---

## Credits
- [DevD4v3](https://github.com/DevD4v3)  
  Author and maintainer of **P-Scanf**.
- [Microsoft Corporation](https://github.com/Microsoft)  
  For providing the `cl.exe` compiler used to test the library.
