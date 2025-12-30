# Veo Syntax Examples
This document contains comprehensive examples covering core Veo syntax features, including arrays, lists, tuples, dicts (strong/weak attributes).

## 1. Composite Type Examples (Array/List/Tuple/Dict)
```veo
# 1. Strong Array (fixed length, same type)
int[5] static_strong_arr = [1, 2, 3, 4, 5]
# static_strong_arr.append(6)  # Error: strong array cannot expand
# static_strong_arr[5] = 6     # Error: out of bounds
print(f"Static strong array: {static_strong_arr}")  # Output: [1,2,3,4,5]

# 2. Weak List (dynamic length, arbitrary types)
let dynamic_weak_list: list[] = [1, "a", 3.14, true]
dynamic_weak_list.append("new_item")
dynamic_weak_list = 123  # Valid: dynamic typing allows type switching
print(f"Dynamic weak list (type switched): {dynamic_weak_list}")  # Output: 123

# 3. Strong Tuple (fixed max length, immutable)
tuple[3] static_strong_tuple = (1, "b", true)
# static_strong_tuple[0] = 2  # Error: tuple is immutable
# static_strong_tuple.append(3)  # Error: strong tuple cannot expand
print(f"Static strong tuple: {static_strong_tuple}")  # Output: (1, "b", true)

# 4. Weak Dict (dynamic length, Python-like K/V)
dict[] static_weak_dict = {"name": "Veo", 1: 2, true: 3.14}
static_weak_dict["age"] = 3
print(f"Static weak dict: {static_weak_dict}")  # Output: {"name":"Veo", 1:2, true:3.14, "age":3}

# 5. Type conversion between composite types
int[3] strong_arr = [1, 2, 3]
list[] weak_list = list[](strong_arr)  # Strong array → weak list
weak_list.append("converted")
print(f"Converted weak list: {weak_list}")  # Output: [1,2,3, "converted"]
```

## 2. Function & Class with Composite Types
```veo
# Function with mixed composite type parameters
def process_composite(int[5] strong_arr, list[] weak_list, dict[3] strong_dict) -> tuple[3]:
    # Calculate strong array sum
    let sum = 0
    for item in strong_arr:
        sum += item
    
    # Count int elements in weak list
    let int_count = 0
    for item in weak_list:
        if type(item) == int:
            int_count += 1
    
    # Count dict keys
    let dict_count = len(strong_dict)
    
    return (sum, int_count, dict_count)

# Class with composite type members
class DataProcessor:
    private int[10] strong_buffer  # Strong array buffer (fixed length)
    public list[] weak_list        # Weak list (dynamic length, arbitrary types)
    protected dict[5] strong_config  # Strong dict (max length 5)

    def __init__(self, int[10] buffer, list[] weak_list, dict[5] config):
        self.strong_buffer = buffer
        self.weak_list = weak_list
        self.strong_config = config

    public def update_config(str key, any value):
        if len(self.strong_config) < 5:
            self.strong_config[key] = value

# Usage
int[10] buffer = [0] * 10  # Strong array initialized with 0s
list[] weak_list = [1, "a", 3.14]
dict[5] config = {"mode": "debug", "level": "info"}

DataProcessor processor = DataProcessor(buffer, weak_list, config)
processor.update_config("port", 8080)
print(f"Updated config: {processor.strong_config}")  # Output: {"mode":"debug", "level":"info", "port":8080}

int[5] arr = [1, 2, 3, 4, 5]
let result = process_composite(arr, weak_list, config)
print(f"Function result: {result}")  # Output: (15, 1, 3)
```

