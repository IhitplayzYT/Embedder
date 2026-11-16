
# Embedder

A lightweight C utility for converting binary data into C or Assembly array representations. Embedder reads binary data from standard input and outputs it as hex-encoded arrays, making it easy to embed binary files directly into your source code.

## Why Embedder?

When developing low-level applications, you often need to embed binary resources—such as images, fonts, configuration files, or even small executables—directly into your source code.
Embedder simplifies this process by providing a clean, efficient way to convert any binary data into the appropriate format for C or Assembly inclusion.

## Features

- **Dual Output Modes**: Generate C arrays or Assembly `db` directives
- **Formatted Output**: Automatically wraps lines every 16 bytes for readability
- **Hex Encoding**: Converts binary data to clean hexadecimal representation

## Building

### Prerequisites

- GCC
- Make
- Standard C library

### Compilation

```bash
make
```

This will:
- Clear the terminal
- Remove old build artifacts
- Compile `Embedder.c` with optimization flags
- Link the executable as `Embedder`

### Clean Build Artifacts

```bash
make clean
```

## Usage

### Basic Syntax

```bash
./Embedder [OPTIONS] <identifier>
```

Read binary data from stdin and output it as a hex-encoded array named `<identifier>`.

### Options

- `-h, --help`: Display usage information
- `-a`: Output in Assembly format (default: C format)

https://github.com/user-attachments/assets/54a62294-395d-480d-8520-dcb361aff612

### Examples

#### Example 1: Embed Text as C Array

```bash
echo "Hello World" | ./Embedder greeting
```

**Output:**
```c
unsigned char greeting[] =
    "\x48\x65\x6c\x6c\x6f\x20\x57\x6f\x72\x6c\x64\x0a";
```

#### Example 2: Embed Binary File as C Array

```bash
cat image.png | ./Embedder image_data
```

**Output:**
```c
unsigned char image_data[] =
    "\x89\x50\x4e\x47\x0d\x0a\x1a\x0a\x00\x00\x00\x0d\x49\x48\x44\x52"
    "\x00\x00\x00\x01\x00\x00\x00\x01\x08\x06\x00\x00\x00\x1f\x15\xc4"
    "\x89\x00\x00\x00\x0a\x49\x44\x41\x54\x78\x9c\x63\x00\x01\x00\x00"
    "\x05\x00\x01\x0d\x0a\x2d\xb4\x00\x00\x00\x00\x49\x45\x4e\x44\xae"
    "\x42\x60\x82";
```

#### Example 3: Embed Data as Assembly Array

```bash
echo "Test" | ./Embedder -a shellcode
```

**Output:**
```asm
shellcode:
    db  0x54,0x65,0x73,0x74,0x0a
```

#### Example 4: Embed Executable as Assembly

```bash
cat /bin/ls | ./Embedder -a ls_binary
```

**Output:**
```asm
ls_binary:
    db  0x7f,0x45,0x4c,0x46,0x02,0x01,0x01,0x00,0x00,0x00,0x00,0x00
    db  0x00,0x00,0x00,0x00,0x03,0x00,0x3e,0x00,0x01,0x00,0x00,0x00
    db  0x30,0x1a,0x00,0x00,0x00,0x00,0x00,0x00,0x40,0x00,0x00,0x00
    ...
```

#### Example 5: Use in Makefile

```makefile
resources.h: logo.png
    cat logo.png | ./Embedder logo_data > resources.h
```

```c
#include "resources.h"

// logo_data is now available as unsigned char array
```

## Output Format Details

### C Format (Default)

- Generates a `unsigned char` array declaration
- Uses `\x` escape sequences for hex values
- Wraps lines every 16 bytes with proper string concatenation
- Terminates with `;\n`

### Assembly Format (-a flag)

- Generates a label with `db` directives
- Uses `0x` prefix for hex values
- Separates values with commas
- Wraps lines every 16 bytes with new `db` directive
- Terminates with newline

## Technical Details

- **Input**: Binary data via stdin (file descriptor 0)
- **Output**: Formatted text via stdout (file descriptor 1)


## Contributing

Contributions are welcome! The codebase is intentionally simple and well-documented for easy modification.

## Author

Created as a utility tool for binary data embedding in low-level programming projects.
