Veo 是一款开创性的双模编程语言，采用人性化的语法设计。本文档涵盖核心基础语法规则。

## 1. 注释
Veo 支持多种注释风格，适配不同使用场景：
```veo
# 单行注释（Python 风格，推荐）
// 单行注释（C++ 风格，兼容）

'''
多行注释
支持换行
可跨多行编写
'''
```

## 2. 缩进与代码块
Veo 采用**缩进**界定代码块（同 Python），推荐使用 4 个空格（不支持花括号 `{}`）：
```veo
# 正确示例
if 1 > 0:
    print("缩进合法")
    if 2 > 1:
        print("嵌套缩进")

# 错误示例（缩进不一致）
if 1 > 0:
  print("2个空格")
    print("4个空格")  # 报错：缩进不统一
```

## 3. 保留关键字
以下关键字为系统保留，不可作为变量/函数/类名使用：
| 分类         | 关键字                                      |
|--------------|---------------------------------------------|
| 基础         | let, const, def, class, namespace, using    |
| 流程控制     | if, else if, else, for, while, break, continue |
| 访问控制     | public, private, protected                  |
| 执行模式     | main（顶级函数专用）|
| 其他         | return, async, await, unsafe                |

## 4. 变量与类型
Veo 支持两种变量声明方式（编译/解释模式下均适用）：
- **静态类型声明**（如 `int a`、`int[10] arr`、`list[5] mixed`）：变量绑定指定类型，修改类型需强制转换；
- **动态类型声明**（`let a`、`let b: list[5]`）：同 Python，变量类型随赋值自动变化。

### 核心定义：复合类型的强弱之分
| 类型属性 | 核心规则                          | 适用场景                  |
|----------|-----------------------------------|---------------------------|
| 强（Strong） | 长度固定（不可扩容/缩容）         | 内存精确控制（无浪费）    |
| 弱（Weak）   | 长度动态（支持扩容/缩容）         | 灵活数据存储              |

### 4.1 核心数据类型
| 类型               | 说明                                                  | 静态声明示例        | 动态声明示例        |
|--------------------|-------------------------------------------------------|---------------------|---------------------|
| int                | 整型                                                  | int a = 10          | let a = 10          |
| float              | 浮点型                                                | float b = 3.14      | let b = 3.14        |
| str                | 字符串                                                | str c = "Veo"       | let c = "Veo"       |
| bool               | 布尔型                                                | bool d = true       | let d = true        |
| T[N]               | 强数组（长度固定，元素类型相同）                      | int[10] e = [1,2,3] | let e: int[10] = [1,2,3] |
| T[]                | 弱数组（长度动态，元素类型相同）                      | int[] f = [1,2,3]   | let f: int[] = [1,2,3] |
| list[N]            | 强列表（长度固定，元素类型任意）                      | list[5] g = [1,"a",3.14] | let g: list[5] = [1,"a",3.14] |
| list[]             | 弱列表（长度动态，元素类型任意）                      | list[] h = [1,"a",3.14] | let h: list[] = [1,"a",3.14] |
| tuple[N]           | 强元组（最大长度固定，类型任意，不可修改）            | tuple[3] i = (1,"a",true) | let i: tuple[3] = (1,"a",true) |
| tuple[]            | 弱元组（长度动态，类型任意，不可修改）                | tuple[] j = (1,"a",true) | let j: tuple[] = (1,"a",true) |
| dict[N]            | 强字典（最大长度固定，键值类型灵活，可修改）          | dict[5] k = {"name":"Veo",1:2} | let k: dict[5] = {"name":"Veo",1:2} |
| dict[]             | 弱字典（长度动态，键值类型灵活，可修改）              | dict[] l = {"name":"Veo",1:2} | let l: dict[] = {"name":"Veo",1:2} |
| any                | 任意类型（静态兼容）                                  | any m = 123         | let m = 123         |

