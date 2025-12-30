# Veo 命名空间与非命名空间系统
Veo 内置**两套完全隔离的系统**（命名空间/非命名空间）——支持同名元素共存且互不干扰，分别适配结构化项目开发与轻量级脚本编写场景。所有复合类型（数组、列表、元组、字典）均与两套系统完全兼容。

## 一、核心概念
| 系统类型       | 语法风格                     | 适用场景               | 核心约束                               |
|----------------|------------------------------|------------------------|----------------------------------------|
| 命名空间系统   | `using <pkg>::<ns>` + `::<>` | 结构化项目开发         | 必须通过`using`导入；调用需使用`::+<>`语法 |
| 非命名空间系统 | `from...import` + `.`        | 轻量级脚本编写         | 无冗余约束（与Python使用方式一致）      |

## 二、非命名空间系统（脚本编写）
### 2.1 导入语法
Veo 统一使用`from...import`完成导入（无原生`import`语法）：
```veo
# 加载整个包（仅加载元数据，独立副本）
from test import .

# 加载指定模块/文件
from test.a import b

# 加载指定元素（包含复合类型的函数/类/变量）
from test.a.b import process_strong_list
from test.a.b import User  # 包含复合类型成员的类
```

### 2.2 调用语法
使用`.`作为分隔符（与Python一致），复合类型使用方式保持统一：
```veo
# 加载test包
from test import .

# 调用带强类型列表参数的函数（非命名空间）
list[4] static_list = [1, "a", 3.14, true]
let res = test.math.process_strong_list(static_list)
print(res)  # 输出：2

# 调用包含复合类型成员的类（非命名空间）
list[] hobbies = ["reading", "coding"]
dict[5] info = {"city": "Beijing"}
test.model.User user = test.model.User("Alice", hobbies, info)
print(user.name)  # 输出：Alice

# 非命名空间下的弱类型字典操作（类Python风格）
test.data.config["debug"] = true
print(test.data.config)  # 输出：{"debug": true}
```

## 三、命名空间系统（项目开发）
### 3.1 命名空间定义
使用`namespace`定义命名空间（支持嵌套，完全兼容复合类型）：
```veo
# 包含复合类型的基础命名空间
namespace math {
    # 带强类型数组参数的函数
    public def square_strong_array(int[5] arr) -> int[]:
        let result: int[] = []
        for item in arr:
            result.append(item * item)
        return result
    
    # 强类型字典（固定最大长度，类Python键值对）
    public dict[2] constants = {"pi": 3.14, "e": 2.71}
    
    # 弱类型列表（动态长度，支持任意类型）
    public list[] mixed_utils = [100, "max", 0.0, false]
}

# 包含复合类型的嵌套命名空间
namespace test::io {
    public def read_weak_list(str path) -> list[]:
        return ["line1", 2, 3.14, true]  # 返回弱类型列表（支持任意类型）
}
```

### 3.2 命名空间导入
必须先加载包，再通过`using`导入命名空间：
```veo
# 步骤1：加载包含命名空间的包
from test import .

# 步骤2：导入命名空间
using test::math  # 从test包导入math命名空间
using test::io    # 导入嵌套命名空间test::io
```

### 3.3 命名空间调用语法
强制使用`::`分隔命名空间，`<>`指定目标元素（复合类型使用方式保持统一）：
```veo
# 前提：已加载test包 + using test::math

# 1. 调用带强类型数组参数的函数（命名空间）
int[5] arr = [1, 2, 3, 4, 5]
let squared = math<square_strong_array>(arr)
print(squared)  # 输出：[1, 4, 9, 16, 25]

# 2. 访问命名空间内的强类型字典（固定最大长度）
print(math<.>.constants["pi"])  # 输出：3.14
# math<.>.constants["phi"] = 1.618  # 错误：超过最大长度2

# 3. 修改命名空间内的弱类型列表（动态长度）
math<.>.mixed_utils.append("new_item")
print(math<.>.mixed_utils)  # 输出：[100, "max", 0.0, false, "new_item"]

# 4. 调用嵌套命名空间函数（返回弱类型列表）
let lines = test::io<read_weak_list>("./test.txt")
print(lines)  # 输出：["line1", 2, 3.14, true]
```

## 四、两套系统的隔离性
命名空间与非命名空间的元素可同名且互不干扰，包含复合类型元素在内：
```veo
from test import .
using test::math

# 非命名空间函数（与命名空间函数同名）
def square_strong_array(int[5] arr) -> int[]:
    let result: int[] = []
    for item in arr:
        result.append(item * 3)
    return result

def main():
    int[5] arr = [1, 2, 3, 4, 5]
    
    # 非命名空间调用（返回弱类型数组）
    let non_ns_result = square_strong_array(arr)
    print(non_ns_result)  # 输出：[3, 6, 9, 12, 15]
    
    # 命名空间调用（返回弱类型数组）
    let ns_result = math<square_strong_array>(arr)
    print(ns_result)  # 输出：[1, 4, 9, 16, 25]
```

## 五、核心规则
1. 跨系统无干扰（允许同名元素，包含复合类型元素）
2. 命名空间系统强制严格的作用域管控（避免大型项目中的命名冲突）
3. 非命名空间系统保留Python的简洁性（快速编写脚本）
4. 两套系统均完全支持数组/列表/元组/字典（强/弱属性表现一致）

---