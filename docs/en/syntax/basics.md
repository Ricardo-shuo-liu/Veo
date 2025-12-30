# Veo Basic Syntax
Veo is a groundbreaking dual-mode programming language with a human-centric syntax design. This document covers core basic syntax rules.

## 1. Comments
Veo supports multiple comment styles for different use cases:
```veo
# Single-line comment (Python style, recommended)
// Single-line comment (C++ style, compatible)

'''
Multi-line comment
Supports line breaks
Can span multiple lines
'''
```

## 2. Indentation & Code Blocks
Veo uses **indentation** to define code blocks (same as Python), 4 spaces are recommended (curly braces `{}` are not supported):
```veo
# Correct example
if 1 > 0:
    print("Valid indentation")
    if 2 > 1:
        print("Nested indentation")

# Error example (inconsistent indentation)
if 1 > 0:
  print("2 spaces")
    print("4 spaces")  # Error: Inconsistent indentation
```

## 3. Reserved Keywords
The following keywords are reserved and cannot be used as variable/function/class names:
| Category         | Keywords                                      |
|------------------|-----------------------------------------------|
| Basic            | let, const, def, class, namespace, using      |
| Flow Control     | if, else if, else, for, while, break, continue |
| Access Control   | public, private, protected                    |
| Execution Mode   | main (exclusive for top-level function)       |
| Others           | return, async, await, unsafe                  |

## 4. Variables & Types
Veo supports two variable declaration styles (both available in compiled/interpreted modes):
- **Static typing** (e.g., `int a`, `int[10] arr`, `list[5] mixed`)：Variable is bound to a specific type (type conversion required for modification)
- **Dynamic typing** (`let a`, `let b: list[5]`)：Same as Python (type changes automatically with assignment)

### Key Definition: Strong vs Weak Composite Types
| Type Attribute | Core Rule                                  | Use Case                          |
|----------------|--------------------------------------------|-----------------------------------|
| Strong         | Fixed length (cannot expand/shrink)         | Precise memory control (no waste) |
| Weak           | Dynamic length (supports expand/shrink)     | Flexible data storage             |

### 4.1 Core Data Types
| Type               | Description                                                                 | Static Declaration Example       | Dynamic Declaration Example       |
|--------------------|-----------------------------------------------------------------------------|----------------------------------|-----------------------------------|
| int                | Integer                                                                     | int a = 10                       | let a = 10                        |
| float              | Floating-point number                                                       | float b = 3.14                   | let b = 3.14                      |
| str                | String                                                                      | str c = "Veo"                    | let c = "Veo"                     |
| bool               | Boolean                                                                     | bool d = true                    | let d = true                      |
| T[N]               | Strong Array (fixed length, same element type)                              | int[10] e = [1,2,3]              | let e: int[10] = [1,2,3]          |
| T[]                | Weak Array (dynamic length, same element type)                              | int[] f = [1,2,3]                | let f: int[] = [1,2,3]            |
| list[N]            | Strong List (fixed length, arbitrary element types)                         | list[5] g = [1, "a", 3.14]       | let g: list[5] = [1, "a", 3.14]   |
| list[]             | Weak List (dynamic length, arbitrary element types)                         | list[] h = [1, "a", 3.14]        | let h: list[] = [1, "a", 3.14]    |
| tuple[N]           | Strong Tuple (fixed max length, arbitrary types, immutable)                 | tuple[3] i = (1, "a", true)      | let i: tuple[3] = (1, "a", true)  |
| tuple[]            | Weak Tuple (dynamic length, arbitrary types, immutable)                     | tuple[] j = (1, "a", true)       | let j: tuple[] = (1, "a", true)   |
| dict[N]            | Strong Dict (fixed max length, Python-like flexible K/V, mutable)           | dict[5] k = {"name": "Veo", 1: 2} | let k: dict[5] = {"name": "Veo", 1: 2} |
| dict[]             | Weak Dict (dynamic length, Python-like flexible K/V, mutable)               | dict[] l = {"name": "Veo", 1: 2} | let l: dict[] = {"name": "Veo", 1: 2} |
| any                | Any type (static compatible)                                                | any m = 123                      | let m = 123                       |