### 4.2 动态类型声明（let）
动态变量可带/不带类型标注（复合类型的强弱由标注控制）：
```veo
# 1. 弱列表（长度动态，元素类型任意）
let weak_list = [1, "a", 3.14, true]  # 默认弱列表（list[]）
weak_list.append("新增元素")            # 合法：弱列表支持动态扩容
weak_list = 123                        # 合法：动态类型允许切换类型

# 2. 强列表（长度固定，元素类型任意）
let strong_list: list[4] = [1, "a", 3.14, true]  # 长度固定为4
# strong_list.append("新增元素")                   # 报错：强列表不可扩容
strong_list[0] = "替换元素"                       # 合法：在边界内修改元素
strong_list = "切换类型"                          # 合法：动态类型允许切换

# 3. 弱元组（长度动态，不可修改）
let weak_tuple: tuple[] = (1, "a", 3.14)  # Python 风格元组（长度动态）
# weak_tuple[0] = 2                       # 报错：元组不可修改（Python 风格）
weak_tuple.append((true,))                # 合法：弱元组支持扩容
weak_tuple = [1, 2, 3]                    # 合法：切换为列表类型

# 4. 强元组（最大长度固定，不可修改）
let strong_tuple: tuple[3] = (1, "a", 3.14)  # 最大长度3
# weak_tuple.append((true,))                 # 报错：强元组不可扩容
# weak_tuple[0] = 2                         # 报错：元组不可修改

# 5. 弱字典（长度动态，Python 风格）
let weak_dict: dict[] = {"name": "Veo", 1: 2, true: 3.14}  # 键值类型灵活
weak_dict["new_key"] = "value"                              # 合法：新增键值对
weak_dict = (1, 2, 3)                                       # 合法：切换为元组

# 6. 强字典（最大长度固定，Python 风格）
let strong_dict: dict[3] = {"name": "Veo", 1: 2, true: 3.14}  # 最大长度3
# strong_dict["new_key"] = "value"                              # 报错：超出最大长度
```

### 4.3 静态类型声明（类型名 + 变量名）
静态变量严格绑定声明类型（赋值不兼容类型报错，强弱属性固定）：
```veo
# 1. 强数组（长度固定，元素类型相同）
int[5] strong_arr = [1, 2, 3, 4, 5]  # 强数组（长度5）
strong_arr[0] = 10                   # 合法：边界内修改
# strong_arr.append(6)               # 报错：强数组不可扩容
# strong_arr = [6, 7, 8]             # 报错：静态类型不可重新赋值数组
# strong_arr = 123                   # 报错：无法将 int 赋值给 int[5]

# 2. 弱数组（长度动态，元素类型相同）
int[] weak_arr = [1, 2, 3]  # 弱数组（长度动态）
weak_arr.append(4)          # 合法：弱数组支持扩容
# weak_arr = 123            # 报错：无法将 int 赋值给 int[]

# 3. 强列表（长度固定，元素类型任意）
list[4] static_strong_list = [1, "a", 3.14, true]  # 长度固定4
static_strong_list[1] = "替换"                      # 合法：修改元素
# static_strong_list.append("新增")                  # 报错：不可扩容
# static_strong_list = 123                          # 报错：无法将 int 赋值给 list[4]

# 4. 弱字典（长度动态，Python 风格）
dict[] static_weak_dict = {"name": "Veo", 1: 2}  # 长度动态
static_weak_dict["age"] = 3                      # 合法：新增键值对
# static_weak_dict = 123                         # 报错：无法将 int 赋值给 dict[]

# 5. 常量（静态类型，不可修改）
const tuple[2] VERSION = ("1.0", "stable")  # 强元组（最大长度2）
const list[] MAX_ITEMS = [10, "max"]        # 弱列表（不可修改）
# MAX_ITEMS.append(20)                      # 报错：常量不可修改
```

### 4.4 强制类型转换
静态变量需通过「类型名(值)」方式强制转换；复合类型转换需匹配强弱属性：
```veo
# 基础类型转换
int a = 10
float b = float(a)  # 整型转浮点型
str c = str(a)      # 整型转字符串

# 数组 ↔ 列表转换（需匹配强弱）
int[3] strong_arr = [1, 2, 3]
list[3] strong_list = list[3](strong_arr)  # 强数组 → 强列表
# list[] weak_list = list[](strong_arr)    # 合法：强数组 → 弱列表

# 动态类型转静态类型
let d = [1, "a", 3.14]
list[] e = list[](d)  # 动态弱列表 → 静态弱列表
```

## 5. 流程控制
### 5.1 条件判断
支持 `if/else if/else`（简化设计，无 `switch/case`）：
```veo
int score = 85
if score >= 90:
    print("优秀")
else if score >= 80:
    print("良好")
else:
    print("加油")

let temp = 25.5
if temp > 30:
    print("高温")
else:
    print("适宜")
```

