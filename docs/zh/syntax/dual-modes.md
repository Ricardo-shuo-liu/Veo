# Veo 双模式执行
Veo 支持**编译模式**（生成二进制文件）与**解释模式**（脚本直接执行）自动切换，模式仅由程序入口逻辑决定，与文件后缀、变量声明方式无关。

## 一、执行模式规则
| 模式       | 触发条件                          | 输出结果                | 适用场景                |
|------------|-----------------------------------|-------------------------|-------------------------|
| 编译模式   | 存在顶层无缩进的`main`函数        | 原生二进制可执行文件    | 生产级部署上线          |
| 解释模式   | 无`main`函数 + 包含`if __name__ == "__main__"` | 不生成二进制文件（直接运行） | 快速原型开发/脚本编写   |

## 二、编译模式（生成二进制文件）
### 2.1 语法示例（含复合类型）
```veo
# 静态/动态混合类型 + 复合类型（编译模式）
from test import .
using test::print_

int version = 1  # 静态基础类型
let mode: str[10] = "compiled"  # 动态强类型字符串数组（长度10）

# 静态复合类型（精准内存控制）
int[5] strong_arr = [1, 2, 3, 4, 5]  # 强类型数组（固定长度）
list[4] strong_list = [1, "a", 3.14, true]  # 强类型列表（固定长度）
dict[3] strong_dict = {"name": "Veo", "age": 3, "stable": true}  # 强类型字典（最大长度3）

# 顶层无缩进main函数（触发编译流程）
def main():
    # 命名空间调用
    print_<a>(f"{mode} mode running, version: {version}")
    
    # 强类型数组操作（固定长度）
    let sum = 0
    for item in strong_arr:
        sum += item
    print(f"Strong array sum: {sum}")  # 输出：15
    
    # 强类型列表操作（支持任意类型）
    let int_count = 0
    for item in strong_list:
        if type(item) == int:
            int_count += 1
    print(f"Int elements in strong list: {int_count}")  # 输出：2
    
    # 强类型字典操作（固定最大长度）
    print(f"Strong dict: {strong_dict}")  # 输出：{"name":"Veo", "age":3, "stable":true}
```

### 2.2 执行命令
```bash
# 编译生成二进制文件（自动生成可执行文件）
veo test.vo

# 运行二进制文件
./test
```

### 2.3 编译模式核心特性
1. 编译为原生二进制文件，性能达到最优
2. 严格的静态类型校验，编译阶段即可捕获复合类型错误
3. 通过强复合类型实现精准内存控制，无动态扩容的内存损耗
4. 专为生产环境部署、高性能业务场景优化

## 三、解释模式（脚本直接执行）
### 3.1 语法示例（含复合类型）
```veo
# 静态/动态混合类型 + 复合类型（解释模式）
from test import .
using test::print_

str[10] msg = "interpreted"  # 静态强类型字符串数组（长度10）
let weak_list: list[] = [123.45, "Veo", true]  # 动态弱类型列表（灵活扩容）

# 动态复合类型（类Python灵活特性）
let weak_tuple: tuple[] = (1, "version", 1.0)  # 弱类型元组（动态长度，不可修改元素）
let weak_dict: dict[] = {"env": "dev", "log_level": "info"}  # 弱类型字典（动态长度）

# 脚本入口（触发解释执行，无main函数）
if __name__ == "__main__":
    # 命名空间调用
    print_<a>(f"{msg} mode running, list: {weak_list}")
    
    # 弱类型列表修改（支持动态扩容）
    weak_list.append(99.99)
    print(f"Updated weak list: {weak_list}")  # 输出：[123.45, "Veo", true, 99.99]
    
    # 弱类型元组操作（元素不可变，支持整体扩容）
    weak_tuple.append(("build", 2024))
    print(f"Weak tuple: {weak_tuple}")  # 输出：(1, "version", 1.0, ("build", 2024))
    
    # 弱类型字典操作（类Python灵活特性）
    weak_dict["debug"] = true
    print(f"Weak dict: {weak_dict}")  # 输出：{"env":"dev", "log_level":"info", "debug":true}
    
    # 动态类型切换（let声明支持类型变更）
    weak_list = "switched to string"
    print(f"Type switched: {weak_list}")  # 输出：switched to string
```

### 3.2 执行命令
```bash
# 直接解释执行（不生成二进制文件）
veo test.vo
```

### 3.3 解释模式核心特性
1. 无需编译步骤，开发迭代效率更高
2. 灵活的动态类型机制，let声明支持复合类型自由切换
3. 弱复合类型具备类Python的灵活特性，支持动态扩容
4. 专为快速原型开发、短脚本编写场景优化

## 四、核心注意事项
1. 执行模式**仅由**程序入口逻辑决定（`main`函数 / `__name__`入口），变量声明方式（静态/动态）、复合类型属性（强/弱）均不影响模式切换
2. 两种模式均完整支持数组、列表、元组、字典，语法与使用行为完全一致
3. 强复合类型（固定长度）用于精准内存控制，弱复合类型（动态长度）用于灵活开发
4. 无需区分文件后缀，所有Veo文件统一使用`.vo`后缀

---