### 4.2 Dynamic Typing (let)
Dynamic variables can be declared with/without type annotations (strong/weak composite types are controlled by annotations):
```veo
# 1. Weak List (dynamic length, arbitrary types)
let weak_list = [1, "a", 3.14, true]  # Default weak list (list[])
weak_list.append("add")                # Valid: weak list supports dynamic expansion
weak_list = 123                        # Valid: dynamic typing allows type switching

# 2. Strong List (fixed length, arbitrary types)
let strong_list: list[4] = [1, "a", 3.14, true]  # Fixed length 4
# strong_list.append("add")                       # Error: strong list cannot expand
strong_list[0] = "replace"                       # Valid: modify element within bounds
strong_list = "switch type"                      # Valid: dynamic typing allows switching

# 3. Weak Tuple (dynamic length, immutable)
let weak_tuple: tuple[] = (1, "a", 3.14)  # Python-like tuple (dynamic length)
# weak_tuple[0] = 2                       # Error: tuple is immutable (Python-like)
weak_tuple.append((true,))                # Valid: weak tuple supports expansion
weak_tuple = [1, 2, 3]                    # Valid: switch to list type

# 4. Strong Tuple (fixed max length, immutable)
let strong_tuple: tuple[3] = (1, "a", 3.14)  # Max length 3
# weak_tuple.append((true,))                 # Error: strong tuple cannot expand
# weak_tuple[0] = 2                         # Error: tuple is immutable

# 5. Weak Dict (dynamic length, Python-like)
let weak_dict: dict[] = {"name": "Veo", 1: 2, true: 3.14}  # Flexible K/V types
weak_dict["new_key"] = "value"                              # Valid: add key-value pair
weak_dict = (1, 2, 3)                                       # Valid: switch to tuple

# 6. Strong Dict (fixed max length, Python-like)
let strong_dict: dict[3] = {"name": "Veo", 1: 2, true: 3.14}  # Max length 3
# strong_dict["new_key"] = "value"                              # Error: exceeds max length
```

### 4.3 Static Typing (Type Name + Variable Name)
Static variables strictly bind to the declared type (error on incompatible assignment, strong/weak attributes are fixed):
```veo
# 1. Strong Array (fixed length, same element type)
int[5] strong_arr = [1, 2, 3, 4, 5]  # Strong array (length 5)
strong_arr[0] = 10                   # Valid: modify within bounds
# strong_arr.append(6)               # Error: strong array cannot expand
# strong_arr = [6, 7, 8]             # Error: cannot reassign static array
# strong_arr = 123                   # Error: cannot assign int to int[5]

# 2. Weak Array (dynamic length, same element type)
int[] weak_arr = [1, 2, 3]  # Weak array (dynamic length)
weak_arr.append(4)          # Valid: expand weak array
# weak_arr = 123            # Error: cannot assign int to int[]

# 3. Strong List (fixed length, arbitrary types)
list[4] static_strong_list = [1, "a", 3.14, true]  # Fixed length 4
static_strong_list[1] = "replace"                  # Valid: modify element
# static_strong_list.append("add")                  # Error: cannot expand
# static_strong_list = 123                          # Error: cannot assign int to list[4]

# 4. Weak Dict (dynamic length, Python-like)
dict[] static_weak_dict = {"name": "Veo", 1: 2}  # Dynamic length
static_weak_dict["age"] = 3                      # Valid: add key-value pair
# static_weak_dict = 123                         # Error: cannot assign int to dict[]

# 5. Constants (static typing, immutable)
const tuple[2] VERSION = ("1.0", "stable")  # Strong tuple (max length 2)
const list[] MAX_ITEMS = [10, "max"]        # Weak list (immutable)
# MAX_ITEMS.append(20)                      # Error: constant cannot be modified
```

### 4.4 Explicit Type Conversion
Static variables require type conversion via `Type(value)`; composite types need matching strong/weak attributes:
```veo
# Basic type conversion
int a = 10
float b = float(a)  # Convert int to float
str c = str(a)      # Convert int to string

# Array ↔ List conversion (must match strong/weak)
int[3] strong_arr = [1, 2, 3]
list[3] strong_list = list[3](strong_arr)  # Strong array → strong list
# list[] weak_list = list[](strong_arr)    # Valid: strong array → weak list

# Dynamic to static conversion
let d = [1, "a", 3.14]
list[] e = list[](d)  # Dynamic weak list → static weak list
```

## 5. Flow Control
### 5.1 Conditional Statements
Supports `if/else if/else` (no `switch/case` for simplicity):
```veo
int score = 85
if score >= 90:
    print("Excellent")
else if score >= 80:
    print("Good")
else:
    print("Keep going")

let temp = 25.5
if temp > 30:
    print("High temperature")
else:
    print("Comfortable")
```