## 3. Namespace & Non-Namespace with Composite Types
```veo
# Define namespace with composite types
namespace collection {
    # Strong array utility
    public def fill_strong_array(int[N] arr, int value) -> int[N]:
        for i in range(0, len(arr)):
            arr[i] = value
        return arr
    
    # Weak list utility
    public def filter_int_list(list[] data) -> list[]:
        let result: list[] = []
        for item in data:
            if type(item) == int:
                result.append(item)
        return result
}

# Non-namespace function with strong tuple
def create_strong_tuple() -> tuple[3]:
    return (1, "Veo", 3.14)

# Import and call
from . import .
using collection

def main():
    # Namespace call (strong array)
    int[5] empty_strong_arr = [0] * 5
    let filled_arr = collection<fill_strong_array>(empty_strong_arr, 10)
    print(f"Filled strong array: {filled_arr}")  # Output: [10,10,10,10,10]
    
    # Namespace call (weak list)
    let mixed_list: list[] = [1, "a", 2, 3.14, 3]
    let int_list = collection<filter_int_list>(mixed_list)
    print(f"Filtered int list: {int_list}")  # Output: [1,2,3]
    
    # Non-namespace call (strong tuple)
    let strong_tuple = create_strong_tuple()
    print(f"Non-namespace strong tuple: {strong_tuple}")  # Output: (1, "Veo", 3.14)
```

## 4. Compiled Mode Full Example (with Composite Types)
```veo
# compiled_example.vo (compiled mode via main)
from . import .
using test::io

# Static composite types (precise memory control)
const str[20] APP_NAME = "Veo Composite Type Demo"
int[3] ports = {8080, 8081, 8082}  # Strong array (fixed ports)
dict[3] config = {"env": "prod", "log": "file", "debug": false}  # Strong dict (max length 3)

def main():
    # Read weak list from namespace function
    let lines = test::io<read_weak_list>("./config.txt")
    print(f"Read lines: {lines}")  # Output: ["line1", 2, 3.14]
    
    # Iterate strong array
    for port in ports:
        print(f"{APP_NAME} running on port: {port}")
    
    # Update strong dict (within max length)
    config["timeout"] = 30
    print(f"Final config: {config}")  # Output: {"env":"prod", "log":"file", "debug":false, "timeout":30}
```

## 5. Interpreted Mode Full Example (with Composite Types)
```veo
# interpreted_example.vo (interpreted mode via __name__)
from . import .
using test::utils

# Dynamic composite types (flexibility)
let strong_tuple: tuple[2] = (("version", 1), ("build", 2024))
let weak_list: list[] = [1.0, 2.0, 3.0]

if __name__ == "__main__":
    # Modify weak list
    weak_list.append(4.0)
    print(f"Weak list: {weak_list}")  # Output: [1.0, 2.0, 3.0, 4.0]
    
    # Access strong tuple
    print(f"Version: {strong_tuple[0][1]}")  # Output: 1
    
    # Type switching (dynamic typing)
    strong_tuple = "switched to string"
    print(f"Type switched: {strong_tuple}")  # Output: switched to string
    
    # Async task with weak list
    async def double_int_values(list[] arr) -> list[]:
        let result: list[] = []
        for val in arr:
            if type(val) == int:
                result.append(val * 2)
        return result
    
    let doubled = await double_int_values([1, 2, "a", 3])
    print(f"Doubled int list: {doubled}")  # Output: [2,4,6]
```

## 6. Memory Safety Example (Composite Types)
```veo
# Ownership and borrowing for composite types
def process_strong_array(int[5] arr) -> int[5]:
    let &mut ref = arr  # Mutable borrow
    ref[0] = ref[0] * 2
    return arr  # Ownership transferred back

def main():
    int[5] original = [1, 2, 3, 4, 5]
    int[5] processed = process_strong_array(original)
    print(processed[0])  # Output: 2
    
    # Weak list ownership
    let weak_list = [10, "a", 20.5]
    let cloned = weak_list.clone()  # New owner
    print(cloned[0])  # Output: 10
    
    # Unsafe block for direct memory access
    unsafe {
        int* ptr = &processed[0]
        print(*ptr)  # Output: 2 (direct access to strong array)
    }
```

---