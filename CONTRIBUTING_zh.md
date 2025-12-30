# CONTRIBUTING_zh.md - Veo 贡献指南
感谢你对 Veo 编程语言的关注与贡献！这份指南旨在统一贡献标准、降低协作成本，让所有参与者都能高效参与项目建设。

## 目录
1. [行为准则](#1-行为准则-code-of-conduct)
2. [贡献类型](#2-贡献类型)
3. [核心贡献流程](#3-核心贡献流程)
4. [开发规范](#4-开发规范)
5. [提交信息规范](#5-提交信息规范)
6. [PR 提交规范](#6-pr-提交规范)
7. [Issue 提交规范](#7-issue-提交规范)
8. [本地开发环境搭建](#8-本地开发环境搭建)
9. [测试要求](#9-测试要求)
10. [代码审查](#10-代码审查)
11. [致谢](#11-致谢)

## 1. 行为准则 (Code of Conduct)
参与 Veo 项目的所有贡献者需遵守以下准则：
- **尊重包容**：无论技术水平、背景、身份，均保持友好沟通，禁止人身攻击、歧视性言论。
- **聚焦问题**：讨论围绕技术本身，不偏离主题，对不同意见保持理性。
- **协作优先**：提交代码/文档前确保符合项目规范，主动配合代码审查反馈。
- **责任意识**：提交的代码需经过自测，文档需保证准确可运行，避免引入无意义的 Bug。

## 2. 贡献类型
我们欢迎以下形式的贡献，无需提前申请，直接按流程参与即可：
| 贡献类型               | 说明                                                                 |
|------------------------|----------------------------------------------------------------------|
| 🐞 Bug 报告            | 发现代码/文档/示例中的错误，按规范提交 Issue。                        |
| ✨ 功能请求            | 提出新功能/优化建议，需说明使用场景、核心价值。                      |
| 💻 代码贡献            | 修复 Bug、实现新功能、优化性能、补充测试用例。                      |
| 📝 文档改进            | 完善 README/语法文档/注释，补充示例，修正翻译错误（中英双语）。       |
| 🧪 测试补充            | 为核心模块添加单元测试/集成测试，提升代码覆盖率。                    |
| 🎯 生态建设            | 开发 Veo 插件、示例项目、教程，或推广 Veo 社区。                    |

## 3. 核心贡献流程
### 3.1 通用流程（所有贡献类型）
1. **搜索现有 Issue/PR**：先在 [Issues](https://github.com/Ricardo-shuo-liu/veo/issues) 中搜索相关内容，避免重复提交。
2. **创建 Issue（可选但推荐）**：
   - 非紧急的 Bug/功能请求，先创建 Issue 讨论方案，达成共识后再开发。
   - 小修小补（如文档错别字、代码格式）可直接提交 PR。
3. **Fork 仓库**：点击 GitHub 仓库右上角「Fork」按钮，将仓库克隆到自己账号。
4. **本地开发**：
   ```bash
   # 克隆自己的 Fork 仓库
   git clone https://github.com/your-username/veo.git
   cd veo

   # 添加上游仓库（同步最新代码）
   git remote add upstream https://github.com/your-username/veo.git

   # 创建功能分支（禁止直接在 main 分支开发）
   git checkout -b [type]/[issue-id]-[short-description]
   # 示例：git checkout -b fix/123-main-function-compile-error
   ```
5. **提交变更**：遵循 [提交信息规范](#5-提交信息规范) 提交代码/文档。
6. **同步上游代码**：提交 PR 前，同步上游 main 分支，解决冲突：
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```
7. **推送分支并创建 PR**：
   ```bash
   git push origin [your-branch-name]
   ```
   访问 GitHub 仓库，点击「Compare & pull request」按钮，按规范填写 PR 信息。
8. **配合代码审查**：根据维护者的反馈修改代码，直到 PR 被合并。

### 3.2 特殊说明
- 首次贡献者可优先选择标注 `good first issue` 的 Issue，降低参与门槛。
- 大型功能（如 JIT 编译、FFI 扩展）建议先创建「RFC Issue」，详细说明设计方案，避免重复开发。

## 4. 开发规范
### 4.1 代码规范
Veo 项目包含两部分代码，需分别遵循对应规范：
#### 4.1.1 编译器代码（Rust）
- 遵循 [Rust 官方风格指南](https://doc.rust-lang.org/style-guide/)，使用 `cargo fmt` 格式化代码。
- 命名规范：
  - 结构体/枚举：PascalCase（如 `VeoParser`、`ExecutionMode`）；
  - 函数/变量：snake_case（如 `parse_veo_syntax`、`compiled_mode`）；
  - 常量：SCREAMING_SNAKE_CASE（如 `MAX_STACK_SIZE`）。
- 注释要求：
  - 公共函数/结构体必须添加文档注释（`///`），说明功能、参数、返回值；
  - 复杂逻辑需添加行内注释，解释设计思路。

#### 4.1.2 Veo 标准库/示例代码（Veo 语言）
- 遵循 Veo 语法文档中的规范，保持风格统一：
  - 缩进：4 个空格（禁止使用 Tab）；
  - 命名规范：
    - 函数/变量：snake_case（如 `process_strong_list`、`weak_dict`）；
    - 类/命名空间：PascalCase（如 `DataProcessor`、`Collection`）；
    - 常量：SCREAMING_SNAKE_CASE（如 `MAX_DICT_LENGTH`）。
  - 类型规范：
    - 公共 API 优先使用静态类型声明（如 `int[5] arr`、`list[4] mixed`），明确强弱类型；
    - 内部临时变量可使用动态类型（`let`），但需添加类型注释（如 `let weak_list: list[] = []`）。
  - 注释要求：
    - 公共函数/类必须添加文档注释（`#` 单行注释块）；
    - 复合类型操作需说明内存安全注意事项（如所有权、借用规则）。

### 4.2 文档规范
- 格式：统一使用 Markdown，遵循 [CommonMark](https://commonmark.org/) 标准；
- 语言：
  - 核心文档（README、SUMMARY、语法文档）需提供中英双语，保持内容一致；
  - 注释/代码示例优先使用英文（适配国际社区），中文作为补充；
- 示例：所有代码示例必须可运行，标注执行模式（编译/解释）、预期输出；
- 结构：新增文档需加入 `docs/SUMMARY.md` 导航，保持目录层级清晰。

## 5. 提交信息规范
采用 [Conventional Commits](https://www.conventionalcommits.org/) 格式，确保提交记录清晰可追溯：
```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

### 5.1 类型（type）
| 类型       | 说明                                                                 |
|------------|----------------------------------------------------------------------|
| feat       | 新增功能（如「新增弱元组扩容功能」）|
| fix        | 修复 Bug（如「修复强数组越界编译错误」）|
| docs       | 文档修改（如「补充复合类型内存安全说明」）|
| style      | 代码格式调整（无逻辑变更，如「格式化 Rust 编译器代码」）|
| refactor   | 代码重构（无功能新增/修复，如「重构命名空间调用逻辑」）|
| test       | 添加/修改测试用例（如「补充强列表单元测试」）|
| chore      | 构建/工具链调整（如「升级 Rust 依赖版本」）|
| perf       | 性能优化（如「优化弱字典内存分配逻辑」）|

### 5.2 作用域（scope）
可选，说明修改的模块，如：
- compiler（编译器核心）
- stdlib（标准库）
- docs（文档）
- examples（示例代码）
- cli（命令行工具）

### 5.3 示例
```
feat(stdlib): add weak tuple append method
fix(compiler): resolve strong list length check error
docs(zh): update memory safety chapter for composite types
```

## 6. PR 提交规范
### 6.1 PR 标题
与提交信息格式一致，示例：`feat(stdlib): support async processing for weak list`

### 6.2 PR 描述
需包含以下内容：
1. **变更目的**：说明修改的原因、解决的问题（关联 Issue 需标注，如 `Fixes #123`）；
2. **变更内容**：简要说明修改的核心逻辑、新增/删除的 API；
3. **测试验证**：说明已完成的测试（如单元测试、手动测试），附测试结果；
4. **注意事项**：（可选）兼容性影响、性能变化、文档更新情况。

### 6.3 其他要求
- 单个 PR 聚焦一个问题，避免「大杂烩」式提交；
- 确保 CI 检查通过（代码格式化、测试用例、编译验证）；
- 若修改涉及用户可见的功能，需同步更新文档和示例。

## 7. Issue 提交规范
### 7.1 Bug 报告模板
提交 Bug 时，请按以下格式填写：
```
### 问题描述
清晰简洁地描述 Bug 现象（如「强字典超出最大长度时未抛出错误」）。

### 复现步骤
1. 创建文件 `test.vo`，内容如下：
   ```veo
   dict[2] d = {"a":1, "b":2, "c":3}
   ```
2. 执行命令：`veo test.vo`
3. 观察现象：无报错，字典被错误扩容。

### 预期行为
应抛出「强字典超出最大长度（2）」的编译错误。

### 环境信息
- Veo 版本：v0.1.0
- 操作系统：macOS 14.0 / Ubuntu 22.04
- 复现概率：100%

### 补充信息
（可选）相关日志、截图、调试信息等。
```

### 7.2 功能请求模板
提交功能请求时，请按以下格式填写：
```
### 功能描述
清晰简洁地描述功能（如「为强列表添加批量修改元素的方法」）。

### 使用场景
说明该功能的适用场景（如「批量更新配置列表，减少循环调用开销」）。

### 设计思路
（可选）提出你的实现思路，或参考其他语言的实现方式。

### 优先级
（可选）该功能的紧急程度：高/中/低。
```

### 7.3 RFC Issue 模板（大型功能）
对于影响架构的大型功能（如 JIT 编译、FFI），需提交 RFC Issue：
```
### 功能名称
[RFC] 支持 C 语言 FFI 调用

### 背景与动机
说明为什么需要该功能，解决什么核心问题。

### 核心设计
1. 接口设计：定义 Veo 调用 C 函数的语法（如 `unsafe extern "C" fn xxx()`）；
2. 内存安全：说明如何适配 Veo 的所有权模型；
3. 兼容性：支持的 C 版本、编译选项。

### 实现计划
1. 阶段 1：实现基础类型（int/float）的 FFI 调用；
2. 阶段 2：支持复合类型（数组/列表）的 FFI 调用；
3. 阶段 3：添加测试用例和文档。

### 潜在风险
（可选）性能影响、安全风险、兼容性问题。
```

## 8. 本地开发环境搭建
### 8.1 基础依赖
- LLVM 16.0+
- Rust 1.70+
- Git 2.30+
- Python 3.8+（用于测试脚本）

### 8.2 完整搭建步骤
```bash
# 1. 安装系统依赖（以 Ubuntu 为例）
sudo apt update && sudo apt install -y llvm-16 clang-16 libllvm16 llvm-16-dev

# 2. 设置 LLVM 环境变量
export LLVM_SYS_160_PREFIX=/usr/lib/llvm-16

# 3. 克隆仓库（Fork 后替换为自己的地址）
git clone https://github.com/your-username/veo.git
cd veo

# 4. 安装 Rust 工具链（若未安装）
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env

# 5. 构建编译器
cargo build --release

# 6. 验证构建
./target/release/veo --version
# 预期输出：veo 0.1.0 (commit-hash)

# 7. 安装预提交钩子（可选，自动格式化代码）
pip install pre-commit
pre-commit install
```

### 8.3 调试模式
```bash
# 构建调试版本
cargo build

# 运行调试版本（带日志）
RUST_LOG=debug ./target/debug/veo test.vo
```

## 9. 测试要求
### 9.1 测试类型
- **单元测试**：覆盖核心函数/方法（如编译器解析逻辑、标准库函数）；
- **集成测试**：覆盖端到端场景（如编译模式/解释模式执行、命名空间调用）；
- **文档测试**：确保文档中的代码示例可运行（使用 `veo test docs/examples/*.vo`）。

### 9.2 测试执行
```bash
# 运行所有测试
cargo test

# 运行指定模块测试
cargo test test_strong_list

# 运行文档示例测试
veo test docs/en/syntax/examples.md
```

### 9.3 测试覆盖率
新增代码需保证测试覆盖率不低于 80%，可通过以下命令查看覆盖率：
```bash
# 安装 cargo-tarpaulin
cargo install cargo-tarpaulin

# 生成覆盖率报告
cargo tarpaulin --out html
```

## 10. 代码审查
### 10.1 审查标准
- 功能正确性：符合 Issue/PR 描述，无逻辑错误；
- 代码质量：遵循开发规范，无冗余/低效代码；
- 内存安全：符合 Veo 内存模型，无内存泄漏/数据竞争；
- 测试覆盖：新增功能需配套测试用例；
- 文档完整：用户可见的变更需同步更新文档。

### 10.2 反馈方式
- 小问题（如格式、注释）：直接在 PR 中提交代码建议；
- 大问题（如设计思路）：在 PR 评论中详细说明，给出改进方向；
- 需多次修改的 PR：标注「request changes」，待修改后重新审查。

## 11. 致谢
所有符合规范的贡献者将被列入仓库的 `AUTHORS.md` 文件（可自愿选择是否加入），我们会定期在社区中感谢核心贡献者。

如果你有任何疑问，可通过以下方式沟通：
- GitHub Discussions：https://github.com/Ricardo-shuo-liu/veo/discussions
- 邮件：contributors@veo-lang.org

再次感谢你的贡献，让 Veo 变得更好！

Veo 核心团队  
2025