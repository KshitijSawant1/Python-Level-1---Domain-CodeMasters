## 2. Memory Management
## 2.1 Introduction 
Memory management in Python is automatic and handled by the Python memory manager. It uses techniques like reference counting and garbage collection to allocate and free memory. This ensures efficient resource use without requiring manual intervention from the programmer.

## 2.2 What is Memory Management? 
* It is the process of allocating, using, and releasing memory during program execution.
* Ensures optimal use of system memory by avoiding leaks and overflow.
* Manages memory for variables, objects, and data structures used in a program.
* Involves tracking memory usage and cleaning up unused data (garbage collection).
* Aims to maintain program performance and stability by preventing crashes or slowdowns.

## 2.3 Importance
* Prevents Memory Leaks – Frees up unused memory to avoid system slowdowns.
* Improves Performance – Efficient allocation ensures faster execution.
* Optimizes Resource Use – Avoids unnecessary memory consumption.
* Supports Scalability – Handles large datasets or complex programs smoothly.
* Enhances Stability – Reduces chances of crashes due to memory issues.

## 2.4 Memory vs Storage
| Aspect            | Memory (RAM)                                | Storage (Disk/SSD)                          |
|-------------------|----------------------------------------------|---------------------------------------------|
| **Definition**     | Temporary workspace used during program execution | Permanent space used to save files and data |
| **Speed**          | Much faster (volatile)                      | Slower compared to memory                   |
| **Volatility**     | Volatile – data is lost when power is off   | Non-volatile – data remains after shutdown  |
| **Usage**          | Stores active processes and variables       | Stores programs, files, databases, etc.     |
| **Example**        | RAM used to run Python code                 | SSD/HDD used to store Python scripts        |


 ## 2.5  Three Areas of Memory Management 
 ## 2.5.1 Hardware Level (Physical Memory)
* Manages actual physical memory (RAM, cache).
* Uses memory address lines to read/write data.
* Ensures fast and efficient hardware-level access.

## 2.5.2 Operating System Level (OS Memory Manager)
* Allocates memory to processes and applications.
* Handles virtual memory, paging, and segmentation.
* Prevents memory conflicts between programs.

## 2.5.3 Program/Application Level (User Space)
* Handled by programming languages like Python.
* Manages variables, objects, and data structures.
* Uses features like dynamic allocation and garbage collection.

# 2.6 Memory Management in Python – Key Points
* **Automatic Handling**:Python automatically manages memory allocation and deallocation using its built-in memory manager.
* **Private Heap Space**:All Python objects and data structures are stored in a private heap managed by the interpreter.
* **Reference Counting**:Python tracks the number of references to each object; when it reaches zero, the memory is released.
* **Garbage Collection**:A built-in garbage collector handles cyclic references and reclaims unused memory.
* **Dynamic Typing**:Python dynamically allocates memory based on variable type and usage during runtime.

## 2.7 Global Interpreter Lock (GIL) in Python
The Global Interpreter Lock (GIL) is a mechanism used in the CPython implementation of Python to ensure that only one thread executes Python bytecode at a time, even on multi-core systems.
- **Ensures Thread Safety** by allowing only one thread to execute at a time in CPython.
- **Limits Multithreading** for CPU-bound tasks due to single-thread execution.
- **No Major Impact on I/O-bound** operations like file or network handling.
- **Workarounds** include using the `multiprocessing` module or GIL-free interpreters like Jython and IronPython.

# 2.8 Python Memory Allocation
Python uses a layered and automatic memory management system to efficiently allocate memory during runtime.
 Key Points:
- **Private Heap Space**  
  All Python objects and data structures are stored in a dedicated private heap managed by the Python interpreter.
- **Memory Manager**  
  Python has a built-in memory manager that handles allocation and deallocation of memory internally.
- **Object-Specific Allocators**  
  Python uses different allocators for various object types. Small integers and strings are often preallocated and reused to save memory.
- **PyMalloc**  
  A specialized memory allocator in CPython designed for fast allocation of small memory blocks.
- **Dynamic Allocation**  
  Memory is dynamically allocated when objects are created and automatically released when no longer in use.

# 2.9 Garbage Collection and Generational GC in Python
## Theory

### 2.9.1 Garbage Collection Mechanism in Python

Garbage collection (GC) in Python refers to the **automatic process** of identifying and freeing memory occupied by objects that are **no longer in use**.
Python primarily uses:
- **Reference Counting** – Tracks how many references point to an object.
- **Generational GC** – An enhancement to handle cyclic references and improve performance.

### 2.9.2 Reference Counting
- Every object has an internal **reference count**.
- When the count drops to **zero**, the object becomes **eligible for deletion**.
- Reference counting fails when **cyclical references** exist (i.e., two or more objects refer to each other).

### 2.9.3 Generational Garbage Collection

To handle cycles and optimize performance, Python divides objects into **three generations**:

