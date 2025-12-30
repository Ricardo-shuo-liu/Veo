# Veo Namespace & Non-Namespace System
Veo features **two fully isolated systems** (namespace/non-namespace) — supporting duplicate element names with no interference, designed to adapt to structured project development and lightweight scripting respectively. All composite types (arrays, lists, tuples, dicts) are fully compatible with both systems.

## 1. Core Concepts
| System               | Syntax Style               | Use Case                  | Key Constraints                          |
|----------------------|----------------------------|---------------------------|------------------------------------------|
| Namespace System     | `using <pkg>::<ns>` + `::<>` | Structured project development | Must import with `using`; call with `::+<>` |
| Non-Namespace System | `from...import` + `.`      | Lightweight scripting      | No redundant constraints (same as Python) |

## 2. Non-Namespace System (Scripting)
### 2.1 Import Syntax
Veo unifies import with `from...import` (no raw `import` syntax):
```veo
# Load entire package (metadata only, independent copy)
from test import .

# Load specific module/file
from test.a import b

# Load specific element (functions/classes/variables with composite types)
from test.a.b import process_strong_list
from test.a.b import User  # Class with composite type members
```

### 2.2 Call Syntax
Use `.` separator (same as Python), composite types work consistently:
```veo
# Load test package
from test import .

# Call function with strong list parameter (non-namespace)
list[4] static_list = [1, "a", 3.14, true]
let res = test.math.process_strong_list(static_list)
print(res)  # Output: 2

# Call class with composite type members (non-namespace)
list[] hobbies = ["reading", "coding"]
dict[5] info = {"city": "Beijing"}
test.model.User user = test.model.User("Alice", hobbies, info)
print(user.name)  # Output: Alice

# Weak dict operation in non-namespace (Python-like)
test.data.config["debug"] = true
print(test.data.config)  # Output: {"debug": true}
```

## 3. Namespace System (Project Development)
### 3.1 Namespace Definition
Define namespaces with `namespace` (supports nesting, fully supports composite types):
```veo
# Basic namespace with composite types
namespace math {
    # Function with strong array parameter
    public def square_strong_array(int[5] arr) -> int[]:
        let result: int[] = []
        for item in arr:
            result.append(item * item)
        return result
    
    # Strong dict (fixed max length, Python-like K/V)
    public dict[2] constants = {"pi": 3.14, "e": 2.71}
    
    # Weak list (dynamic length, arbitrary types)
    public list[] mixed_utils = [100, "max", 0.0, false]
}

# Nested namespace with composite types
namespace test::io {
    public def read_weak_list(str path) -> list[]:
        return ["line1", 2, 3.14, true]  # Return weak list (arbitrary types)
}
```

### 3.2 Namespace Import
Must load the package first, then import with `using`:
```veo
# Step 1: Load package containing namespace
from test import .

# Step 2: Import namespace
using test::math  # Import math namespace from test package
using test::io    # Import nested namespace test::io
```

### 3.3 Namespace Call Syntax
Mandatory `::` for namespace separation and `<>` for element targeting (composite types work consistently):
```veo
# Prerequisite: Load test package + using test::math

# 1. Call function with strong array parameter (namespace)
int[5] arr = [1, 2, 3, 4, 5]
let squared = math<square_strong_array>(arr)
print(squared)  # Output: [1, 4, 9, 16, 25]

# 2. Access strong dict in namespace (fixed max length)
print(math<.>.constants["pi"])  # Output: 3.14
# math<.>.constants["phi"] = 1.618  # Error: exceeds max length 2

# 3. Modify weak list in namespace (dynamic length)
math<.>.mixed_utils.append("new_item")
print(math<.>.mixed_utils)  # Output: [100, "max", 0.0, false, "new_item"]

# 4. Call nested namespace function (return weak list)
let lines = test::io<read_weak_list>("./test.txt")
print(lines)  # Output: ["line1", 2, 3.14, true]
```

## 4. Isolation Between Two Systems
Namespace and non-namespace elements can have the same name (no interference), including elements with composite types:
```veo
from test import .
using test::math

# Non-namespace function (same name as namespace function)
def square_strong_array(int[5] arr) -> int[]:
    let result: int[] = []
    for item in arr:
        result.append(item * 3)
    return result

def main():
    int[5] arr = [1, 2, 3, 4, 5]
    
    # Non-namespace call (weak array result)
    let non_ns_result = square_strong_array(arr)
    print(non_ns_result)  # Output: [3, 6, 9, 12, 15]
    
    # Namespace call (weak array result)
    let ns_result = math<square_strong_array>(arr)
    print(ns_result)  # Output: [1, 4, 9, 16, 25]
```

## 5. Key Rules
- No cross-system interference (duplicate names are allowed, including composite type elements)
- Namespace system enforces strict scoping (avoids naming conflicts in large projects)
- Non-namespace system retains Python's simplicity (fast scripting)
- Both systems fully support arrays/lists/tuples/dicts (strong/weak attributes work consistently)

---