### 5.2 循环
#### 5.2.1 for 循环
支持 Python 式遍历（适配数组、列表、元组、字典）：
```veo
# 遍历强数组
int[5] arr = [1, 2, 3, 4, 5]
for item in arr:
    print(item)

# 遍历弱列表（元素类型任意）
list[] mixed_list = [1, "a", 3.14]
for item in mixed_list:
    print(item)

# 遍历元组
tuple[] t = (1, "b", true)
for item in t:
    print(item)

# 遍历字典（Python 风格键值对）
dict[] user_info = {"name": "Veo", "age": 3}
for key, value in user_info.items():
    print(f"{key}: {value}")
```

#### 5.2.2 while 循环
```veo
int i = 0
while i < 3:
    print(i)
    i += 1

let j = 0.0
while j < 1.0:
    print(j)
    j += 0.2
```

### 5.3 跳转语句
支持 `break`（终止循环）、`continue`（跳过当前循环）：
```veo
list[] nums = [1, 2, 3, 4]
for item in nums:
    if item == 2:
        continue  # 跳过当前循环
    if item == 4:
        break     # 终止循环
    print(item)   # 输出：1, 3
```

## 6. 函数
### 6.1 基础函数
使用 `def` 定义函数（支持静态/动态参数、返回值标注，含复合类型）：
```veo
# 接收强列表参数的函数
def process_strong_list(list[4] data) -> int:
    let count = 0
    for item in data:
        if type(item) == int:
            count += 1
    return count

# 接收弱字典参数的函数（Python 风格）
def print_weak_dict(dict[] data):
    for key, value in data.items():
        print(f"{key}: {value}")

# 动态参数函数
def add(a, b):
    return a + b

# 函数调用
list[4] static_list = [1, "a", 2, 3.14]
print(process_strong_list(static_list))  # 输出：2（两个整型元素）

dict[] user_info = {"name": "Alice", "age": 25}
print_weak_dict(user_info)  # 输出：name: Alice, age: 25
```

### 6.2 可变参数
支持 `*args`（可变位置参数）、`**kwargs`（可变关键字参数）：
```veo
def func(*args, **kwargs):
    print(args)    # 元组：接收位置参数
    print(kwargs)  # 字典：接收关键字参数

func(1, "a", 3.14, name="Veo", age=3)
# 输出：
# (1, "a", 3.14)
# {"name": "Veo", "age": 3}
```

## 7. 类与访问控制
Veo 支持面向对象编程，新增 `public/private/protected` 访问控制（完全支持复合类型）：
### 7.1 类定义
```veo
class User:
    # 成员变量：访问控制 + 复合类型
    public str[20] name       # 公有 + 强字符串数组（长度20）
    private list[] hobbies     # 私有 + 弱列表（长度动态，类型任意）
    protected dict[5] info     # 保护 + 强字典（最大长度5）

    # 构造函数（同 Python：__init__）
    def __init__(self, str[20] name, list[] hobbies, dict[5] info):
        self.name = name
        self.hobbies = hobbies
        self.info = info

    # 公有成员函数（返回元组）
    public def get_basic_info(self) -> tuple[2]:
        return (self.name, len(self.hobbies))
```

### 7.2 类实例化与调用
```veo
# 静态类型实例
list[] hobbies = ["reading", "coding", 123]
dict[5] info = {"city": "Beijing", "job": "Developer"}
User user = User("Alice", hobbies, info)

# 访问公有成员（合法）
print(user.name)  # 输出：Alice
print(user.get_basic_info())  # 输出：("Alice", 3)

# 修改弱数组成员
user.hobbies.append("hiking")
print(user.hobbies)  # 输出：["reading", "coding", 123, "hiking"]

# 修改强字典成员（不超出最大长度）
user.info["age"] = 25
# user.info["gender"] = "female"  # 报错：超出最大长度5

# 动态类型实例
let user2 = User("Bob", ["gaming", true], {"city": "Shanghai"})
user2.name = "Bob2"  # 合法：修改强字符串数组
```

### 7.3 继承
支持单继承（通过 `super()` 调用父类方法）：
```veo
class Student(User):
    public tuple[10] student_id  # 强元组（最大长度10，不可修改）

    def __init__(self, str[20] name, list[] hobbies, dict[5] info, tuple[10] student_id):
        super().__init__(name, hobbies, info)
        self.student_id = student_id

    # 访问父类保护成员（合法）
    public def get_city(self) -> str:
        return self.info["city"]

# 实例化子类
tuple[10] stu_id = (2024, 0, 1, 0, 0, 1, 2, 3, 4, 5)
Student stu = Student("Charlie", ["sports", 3.14], {"city": "Guangzhou"}, stu_id)
print(stu.get_city())  # 输出：Guangzhou
```
---