| Generation | Description                              |
|------------|------------------------------------------|
| **Gen 0**  | Newly created objects (collected most frequently) |
| **Gen 1**  | Objects that survived Gen 0 collections   |
| **Gen 2**  | Long-lived objects (collected the least)  |
- Objects move from younger to older generations if they survive collection.
- Python assumes most objects are **short-lived**, so Gen 0 is collected often.

### 2.9.4 Purpose of Generational GC in Python:
- Improves performance by **not checking long-lived objects frequently**.
- Efficiently **cleans up cycles** that reference counting alone can’t detect.

## 2.9.5 Example – Using `gc` Module in Python

```python
import gc

# Create cyclic reference
class Node:
    def __init__(self):
        self.ref = None

a = Node()
b = Node()
a.ref = b
b.ref = a

# Remove the references
del a
del b

# Force garbage collection
unreachable = gc.collect()
print(f"Unreachable objects collected: {unreachable}")
```
# 2.9.6 Generational Garbage Collection in Python – Visual

Below is a simple representation of how Python's garbage collector manages objects across generations:

                      +-------------------+
                      |  Newly Created    |
                      |   Objects (Gen 0) |
                      +--------+----------+
                               |
          Survive collection   |
                               v
                      +-------------------+
                      |  Survived Once     |
                      |   Objects (Gen 1)  |
                      +--------+----------+
                               |
          Survive again        |
                               v
                      +-------------------+
                      |  Long-Lived        |
                      |   Objects (Gen 2)  |
                      +-------------------+

 **Collection Frequency**:
- **Gen 0**: Collected **frequently**
- **Gen 1**: Collected **less frequently**
- **Gen 2**: Collected **rarely**

## 2.9.7 Python assumes:
- Most objects die young (Gen 0).
- Fewer objects survive long enough to move to Gen 2.

## 2.10 Reference Counting in Python

Reference counting is the **primary memory management technique** used by Python to track and manage object lifecycles. Every object in Python maintains a **counter** of how many references point to it. When this count drops to zero, the object is **automatically destroyed**, and the memory is reclaimed.

---

## 2.10.1 Key Concepts

1. **Reference Count**  
   Every object has an internal reference count that keeps track of how many references (variables, data structures, etc.) point to it.

2. **Automatic Deallocation**  
   Python automatically deallocates memory for objects once their reference count becomes zero — no need for manual memory management.

3. **Incrementing Reference Count**  
   Assigning an object to a new variable or passing it as a parameter to a function increases its reference count.

4. **Decrementing Reference Count**  
   Deleting a reference using `del`, or letting a variable go out of scope, reduces the reference count.

5. **Limitation – Cyclical References**  
   Reference counting **cannot handle cycles**, where two or more objects refer to each other (e.g., in linked lists or graphs). These objects remain in memory even when unreachable.


## 2.10.2 Example

```python
import sys

x = []
y = x  # Reference count increases
print(sys.getrefcount(x))  # Output includes temp reference from getrefcount

del y  # Reference count decreases
print(sys.getrefcount(x))  # One less reference now

```
# 2.11 Cyclical References in Python

Cyclical (or circular) references occur when two or more objects reference each other, forming a cycle. These objects cannot be garbage collected through reference counting alone, which may result in memory leaks.

## 2.11.1 What is a Cyclical Reference?

A cyclical reference is a scenario where objects are linked in such a way that they reference each other, preventing their reference count from reaching zero even if they are no longer used by the program.

## 2.11.2 Example of Cyclic Reference

```python
class Node:
    def __init__(self):
        self.reference = None

a = Node()
b = Node()
a.reference = b
b.reference = a  # Cyclic reference created

del a
del b  # Objects are unreachable but not collected by reference counting
Even after a and b are deleted, the objects remain in memory because they reference each other.
```
# 2.11 Common Ways to Reduce Space Complexity in Python

Efficient use of memory is a critical skill in Python programming. The following practices help reduce space complexity, making your code more optimized and scalable—especially important in applications that handle large datasets or run on resource-constrained devices.

### 1. Use Generators Instead of Lists
Generators allow iteration without storing the entire sequence in memory, which is ideal when working with large data.
**Example:**
```python
def get_numbers():
    for i in range(10000):
        yield i

gen = get_numbers()  # Much lighter on memory than list
```
### 2. Prefer Built-in Data Structures
Use Python’s space-efficient native types:
- tuple over list for fixed-size collections
- set for fast membership tests and uniqueness
- array.array for numeric data instead of lists

### 3. Avoid Unnecessary Copies
Avoid duplicating large data structures unless needed.
Example:
# Creates a full copy, avoid if not required
copied = original[:]

### 4. Delete Unused Variables
Explicitly free memory by deleting objects no longer in use.

Example:
del big_data

### 5. Use __slots__ to Optimize Class Instances
For custom classes, using __slots__ can prevent Python from creating a dynamic dictionary for attributes—saving memory when instantiating many objects.

Example:
class Point:
    __slots__ = ['x', 'y']
    def __init__(self, x, y):
        self.x = x
        self.y = y