### 5.2 Loops
#### 5.2.1 For Loop
Supports Python-style iteration (works for arrays, lists, tuples, dicts):
```veo
# Iterate over strong array
int[5] arr = [1, 2, 3, 4, 5]
for item in arr:
    print(item)

# Iterate over weak list (arbitrary types)
list[] mixed_list = [1, "a", 3.14]
for item in mixed_list:
    print(item)

# Iterate over tuple
tuple[] t = (1, "b", true)
for item in t:
    print(item)

# Iterate over dict (Python-like key-value)
dict[] user_info = {"name": "Veo", "age": 3}
for key, value in user_info.items():
    print(f"{key}: {value}")
```

#### 5.2.2 While Loop
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

### 5.3 Jump Statements
Supports `break` (terminate loop) and `continue` (skip current iteration):
```veo
list[] nums = [1, 2, 3, 4]
for item in nums:
    if item == 2:
        continue  # Skip current iteration
    if item == 4:
        break     # Terminate loop
    print(item)   # Output: 1, 3
```

## 6. Functions
### 6.1 Basic Functions
Define functions with `def` (supports static/dynamic parameters and return type annotation, including composite types):
```veo
# Function with strong list parameter
def process_strong_list(list[4] data) -> int:
    let count = 0
    for item in data:
        if type(item) == int:
            count += 1
    return count

# Function with weak dict parameter (Python-like)
def print_weak_dict(dict[] data):
    for key, value in data.items():
        print(f"{key}: {value}")

# Dynamic parameter function
def add(a, b):
    return a + b

# Call functions
list[4] static_list = [1, "a", 2, 3.14]
print(process_strong_list(static_list))  # Output: 2 (two int elements)

dict[] user_info = {"name": "Alice", "age": 25}
print_weak_dict(user_info)  # Output: name: Alice, age: 25
```

### 6.2 Variable Parameters
Supports `*args` (variable positional parameters) and `**kwargs` (variable keyword parameters):
```veo
def func(*args, **kwargs):
    print(args)    # Tuple: positional parameters
    print(kwargs)  # Dictionary: keyword parameters

func(1, "a", 3.14, name="Veo", age=3)
# Output:
# (1, "a", 3.14)
# {"name": "Veo", "age": 3}
```

## 7. Classes & Access Control
Veo supports OOP with `public/private/protected` access control (composite types are fully supported):
### 7.1 Class Definition
```veo
class User:
    # Member variables: access control + composite types
    public str[20] name       # Public + strong string array (length 20)
    private list[] hobbies     # Private + weak list (dynamic length, arbitrary types)
    protected dict[5] info     # Protected + strong dict (max length 5)

    # Constructor (same as Python: __init__)
    def __init__(self, str[20] name, list[] hobbies, dict[5] info):
        self.name = name
        self.hobbies = hobbies
        self.info = info

    # Public member function (return tuple)
    public def get_basic_info(self) -> tuple[2]:
        return (self.name, len(self.hobbies))
```

### 7.2 Class Instantiation & Call
```veo
# Static type instance
list[] hobbies = ["reading", "coding", 123]
dict[5] info = {"city": "Beijing", "job": "Developer"}
User user = User("Alice", hobbies, info)

# Access public members (valid)
print(user.name)  # Output: Alice
print(user.get_basic_info())  # Output: ("Alice", 3)

# Modify weak list member
user.hobbies.append("hiking")
print(user.hobbies)  # Output: ["reading", "coding", 123, "hiking"]

# Modify strong dict member (within max length)
user.info["age"] = 25
# user.info["gender"] = "female"  # Error: exceeds max length 5

# Dynamic type instance
let user2 = User("Bob", ["gaming", true], {"city": "Shanghai"})
user2.name = "Bob2"  # Valid: modify strong string array
```

### 7.3 Inheritance
Supports single inheritance (use `super()` to call parent methods):
```veo
class Student(User):
    public tuple[10] student_id  # Strong tuple (max length 10, immutable)

    def __init__(self, str[20] name, list[] hobbies, dict[5] info, tuple[10] student_id):
        super().__init__(name, hobbies, info)
        self.student_id = student_id

    # Access parent protected member (valid)
    public def get_city(self) -> str:
        return self.info["city"]

# Instantiate subclass
tuple[10] stu_id = (2024, 0, 1, 0, 0, 1, 2, 3, 4, 5)
Student stu = Student("Charlie", ["sports", 3.14], {"city": "Guangzhou"}, stu_id)
print(stu.get_city())  # Output: Guangzhou
```

---
