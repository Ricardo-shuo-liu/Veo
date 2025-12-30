# Veo Dual-Mode Execution
Veo features automatic switching between **compiled mode** (binary generation) and **interpreted mode** (script execution) — determined solely by entry logic (not file suffixes or variable declaration styles).

## 1. Execution Mode Rules
| Mode              | Trigger Condition                          | Output                  | Use Case                  |
|-------------------|--------------------------------------------|-------------------------|---------------------------|
| Compiled Mode     | Top-level unindented `main` function exists | Native binary file      | Production-grade deployment |
| Interpreted Mode  | No `main` + `if __name__ == "__main__"`    | No binary (direct run)  | Rapid prototyping/scripting |

## 2. Compiled Mode (Binary Generation)
### 2.1 Syntax Example (with composite types)
```veo
# Mixed static/dynamic typing + composite types (compiled mode)
from test import .
using test::print_

int version = 1  # Static basic type
let mode: str[10] = "compiled"  # Dynamic strong string array (length 10)

# Static composite types (precise memory control)
int[5] strong_arr = [1, 2, 3, 4, 5]  # Strong array (fixed length)
list[4] strong_list = [1, "a", 3.14, true]  # Strong list (fixed length)
dict[3] strong_dict = {"name": "Veo", "age": 3, "stable": true}  # Strong dict (max length 3)

# Top-level unindented main function (triggers compilation)
def main():
    # Namespace call
    print_<a>(f"{mode} mode running, version: {version}")
    
    # Strong array operation (fixed length)
    let sum = 0
    for item in strong_arr:
        sum += item
    print(f"Strong array sum: {sum}")  # Output: 15
    
    # Strong list operation (arbitrary types)
    let int_count = 0
    for item in strong_list:
        if type(item) == int:
            int_count += 1
    print(f"Int elements in strong list: {int_count}")  # Output: 2
    
    # Strong dict operation (fixed max length)
    print(f"Strong dict: {strong_dict}")  # Output: {"name":"Veo", "age":3, "stable":true}
```

### 2.2 Execution Command
```bash
# Compile to binary (auto-generates executable file)
veo test.vo

# Run the binary
./test
```

### 2.3 Key Features of Compiled Mode
- Compiles to native binary for maximum performance
- Strict static type checking (catches composite type errors at compile time)
- Precise memory control via strong composite types (no waste from dynamic expansion)
- Optimized for production deployment and high-performance scenarios

## 3. Interpreted Mode (Script Execution)
### 3.1 Syntax Example (with composite types)
```veo
# Mixed static/dynamic typing + composite types (interpreted mode)
from test import .
using test::print_

str[10] msg = "interpreted"  # Static strong string array (length 10)
let weak_list: list[] = [123.45, "Veo", true]  # Dynamic weak list (flexible)

# Dynamic composite types (Python-like flexibility)
let weak_tuple: tuple[] = (1, "version", 1.0)  # Weak tuple (dynamic length, immutable)
let weak_dict: dict[] = {"env": "dev", "log_level": "info"}  # Weak dict (dynamic length)

# Script entry (triggers interpretation, no main function)
if __name__ == "__main__":
    # Namespace call
    print_<a>(f"{msg} mode running, list: {weak_list}")
    
    # Weak list modification (dynamic expansion)
    weak_list.append(99.99)
    print(f"Updated weak list: {weak_list}")  # Output: [123.45, "Veo", true, 99.99]
    
    # Weak tuple operation (immutable, expandable)
    weak_tuple.append(("build", 2024))
    print(f"Weak tuple: {weak_tuple}")  # Output: (1, "version", 1.0, ("build", 2024))
    
    # Weak dict operation (Python-like flexibility)
    weak_dict["debug"] = true
    print(f"Weak dict: {weak_dict}")  # Output: {"env":"dev", "log_level":"info", "debug":true}
    
    # Dynamic type switching (let allows type change)
    weak_list = "switched to string"
    print(f"Type switched: {weak_list}")  # Output: switched to string
```

### 3.2 Execution Command
```bash
# Direct interpretation (no binary generation)
veo test.vo
```

### 3.3 Key Features of Interpreted Mode
- No compilation step (fast iteration)
- Flexible dynamic typing (let allows composite type switching)
- Weak composite types for Python-like flexibility (dynamic expansion)
- Optimized for rapid prototyping and short scripts

## 4. Key Notes
- Execution mode is **only** determined by entry logic (`main`/`__name__`) — variable declaration style (static/dynamic) or composite type attributes (strong/weak) do not affect mode
- Both modes fully support arrays, lists, tuples, dicts (consistent syntax and behavior)
- Strong composite types (fixed length) for precise memory control; weak types (dynamic length) for flexibility
- No need to distinguish file suffixes (all Veo files use `.vo`)

---