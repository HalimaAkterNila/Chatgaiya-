# Chatgaiya++ Compiler

A small educational compiler written in C++. It uses **Flex** for lexical analysis and a **recursive-descent parser** for syntax analysis, then generates Python 3 source code.

## Windows build

The simplest native Windows route is:

1. Install a MinGW-w64 C++17 compiler and add its `bin` directory to `PATH`.
2. Install **WinFlexBison** and add the directory containing `win_flex.exe` to `PATH`.
3. Open Command Prompt in this project folder and run:

   ```bat
   build_windows.bat
   ```

The script creates `chatgaiya.exe`. It accepts either `win_flex` or `flex` on `PATH`.

Alternatively, install **MSYS2 UCRT64** from [msys2.org](https://www.msys2.org/), open its UCRT64 terminal, and install the tools:

```sh
pacman -Syu
pacman -S --needed mingw-w64-ucrt-x86_64-gcc flex make
```

Then build from the UCRT64 terminal with `make`. This project does not require Bison.

## Build on Linux / macOS

Install a C++17 compiler, Flex, Make, and Python 3, then run:

```sh
make
```

## Compile a program

```bat
chatgaiya.exe examples\program.cg
python examples\program.py
```

To choose the output path:

```bat
chatgaiya.exe examples\program.cg -o output.py
python output.py
```

The generated Python file is written beside the input by default. Python 3 is only needed to run the generated program and smoke tests.

## Test

Windows:

```bat
python tests\run_tests.py
```

Linux / macOS:

```sh
make test
```

## Project files

| File | Purpose |
|---|---|
| `lexer.l` | Flex token rules and source locations |
| `tokens.hpp` | Shared token definitions for the scanner and parser |
| `parser.cpp`, `parser.hpp` | Hand-written recursive-descent parser and precedence handling |
| `compiler.hpp`, `compiler.cpp` | Abstract syntax tree and language types |
| `semantic_analysis.*`, `symbol_table.*` | Type checks, declaration checks, and symbol table |
| `code_generation.*` | Python code generator |
| `main.cpp` | Compiler driver |
| `build_windows.bat`, `Makefile` | Windows and Unix-like build entry points |
| `examples/`, `tests/` | Sample program and smoke tests |

## Language overview

Types are `ongko` (integer), `dhoshomik` (float), `kotha` (string), and `ho` (boolean). Boolean literals are `hasa` and `misa`. Statements include declarations, assignment, `ko(expr);` output, `lo()` input, `zodi (...) { ... }` conditionals with optional `noile`/`tokon` else branches, `zotokkhon (...) { ... }` loops, `tham;` break, and `chol;` continue. Statements end with semicolons. Expressions support arithmetic, comparisons, logical and bitwise operators, shifts, unary operators, parentheses, and string indexing. `ki_type(expr)` returns a type name at compile time.

The parser is recursive descent (not Bison); the lexer is authored in Flex. The compiler itself does not depend on Linux APIs or shell commands at runtime.
