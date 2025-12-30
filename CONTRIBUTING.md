# CONTRIBUTING.md - Veo Contribution Guidelines
Thank you for your interest and contributions to the Veo programming language! This guide unifies contribution standards, reduces collaboration costs, and enables all participants to join the project efficiently.

👉 **Chinese Version / 中文版**: [CONTRIBUTING_zh.md](./CONTRIBUTING_zh.md)

## Table of Contents
1. [Code of Conduct](#1-code-of-conduct)
2. [Types of Contributions](#2-types-of-contributions)
3. [Core Contribution Workflow](#3-core-contribution-workflow)
4. [Development Standards](#4-development-standards)
5. [Commit Message Standards](#5-commit-message-standards)
6. [PR Submission Standards](#6-pr-submission-standards)
7. [Issue Submission Standards](#7-issue-submission-standards)
8. [Local Development Environment Setup](#8-local-development-environment-setup)
9. [Testing Requirements](#9-testing-requirements)
10. [Code Review](#10-code-review)
11. [Acknowledgements](#11-acknowledgements)

## 1. Code of Conduct
All contributors to the Veo project must abide by the following guidelines:
- **Respect & Inclusiveness**: Maintain friendly communication regardless of skill level, background or identity; no personal attacks or discriminatory language.
- **Issue-Focused Discussion**: Keep conversations technical and on-topic, and remain rational about differing opinions.
- **Collaboration First**: Ensure compliance with project standards before submitting code/docs, and actively respond to code review feedback.
- **Sense of Responsibility**: Self-test submitted code, ensure docs are accurate and runnable, and avoid introducing meaningless bugs.

## 2. Types of Contributions
We welcome contributions in the following forms—no prior application required, just follow the workflow to participate:
| Contribution Type       | Description                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| 🐞 Bug Reports          | Report errors in code/docs/examples and submit an Issue per standards.      |
| ✨ Feature Requests     | Propose new features/optimizations with clear usage scenarios and core value. |
| 💻 Code Contributions   | Fix bugs, implement new features, optimize performance, and add test cases. |
| 📝 Documentation Improvements | Improve README/syntax docs/comments, add examples, and correct translation errors (bilingual Chinese/English). |
| 🧪 Test Additions       | Add unit/integration tests for core modules to improve code coverage.       |
| 🎯 Ecosystem Building   | Develop Veo plugins, sample projects, tutorials, or promote the Veo community. |

## 3. Core Contribution Workflow
### 3.1 General Workflow (for all contribution types)
1. **Search Existing Issues/PRs**: First search for related content in [Issues](https://github.com/Ricardo-shuo-liu/veo/issues) to avoid duplicate submissions.
2. **Create an Issue (optional but recommended)**:
   - For non-urgent bugs/feature requests, create an Issue to discuss the solution first, then develop after reaching a consensus.
   - Minor fixes (e.g., doc typos, code formatting) can be submitted directly via PR.
3. **Fork the Repository**: Click the "Fork" button in the top right corner of the GitHub repository to clone it to your account.
4. **Local Development**:
   ```bash
   # Clone your forked repository
   git clone https://github.com/your-username/veo.git
   cd veo

   # Add upstream repository (sync latest code)
   git remote add upstream https://github.com/Ricardo-shuo-liu/veo.git

   # Create a feature branch (never develop directly on the main branch)
   git checkout -b [type]/[issue-id]-[short-description]
   # Example: git checkout -b fix/123-main-function-compile-error
   ```
5. **Commit Changes**: Submit code/docs following the [Commit Message Standards](#5-commit-message-standards).
6. **Sync Upstream Code**: Before submitting a PR, sync the upstream main branch and resolve conflicts:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```
7. **Push Branch & Create PR**:
   ```bash
   git push origin [your-branch-name]
   ```
   Visit the GitHub repository, click the "Compare & pull request" button, and fill in PR information per standards.
8. **Cooperate with Code Review**: Modify code based on maintainers' feedback until the PR is merged.

### 3.2 Special Notes
- First-time contributors can prioritize Issues labeled `good first issue` to lower participation barriers.
- For large features (e.g., JIT compilation, FFI extensions), it is recommended to create an **RFC Issue** to detail the design plan and avoid duplicate development.

## 4. Development Standards
### 4.1 Code Standards
The Veo project includes two parts of code, each following corresponding standards:
#### 4.1.1 Compiler Code (Rust)
- Comply with the [Rust Official Style Guide](https://doc.rust-lang.org/style-guide/), format code with `cargo fmt`.
- Naming Standards:
  - Structs/Enums: PascalCase (e.g., `VeoParser`, `ExecutionMode`);
  - Functions/Variables: snake_case (e.g., `parse_veo_syntax`, `compiled_mode`);
  - Constants: SCREAMING_SNAKE_CASE (e.g., `MAX_STACK_SIZE`).
- Comment Requirements:
  - Public functions/structs must include doc comments (`///`) describing functionality, parameters, and return values;
  - Add line comments for complex logic to explain design ideas.

#### 4.1.2 Veo Standard Library/Sample Code (Veo Language)
- Comply with standards in the Veo syntax documentation for consistent style:
  - Indentation: 4 spaces (tabs are prohibited);
  - Naming Standards:
    - Functions/Variables: snake_case (e.g., `process_strong_list`, `weak_dict`);
    - Classes/Namespaces: PascalCase (e.g., `DataProcessor`, `Collection`);
    - Constants: SCREAMING_SNAKE_CASE (e.g., `MAX_DICT_LENGTH`).
  - Type Standards:
    - Public APIs prioritize static type declarations (e.g., `int[5] arr`, `list[4] mixed`) to clarify strong/weak types;
    - Dynamic typing (`let`) can be used for internal temporary variables with type annotations (e.g., `let weak_list: list[] = []`).
  - Comment Requirements:
    - Public functions/classes must include doc comments (single-line `#` comment blocks);
    - Add memory safety notes for composite type operations (e.g., ownership, borrowing rules).

### 4.2 Documentation Standards
- Format: Unified Markdown, complying with the [CommonMark](https://commonmark.org/) standard;
- Language:
  - Core docs (README, SUMMARY, syntax docs) must provide bilingual Chinese/English content with consistency;
  - Comments/code examples prioritize English (for international community compatibility), with Chinese as a supplement;
- Examples: All code examples must be runnable, with execution mode (compiled/interpreted) and expected output marked;
- Structure: New docs must be added to the `docs/SUMMARY.md` navigation for a clear directory hierarchy.

## 5. Commit Message Standards
Adopt the [Conventional Commits](https://www.conventionalcommits.org/) format to ensure clear and traceable commit records:
```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

### 5.1 Type
| Type       | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| feat       | Add new features (e.g., "Add weak tuple append functionality")              |
| fix        | Fix bugs (e.g., "Fix strong array out-of-bounds compilation error")         |
| docs       | Modify documentation (e.g., "Add memory safety notes for composite types")  |
| style      | Adjust code formatting (no logical changes, e.g., "Format Rust compiler code") |
| refactor   | Refactor code (no new features/bug fixes, e.g., "Refactor namespace call logic") |
| test       | Add/modify test cases (e.g., "Add unit tests for strong lists")             |
| chore      | Adjust build/toolchain (e.g., "Upgrade Rust dependency version")            |
| perf       | Optimize performance (e.g., "Optimize weak dict memory allocation logic")    |

### 5.2 Scope
Optional, indicating the modified module, e.g.:
- compiler (core compiler)
- stdlib (standard library)
- docs (documentation)
- examples (sample code)
- cli (command-line tool)

### 5.3 Examples
```
feat(stdlib): add weak tuple append method
fix(compiler): resolve strong list length check error
docs(en): update memory safety chapter for composite types
```

## 6. PR Submission Standards
### 6.1 PR Title
Consistent with the commit message format, e.g.: `feat(stdlib): support async processing for weak list`

### 6.2 PR Description
Must include the following content:
1. **Purpose of Change**: Explain the reason for modification and the problem solved (label related Issues, e.g., `Fixes #123`);
2. **Content of Change**: Briefly describe the core logic of modification and newly added/deleted APIs;
3. **Test Verification**: Explain completed tests (e.g., unit tests, manual tests) and attach test results;
4. **Notes**: (Optional) Compatibility impact, performance changes, documentation update status.

### 6.3 Additional Requirements
- A single PR focuses on one issue; avoid "catch-all" submissions;
- Ensure CI checks pass (code formatting, test cases, compilation verification);
- If modifications involve user-visible features, synchronously update docs and examples.

## 7. Issue Submission Standards
### 7.1 Bug Report Template
Fill in the following format when submitting a bug:
```
### Problem Description
Clearly and concisely describe the bug (e.g., "No error thrown when strong dict exceeds max length").

### Reproduction Steps
1. Create file `test.vo` with the following content:
   ```veo
   dict[2] d = {"a":1, "b":2, "c":3}
   ```
2. Run command: `veo test.vo`
3. Observe behavior: No error is thrown, and the dict is incorrectly expanded.

### Expected Behavior
A compilation error of "Strong dict exceeds max length (2)" should be thrown.

### Environment Information
- Veo Version: v0.1.0
- Operating System: macOS 14.0 / Ubuntu 22.04
- Reproduction Probability: 100%

### Additional Information
(Optional) Related logs, screenshots, debugging information, etc.
```

### 7.2 Feature Request Template
Fill in the following format when submitting a feature request:
```
### Feature Description
Clearly and concisely describe the feature (e.g., "Add batch element modification method for strong lists").

### Usage Scenario
Explain the applicable scenarios of the feature (e.g., "Batch update configuration lists to reduce loop call overhead").

### Design Ideas
(Optional) Propose your implementation ideas or reference implementations from other languages.

### Priority
(Optional) Urgency of the feature: High/Medium/Low.
```

### 7.3 RFC Issue Template (Large Features)
For large architecture-impacting features (e.g., JIT compilation, FFI), submit an RFC Issue:
```
### Feature Name
[RFC] Support C language FFI calls

### Background & Motivation
Explain why the feature is needed and what core problems it solves.

### Core Design
1. Interface Design: Define syntax for Veo to call C functions (e.g., `unsafe extern "C" fn xxx()`);
2. Memory Safety: Explain how to adapt to Veo's ownership model;
3. Compatibility: Supported C versions and compilation options.

### Implementation Plan
1. Phase 1: Implement FFI calls for basic types (int/float);
2. Phase 2: Support FFI calls for composite types (arrays/lists);
3. Phase 3: Add test cases and documentation.

### Potential Risks
(Optional) Performance impact, security risks, compatibility issues.
```

## 8. Local Development Environment Setup
### 8.1 Basic Dependencies
- LLVM 16.0+
- Rust 1.70+
- Git 2.30+
- Python 3.8+ (for test scripts)

### 8.2 Complete Setup Steps
```bash
# 1. Install system dependencies (Ubuntu example)
sudo apt update && sudo apt install -y llvm-16 clang-16 libllvm16 llvm-16-dev

# 2. Set LLVM environment variables
export LLVM_SYS_160_PREFIX=/usr/lib/llvm-16

# 3. Clone the repository (replace with your forked address after forking)
git clone https://github.com/your-username/veo.git
cd veo

# 4. Install Rust toolchain (if not installed)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

# 5. Build the compiler
cargo build --release

# 6. Verify the build
./target/release/veo

# 7. Install pre-commit hooks (optional, auto-format code)
pip install pre-commit
pre-commit install
```

### 8.3 Debug Mode
```bash
# Build debug version
cargo build

# Run debug version (with logs)
RUST_LOG=debug ./target/debug/veo test.vo
```

## 9. Testing Requirements
### 9.1 Test Types
- **Unit Tests**: Cover core functions/methods (e.g., compiler parsing logic, standard library functions);
- **Integration Tests**: Cover end-to-end scenarios (e.g., compiled/interpreted mode execution, namespace calls);
- **Doc Tests**: Ensure code examples in docs are runnable (use `veo test docs/examples/*.vo`).

### 9.2 Test Execution
```bash
# Run all tests
cargo test

# Run tests for a specific module
cargo test test_strong_list

# Run doc example tests
veo test docs/en/syntax/examples.md
```

### 9.3 Test Coverage
New code must ensure a test coverage rate of **no less than 80%**. Check coverage with the following commands:
```bash
# Install cargo-tarpaulin
cargo install cargo-tarpaulin

# Generate coverage report
cargo tarpaulin --out html
```

## 10. Code Review
### 10.1 Review Standards
- Functional Correctness: Conform to Issue/PR description with no logical errors;
- Code Quality: Comply with development standards with no redundant/inefficient code;
- Memory Safety: Comply with the Veo memory model with no memory leaks/data races;
- Test Coverage: New features must be accompanied by test cases;
- Documentation Completeness: User-visible changes must be synced to docs.

### 10.2 Feedback Methods
- Minor issues (e.g., formatting, comments): Submit code suggestions directly in the PR;
- Major issues (e.g., design ideas): Explain in detail in PR comments and provide improvement directions;
- PRs requiring multiple modifications: Label with `request changes` and re-review after modifications.

## 11. Acknowledgements
All contributors who comply with the standards will be listed in the repository's `AUTHORS.md` file (opt-in), and we will regularly thank core contributors in the community.

If you have any questions, you can communicate via the following channels:
- GitHub Discussions: https://github.com/Ricardo-shuo-liu/veo/discussions
- Email: contributors@veo-lang.org

Thank you again for your contributions to make Veo better!

The Veo Core Team  
2025
