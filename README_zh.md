# Veo
[🇬🇧 English Version](README.md)

一款开创性的双模编程语言。兼具优雅的人性化语法、完整的底层控制能力与原生内存安全性。独创隔离式命名空间/非命名空间双系统，自动切换编译二进制（main）与解释脚本（__name__）模式，将快速开发与高性能生产部署统一在单一工具链中。

## 核心特性
### 🚀 双模执行
- **编译模式（二进制）**：由顶级 `main` 函数触发，编译为原生二进制文件，适配高性能生产环境部署。
- **解释模式（脚本）**：由 `if __name__ == "__main__"` 触发（无 `main` 函数），以脚本形式运行，适配快速原型开发（无二进制生成）。
- 无文件后缀区分——执行模式仅由入口逻辑决定。

### 🔒 隔离式双系统架构
- **命名空间系统**：显式 `using <pkg>::<ns>` 导入 + `::<>` 调用语法，适配结构化项目开发（严格作用域，无命名冲突）。
- **非命名空间系统**：类 Python 的 `from...import` + `.` 调用语法，适配轻量级脚本编写（无冗余约束）。
- 两套系统完全隔离（支持元素同名）——项目级与脚本级代码无任何干扰。

### ✨ 优雅且安全的语法
- **人性化语法**：类 Python 的易读性（基于缩进，极简样板代码），所有开发者均可快速上手，学习曲线极低。
- **原生内存安全**：内置所有权/借用模型（无内存泄漏、数据竞争）——平衡安全性与底层控制能力。
- **无冗余导入**：禁用原生 `import` 语法，统一为 `from...import`（仅懒加载元数据，内存占用极小，无全局污染）。

### ⚡ 完整的底层控制能力
- 兼容 C/C++ 底层操作（通过 `unsafe` 块）——面向系统级开发时不牺牲性能。
- 无全局解释器锁（GIL）——支持高并发异步编程（`async/await`）。

## 快速开始
### 前置条件
- LLVM 16.0+
- Rust 1.70+（用于编译器开发）

### 安装
```bash
# 克隆仓库
git clone https://github.com/your-username/veo.git
cd veo

# 构建编译器
cargo build --release

# 临时添加到 PATH
export PATH=$PATH:./target/release
```

### 运行示例
#### 1. 解释模式（脚本）
创建 `hello.vo`：
```veo
# hello.vo（无 main 函数 → 解释模式）
from veo.std import .

if __name__ == "__main__":
    veo.std.print<log>("你好，Veo！")  # 非命名空间调用
```
运行：
```bash
veo hello.vo
# 输出：你好，Veo！
```

#### 2. 编译模式（二进制）
创建 `app.vo`：
```veo
# app.vo（含 main 函数 → 编译模式）
from veo.std import .
using veo::math

def main():
    let sum = veo.std.calc<add>(1, 2)  # 命名空间调用
    veo.std.print<log>(f"求和结果：{sum}")
```
运行：
```bash
# 编译为二进制文件（自动生成）
veo app.vo

# 执行二进制文件
./app
# 输出：求和结果：3
```

## 核心设计理念
Veo 旨在重新定义「易用性」与「强大性」的平衡。我们认为现代编程语言应具备以下特质：
1. 对初学者友好（如 Python）——无陡峭的学习曲线。
2. 满足生产环境需求（如 C++）——完全掌控底层细节。
3. 默认安全（如 Rust）——消除常见内存错误。
4. 适配所有工作流——无需为脚本编写与生产部署切换不同语言。

## 路线图
### 短期（v1.0）
- 完整的语法解析器与双模执行核心
- 基础标准库（IO、数学、集合）
- 错误处理系统与友好的错误提示

### 中期（v1.5）
- Veo 包管理器（`veopm`）
- 跨语言外部函数接口（FFI）——支持 C/C++/Python/Rust
- 异步/等待（async/await）并发支持

### 长期（v2.0）
- IDE 集成（VS Code/CLion 插件）
- 性能优化（JIT 编译）
- 生态扩展（Web、机器学习、系统开发类库）

## 贡献指南
我们欢迎各类形式的贡献——Bug 报告、功能请求、代码 PR、文档优化均可。

1. Fork 本仓库
2. 创建功能分支（`git checkout -b feature/your-feature`）
3. 提交变更（`git commit -m "Add your feature"`）
4. 推送到分支（`git push origin feature/your-feature`）
5. 打开 Pull Request

详细规范请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 开源协议
Veo 基于 [MIT 开源协议](LICENSE) 授权。

---