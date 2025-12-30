# Veo 语法示例
本文档包含覆盖Veo核心语法特性的完整示例，涵盖数组、列表、元组、字典（强/弱属性）等类型的使用方式。

## 1. 复合类型示例（数组/列表/元组/字典）
```veo
# 1. 强类型数组（固定长度，类型统一）
int[5] static_strong_arr = [1, 2, 3, 4, 5]
# static_strong_arr.append(6)  # 错误：强类型数组不支持扩容
# static_strong_arr[5] = 6     # 错误：数组下标越界
print(f"静态强类型数组: {static_strong_arr}")  # 输出：[1,2,3,4,5]

# 2. 弱类型列表（动态长度，支持任意类型）
let dynamic_weak_list: list[] = [1, "a", 3.14, true]
dynamic_weak_list.append("new_item")
dynamic_weak_list = 123  # 合法：动态类型支持类型切换
print(f"动态弱类型列表（类型已切换）: {dynamic_weak_list}")  # 输出：123

# 3. 强类型元组（固定最大长度，不可变）
tuple[3] static_strong_tuple = (1, "b", true)
# static_strong_tuple[0] = 2  # 错误：元组元素不可修改
# static_strong_tuple.append(3)  # 错误：强类型元组不支持扩容
print(f"静态强类型元组: {static_strong_tuple}")  # 输出：(1, "b", true)

# 4. 弱类型字典（动态长度，类Python键值对）
dict[] static_weak_dict = {"name": "Veo", 1: 2, true: 3.14}
static_weak_dict["age"] = 3
print(f"静态弱类型字典: {static_weak_dict}")  # 输出：{"name":"Veo", 1:2, true:3.14, "age":3}

# 5. 复合类型间的类型转换
int[3] strong_arr = [1, 2, 3]
list[] weak_list = list[](strong_arr)  # 强类型数组 → 弱类型列表
weak_list.append("converted")
print(f"转换后的弱类型列表: {weak_list}")  # 输出：[1,2,3, "converted"]
```

## 2. 包含复合类型的函数与类
```veo
# 包含混合复合类型参数的函数
def process_composite(int[5] strong_arr, list[] weak_list, dict[3] strong_dict) -> tuple[3]:
    # 计算强类型数组元素总和
    let sum = 0
    for item in strong_arr:
        sum += item
    
    # 统计弱类型列表中的整数元素数量
    let int_count = 0
    for item in weak_list:
        if type(item) == int:
            int_count += 1
    
    # 统计字典键的数量
    let dict_count = len(strong_dict)
    
    return (sum, int_count, dict_count)

# 包含复合类型成员的类
class DataProcessor:
    private int[10] strong_buffer  # 强类型数组缓冲区（固定长度）
    public list[] weak_list        # 弱类型列表（动态长度，支持任意类型）
    protected dict[5] strong_config  # 强类型字典（最大长度5）

    def __init__(self, int[10] buffer, list[] weak_list, dict[5] config):
        self.strong_buffer = buffer
        self.weak_list = weak_list
        self.strong_config = config

    public def update_config(str key, any value):
        if len(self.strong_config) < 5:
            self.strong_config[key] = value

# 使用示例
int[10] buffer = [0] * 10  # 初始化为全0的强类型数组
list[] weak_list = [1, "a", 3.14]
dict[5] config = {"mode": "debug", "level": "info"}

DataProcessor processor = DataProcessor(buffer, weak_list, config)
processor.update_config("port", 8080)
print(f"更新后的配置: {processor.strong_config}")  # 输出：{"mode":"debug", "level":"info", "port":8080}

int[5] arr = [1, 2, 3, 4, 5]
let result = process_composite(arr, weak_list, config)
print(f"函数执行结果: {result}")  # 输出：(15, 1, 3)
```

