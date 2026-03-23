# ferrumc

The **Ferrum compiler** — takes `.fe` source files and produces native binaries via LLVM 18.

**Syntax:** C &nbsp;|&nbsp; **Safety:** compile-time checked &nbsp;|&nbsp; **License:** GNU GPL v3

---

## What it does

`ferrumc` compiles Ferrum source code (`.fe` files) through five stages:

```
.fe source
    │
    ▼
 Lexer          tokenizes C-style syntax, lifetimes 'a, #include → import
    │
    ▼
 Parser         builds AST: functions, structs, generics, unsafe blocks
    │
    ▼
 TypeChecker    infers types, validates generics, resolves C headers
    │
    ▼
 BorrowChecker  enforces ownership, moves, borrows, lifetimes, unsafe
    │
    ▼
 Codegen        emits LLVM IR, inserts automatic free() per scope
    │
    ▼
 llc + gcc      produces a native binary
```

---

## Requirements

- `g++` with C++20
- LLVM 18 (`llvm-config`, `llc`)
- `gcc` (for linking)

```bash
# Ubuntu / Debian
sudo apt install llvm-18 llvm-18-dev gcc g++ build-essential
```

---

## Build

```bash
git clone https://github.com/Ferrum-Language/ferrumc.git
cd ferrumc
bash build.sh
```

This produces `build/ferrumc`.

---

## Usage

```bash
# Compile a .fe file to a binary
./build/ferrumc file.fe -o my_program

# Print the generated LLVM IR instead of compiling
./build/ferrumc file.fe --emit-ir

# Run
./my_program
```

---

## Compile errors

| Code | Meaning |
|------|---------|
| `E0382` | Use of variable after `move()` |
| `E0502` | Borrow conflict (mutable + immutable at the same time) |
| `E0505` | Use of pointer after free |
| `E0596` | Mutation while a borrow is active |
| `E0597` | Borrow outlives the value it borrows from |
| `E0133` | `unsafe` pointer outside an `unsafe {}` block |

---

## Source layout

```
ferrumc/
├── include/ferrum/
│   ├── Token.h          # Token types
│   ├── Lexer.h          # Lexer interface
│   ├── AST.h            # Abstract syntax tree nodes
│   ├── Parser.h         # Parser interface
│   ├── TypeChecker.h    # Type system (Sema)
│   ├── BorrowChecker.h  # Ownership and borrow checker
│   └── Codegen.h        # LLVM IR generator
├── src/
│   ├── lexer/Lexer.cpp
│   ├── parser/Parser.cpp
│   ├── sema/TypeChecker.cpp
│   ├── borrow/BorrowChecker.cpp
│   ├── codegen/Codegen.cpp
│   └── driver/main.cpp  # Entry point
├── tests/               # .fe test programs and C++ unit tests
├── CMakeLists.txt
└── build.sh
```

---

## Running the tests

```bash
cd build
ctest --output-on-failure
```

---

## License

GNU General Public License v3.0 — see [LICENSE](LICENSE).

---

## Related

- [Ferrum-Language/Ferrum](https://github.com/Ferrum-Language/Ferrum) — language spec, docs, full repo
- [Ferrum-Language/ferrum-examples](https://github.com/Ferrum-Language/ferrum-examples) — example programs in Ferrum

- 
*Ferrum-language Compiler v0.3 — [Ferrum-Language](https://github.com/Ferrum-Language)*
