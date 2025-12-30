### Readme.md（带中文版本跳转）
# Veo
[🇨🇳 中文版本](README_zh.md)

A groundbreaking dual-mode programming language. Elegant human-centric syntax, full low-level control, native memory safety. Exclusive isolated namespace/non-namespace system, auto-switch compiled binary (main) & interpreted scripting (__name__) modes, unify rapid dev & high-performance production in one toolchain.

## Key Features
### 🚀 Dual-Mode Execution
- **Compiled Mode** (Binary): Triggered by a top-level `main` function, compiles to native binary for high-performance production deployment.
- **Interpreted Mode** (Script): Triggered by `if __name__ == "__main__"` (no `main` function), runs as a script for rapid prototyping (no binary generation).
- No file suffix distinction — execution mode is determined solely by entry logic.

### 🔒 Isolated Dual-System Architecture
- **Namespace System**: Explicit `using <pkg>::<ns>` import + `::<>` call syntax, for structured project development (strict scoping, no naming conflicts).
- **Non-Namespace System**: Python-like `from...import` + `.` call syntax, for lightweight scripting (no redundant constraints).
- Two systems are fully isolated (support duplicate element names) — no interference between project-level and script-level code.

### ✨ Elegant & Safe Syntax
- **Human-Centric Syntax**: Python-like readability (indentation-based, minimal boilerplate), low learning curve for all developers.
- **Native Memory Safety**: Built-in ownership/borrowing model (no memory leaks, data races) — balance safety and low-level control.
- **No Redundant Imports**: Ban raw `import` syntax, unify with `from...import` (lazy loading of metadata only, minimal memory footprint, no global pollution).

### ⚡ Full Low-Level Control
- Compatible with C/C++ low-level operations (via `unsafe` blocks) — no compromise on performance for system-level development.
- No GIL (Global Interpreter Lock) — support high-concurrency asynchronous programming (`async/await`).

## Quick Start
### Prerequisites
- LLVM 16.0+
- Rust 1.70+ (for compiler development)

### Install
```bash
# Clone the repository
git clone https://github.com/your-username/veo.git
cd veo

# Build the compiler
cargo build --release

# Add to PATH (temporary)
export PATH=$PATH:./target/release
```

### Run Examples
#### 1. Interpreted Mode (Script)
Create `hello.vo`:
```veo
# hello.vo (no main function → interpreted mode)
from veo.std import .

if __name__ == "__main__":
    veo.std.print<log>("Hello, Veo!")  # Non-namespace call
```
Run:
```bash
veo hello.vo
# Output: Hello, Veo!
```

#### 2. Compiled Mode (Binary)
Create `app.vo`:
```veo
# app.vo (with main function → compiled mode)
from veo.std import .
using veo::math

def main():
    let sum = veo.std.calc<add>(1, 2)  # Namespace call
    veo.std.print<log>(f"Sum: {sum}")
```
Run:
```bash
# Compile to binary (auto-generated)
veo app.vo

# Execute the binary
./app
# Output: Sum: 3
```

## Core Philosophy
Veo is built to redefine the balance between **ease of use** and **power**. We believe a modern programming language should:
1. Be approachable for beginners (like Python) — no steep learning curve.
2. Be powerful enough for production (like C++) — full control over low-level details.
3. Be safe by default (like Rust) — eliminate common memory errors.
4. Adapt to all workflows — no need to switch languages for scripting vs. production.

## Roadmap
### Short-Term (v1.0)
- Complete syntax parser & dual-mode execution core
- Basic standard library (IO, math, collections)
- Error handling system & friendly error prompts

### Mid-Term (v1.5)
- Veo Package Manager (`veopm`)
- FFI (Foreign Function Interface) for C/C++/Python/Rust
- Async/await concurrency support

### Long-Term (v2.0)
- IDE integration (VS Code/CLion plugins)
- Performance optimization (JIT compilation)
- Ecosystem expansion (libraries for web, ML, system dev)

## Contributing
We welcome contributions of all kinds — bug reports, feature requests, code PRs, documentation improvements.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m "Add your feature"`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## License
Veo is licensed under the [MIT License](LICENSE).
---