# Veo Memory Safety
Veo integrates Rust-style memory safety mechanisms while retaining C++-level low-level control — balancing safety and flexibility for both application and system development. Composite types (arrays, lists, tuples, dicts) are designed with memory safety and precise control in mind.

## 1. Core Memory Safety Principles
Veo enforces memory safety by default through:
- **Ownership Model**: Each value has exactly one owner (prevents double free, applies to all composite types)
- **Borrowing Rules**: Controlled access to borrowed data (prevents use-after-free)
- **Automatic Memory Management**: No manual memory allocation/deallocation (avoids leaks; strong/weak composite types are auto-managed)
- **Unsafe Blocks**: Opt-in low-level control for system development (explicit unsafe operations)

## 2. Ownership Model (with Composite Types)
### 2.1 Basic Ownership Rules
1. Each value in Veo has exactly one owner (including arrays, lists, tuples, dicts)
2. When the owner goes out of scope, the value is automatically freed
3. Ownership can be transferred (moved) but not duplicated

### 2.2 Syntax Example (Ownership for Composite Types)
```veo
# Ownership for strong array (fixed length)
def create_strong_array() -> int[5]:
    int[5] arr = [1, 2, 3, 4, 5]  # arr is the owner of the strong array
    return arr  # Ownership transferred to the caller

# Ownership for weak list (dynamic length)
def create_weak_list() -> list[]:
    list[] info = [1, "Veo", 3.14]  # info is the owner
    return info  # Ownership transferred

# main takes ownership of composite types
int[5] nums = create_strong_array()
list[] mixed_list = create_weak_list()

print(nums[0])        # Valid: nums is the owner
print(mixed_list[1])  # Valid: mixed_list is the owner

# nums/mixed_list go out of scope here → automatically freed
```

### 2.3 Ownership Transfer (Move Semantics)
```veo
let a: list[] = [1, "a", 3.14]  # a owns the weak list
let b = a                      # Ownership moved to b (a is no longer valid)
# print(a[0])  # Error: a no longer owns the list

# Clone (explicit copy, creates new owner)
let c = b.clone()  # c is a new owner (b remains valid)
print(b[0])        # Valid: b owns the original list
print(c[0])        # Valid: c owns the cloned list

# Strong tuple ownership transfer
tuple[3] d = (1, "b", true)
tuple[3] e = d  # Ownership moved to e (d is invalid)
# print(d[0])  # Error: d no longer owns the strong tuple
```

## 3. Borrowing Rules (with Composite Types)
### 3.1 Basic Borrowing Rules
1. You can have either one mutable borrow or multiple immutable borrows (not both)
2. Borrows cannot outlive the owner
3. Immutable borrows (read-only) vs mutable borrows (read-write)

### 3.2 Syntax Example (Borrowing for Composite Types)
```veo
int[5] nums = [1, 2, 3, 4, 5]  # Strong array
dict[] info = {"a": 1, "b": 2}  # Weak dict

# Immutable borrow (read-only) for strong array
let &ref1 = nums  # & denotes immutable borrow
let &ref2 = nums  # Multiple immutable borrows are allowed
print(ref1[0])    # Valid: read-only access

# Mutable borrow (read-write) for weak dict
let &mut ref3 = info  # &mut denotes mutable borrow
# let &ref4 = info    # Error: Cannot have immutable borrow with mutable borrow
ref3["c"] = 3         # Valid: modify via mutable borrow
print(info["c"])       # Output: 3

# Borrow expires when owner is moved
let new_nums = nums  # Ownership moved to new_nums
# print(ref1[0])     # Error: Borrow outlives owner
```

## 4. Memory Safety for Composite Types
### 4.1 Strong Composite Types (Fixed Length)
- **Safety**: Fixed length prevents buffer overflow (compile-time check)
- **Memory Control**: Precisely allocates memory (no waste from dynamic expansion)
- **Use Case**: Production environments requiring stable memory usage

### 4.2 Weak Composite Types (Dynamic Length)
- **Safety**: Automatic memory management (expands/shrinks without leaks)
- **Flexibility**: Python-like dynamic operations (append, add key-value)
- **Use Case**: Prototyping or scenarios where length is unpredictable

### 4.3 Tuple Specific Safety (Immutable)
- Python-like immutability (cannot modify elements after creation)
- Prevents accidental data modification (thread-safe for read-only scenarios)

## 5. Unsafe Blocks (Low-Level Control)
For system-level development (e.g., C/C++ interop, manual memory management), use `unsafe` blocks to opt out of safety checks (supports all composite types):
### 5.1 Syntax Example (Unsafe Block for Composite Types)
```veo
# Safe code (default)
int[3] safe_strong_arr = [1, 2, 3]
list[] safe_weak_list = [4, "a", 5.6]

# Unsafe block (explicit low-level operations)
unsafe {
    # Manual memory allocation for strong array (C-style)
    int* ptr = malloc(sizeof(int) * 3)  # Raw pointer
    
    # Direct memory access (copy safe_strong_arr to ptr)
    for int i in range(0, 3):
        ptr[i] = safe_strong_arr[i]
    print(ptr[0])  # Output: 1
    
    # Manual deallocation (must do to avoid leaks)
    free(ptr)
}
```

### 5.2 Key Rules for Unsafe Blocks
- `unsafe` blocks are explicit (visible in code review)
- Developers are responsible for memory safety in `unsafe` blocks (including composite types)
- Use `unsafe` only when necessary (e.g., system programming, FFI)

## 6. No GIL (Global Interpreter Lock)
Veo removes Python's GIL (Global Interpreter Lock) — supporting true parallelism and high concurrency (works with all composite types):
```veo
# Async/await for high concurrency (no GIL) with weak list
async def process_list(list[] data) -> list[]:
    let result: list[] = []
    for item in data:
        if type(item) == int:
            result.append(item * 2)
    return result

# Run multiple async tasks in parallel
def main():
    let weak_lists = [[1, "a", 3], [4, "b", 5], [6, "c", 7]]
    for arr in weak_lists:
        spawn(process_list(arr))  # Spawn parallel tasks for weak lists
```
---