## 3. 命名空间与非命名空间下的复合类型使用
```veo
# 定义包含复合类型的命名空间
namespace collection {
    # 强类型数组工具函数
    public def fill_strong_array(int[N] arr, int value) -> int[N]:
        for i in range(0, len(arr)):
            arr[i] = value
        return arr
    
    # 弱类型列表工具函数
    public def filter_int_list(list[] data) -> list[]:
        let result: list[] = []
        for item in data:
            if type(item) == int:
                result.append(item)
        return result
}

# 非命名空间下的强类型元组函数
def create_strong_tuple() -> tuple[3]:
    return (1, "Veo", 3.14)

# 导入并调用
from . import .
using collection

def main():
    # 命名空间调用（强类型数组）
    int[5] empty_strong_arr = [0] * 5
    let filled_arr = collection<fill_strong_array>(empty_strong_arr, 10)
    print(f"填充后的强类型数组: {filled_arr}")  # 输出：[10,10,10,10,10]
    
    # 命名空间调用（弱类型列表）
    let mixed_list: list[] = [1, "a", 2, 3.14, 3]
    let int_list = collection<filter_int_list>(mixed_list)
    print(f"过滤后的整数列表: {int_list}")  # 输出：[1,2,3]
    
    # 非命名空间调用（强类型元组）
    let strong_tuple = create_strong_tuple()
    print(f"非命名空间强类型元组: {strong_tuple}")  # 输出：(1, "Veo", 3.14)
```

## 4. 编译模式完整示例（含复合类型）
```veo
# compiled_example.vo（通过main函数启用编译模式）
from . import .
using test::io

# 静态复合类型（精准内存控制）
const str[20] APP_NAME = "Veo 复合类型示例"
int[3] ports = {8080, 8081, 8082}  # 强类型数组（固定端口列表）
dict[3] config = {"env": "prod", "log": "file", "debug": false}  # 强类型字典（最大长度3）

def main():
    # 从命名空间函数读取弱类型列表
    let lines = test::io<read_weak_list>("./config.txt")
    print(f"读取的行数据: {lines}")  # 输出：["line1", 2, 3.14]
    
    # 遍历强类型数组
    for port in ports:
        print(f"{APP_NAME} 运行在端口: {port}")
    
    # 更新强类型字典（不超过最大长度）
    config["timeout"] = 30
    print(f"最终配置: {config}")  # 输出：{"env":"prod", "log":"file", "debug":false, "timeout":30}
```

## 5. 解释模式完整示例（含复合类型）
```veo
# interpreted_example.vo（通过__name__启用解释模式）
from . import .
using test::utils

# 动态复合类型（灵活使用）
let strong_tuple: tuple[2] = (("version", 1), ("build", 2024))
let weak_list: list[] = [1.0, 2.0, 3.0]

if __name__ == "__main__":
    # 修改弱类型列表
    weak_list.append(4.0)
    print(f"弱类型列表: {weak_list}")  # 输出：[1.0, 2.0, 3.0, 4.0]
    
    # 访问强类型元组
    print(f"版本号: {strong_tuple[0][1]}")  # 输出：1
    
    # 类型切换（动态类型特性）
    strong_tuple = "已切换为字符串类型"
    print(f"类型已切换: {strong_tuple}")  # 输出：已切换为字符串类型
    
    # 带弱类型列表的异步任务
    async def double_int_values(list[] arr) -> list[]:
        let result: list[] = []
        for val in arr:
            if type(val) == int:
                result.append(val * 2)
        return result
    
    let doubled = await double_int_values([1, 2, "a", 3])
    print(f"整数列表翻倍结果: {doubled}")  # 输出：[2,4,6]
```

## 6. 内存安全示例（复合类型）
```veo
# 复合类型的所有权与借用机制
def process_strong_array(int[5] arr) -> int[5]:
    let &mut ref = arr  # 可变借用
    ref[0] = ref[0] * 2
    return arr  # 所有权返还

def main():
    int[5] original = [1, 2, 3, 4, 5]
    int[5] processed = process_strong_array(original)
    print(processed[0])  # 输出：2
    
    # 弱类型列表的所有权
    let weak_list = [10, "a", 20.5]
    let cloned = weak_list.clone()  # 创建新的所有者
    print(cloned[0])  # 输出：10
    
    # 直接内存访问的不安全代码块
    unsafe {
        int* ptr = &processed[0]
        print(*ptr)  # 输出：2（直接访问强类型数组内存）
    }
```

---