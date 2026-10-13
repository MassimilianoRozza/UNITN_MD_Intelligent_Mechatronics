---
tags:
  - RECH
  - readings
data: 2025-07-17
source: "[understanding-stack-and-heap-memory](https://towardsdev.com/understanding-stack-and-heap-memory-a96e90f9c982)"
---
# Stack vs Heap
**The stack** and **the heap** are two fundamental memory regions used by programs at runtime. They serve different purposes for storing data during program execution.
![](https://miro.medium.com/v2/resize:fit:875/1*iFsZC_GQ229Sz8jPveAAUg.png)
> Stack & Heap. [Source](https://namso.me/stack-and-heap-in-programming/)

A computer provides several **core resources** that programs and users can utilize. The operating system manages these resources to ensure efficiency, security, and fairness.
![](https://miro.medium.com/v2/resize:fit:875/1*MRnPDZgHrSPCkOlH1LZcYw.png)
> Computer Resources. [Source](https://mscit.mkcl.org/mscit-for-school-students/computing-resources)

The key computer resources relevant to our topic:
1. **CPU (Central Processing Unit)**
	- Executes instructions and manages program flow.
2. **Main Memory — RAM**
	- Where both the **stack** and the **heap** reside.
![](https://miro.medium.com/v2/resize:fit:875/1*8gjofMirxiPCV3SQKMTScA.png)
> RAM. [Source](https://www.simplilearn.com/tutorials/data-structure-tutorial/stacks-vs-heap)

The regions of RAM that are used while a program is running are called **volatile memory regions**.
![](https://miro.medium.com/v2/resize:fit:553/1*bbDlGT2kOPSNU4OXzixsPA.png)
> Volatile memory regions. [Source](https://www.embedded.com/mastering-stack-and-heap-for-system-reliability-part-1-calculating-stack-size/)

## **Global/Static Variables (Data Segment)**
All global variables and static variables in your code are stored here. These variables are allocated once and exist for the entire program duration.
```C++
int global_var; // in data segment  
static int static_var = 10; // also here
```
It is created when the program starts and destroyed when it ends. It is cleaned automatically. It has a fixed size, and it is determined at compile time based on all the global and static variables and string literals defined in your code.
## **Stack**
The stack is the region of memory that stores local variables, function parameters, and function call information (such as return addresses).

Stack works as LIFO: last-in, first-out (just like the stack data structure).
![](https://miro.medium.com/v2/resize:fit:561/1*2zEU6MesUQrT0b5pAkXzSg.png)
> Stack data structure. [Source](https://www.cs.colostate.edu/~cs165/.Fall19/assignments/P5/doc/stack_description.html)

The **call stack** is a data structure that keeps track of function calls. Each time a function is called, a “stack frame” is pushed onto the stack. When the function ==exists==, its stack frame is popped off.

Suppose we have a code block like this:
```C++
function one(){  
    two();  
}  
  
function two() {  
    three();  
}  
  
function three() {  
    console.trace("Call Stack");  
}
```

Each numbered box shows the call stack at a particular moment:
![](https://miro.medium.com/v2/resize:fit:875/1*CPUulnz6rGuOxP_QWRa1Lg.png)
> Call Stack. [Source](https://medium.com/swlh/in-depth-introduction-to-call-stack-in-javascript-a07b8513bcc3)

1. `one()` is called.
	- **Stack:** Only `one()` is on the stack.
2. Inside `one()`, it calls `two()`.
	- **Stack:** `two()` is added **on top of** `one()`.
3. Inside `two()`, it calls `three()`.
	- **Stack:** `three()` is now on top of `two()`, which is on top of `one()`.
4. `three()` finishes (after `console.trace` runs).
	- **Stack:** `three()` is popped off, returning control to `two()`.
5. `two()` finishes and is removed from the stack.
	- **Stack:** Only `one()` remains.
6. `one()` finishes and is removed.
	- **Stack:** Now the call stack is **empty** (program is done running).

Data on the stack only exists while the function is running.

The stack grows and shrinks automatically with functions calls and returns.

Pointers are variables that store memory addresses.
![](https://miro.medium.com/v2/resize:fit:401/1*PsxNpHCIqANtYpqb9aZiww.png)
> stackPointer. [Source](https://bob.cs.sonoma.edu/testing/sec-stack.html)

When a function returns, all its stack variables are destroyed and the stack pointer moves back.

You don’t have to manually free stack memory. (This is different from the [[#heap]], where you must manually free memory.)

You can have pointers to stack allocated data, but if you return a pointer to a local (stack) variable from a function, it becomes invalid after the function returns (the stack frame is gone!).

```C++
int* foo() {  
    int local = 42;  // stored on stack  
    return &local;   // pointer to stack variable (danger!)  
} // 'local' is destroyed, pointer is now invalid
```

The stack has a limited size, set by the OS (typically a few hundred KBs to several MBs per program/thread).

**Stack overflow** happens when the program uses more stack memory than is available.

## Heap
==The heap is a large region of memory used for dynamic allocation== — for data whose size or lifetime isn’t known until runtime.

You explicitly allocate (and usually free) memory on the heap, using commands like `malloc`/`free`.

The lifetime of data on the heap is controlled by the programmer (or a garbage collector, in managed languages).

It is larger in size compared to the stack. Typically, objects, large data structures (like arrays, linked lists), and any data you want to share between functions or have outlive a single function call are stored in heap.

Each thread has its own stack but the heap are shared between threads.
![](https://miro.medium.com/v2/resize:fit:491/1*ro2xnTb6Qu5l6GAP-j32sw.png)
> Threads. [Source](https://stackoverflow.com/questions/16264118/how-jvm-stack-heap-and-threads-are-mapped-to-physical-memory-or-operation-syste)

Suppose we have a code like this:

```java
class Person {  
    int id;  
    String name;  
  
    public Person(int id, String name) {  
        this.id = id;  
        this.name = name;  
    }  
}  
  
public class PersonBuilder {  
    private static Person buildPerson(int id, String name) {  
        return new Person(id, name);  
    }  
  
    public static void main(String[] args) {  
        int id = 23;  
        String name = "John";  
        Person person = null;  
        person = buildPerson(id, name);  
    }  
}
```

**Call Stack:** At the deepest point, the stack has (top to bottom):

1. `Person(int, String)` (constructor)
2. `buildPerson(int, String)`
3. `main(String[])`
![](https://miro.medium.com/v2/resize:fit:875/1*QbkTk9ICcF_evmZlJuRbdg.png)
> [Source](https://www.baeldung.com/java-stack-heap)

**Call Stack:**
1. A frame for `main()` is pushed onto the stack.
	**Stack Memory (main’s frame):**
	- `id = 23` (primitive int, value stored directly on the stack)
	- `name = "John"` (reference to a String, pointer on the stack)
	- `person = null` (reference to a Person object, initially null)
	`"John"` goes into the **string pool** in the heap. The `name` variable on the stack holds a **reference** (pointer) to the string in the heap.
2. `buildPerson` is called, pushing a new frame onto the call stack.
	**Stack Memory (buildPerson’s frame):**
	- `id = 23` (value copied from main)
	- `name` (reference copied from main; still points to the string in the heap)
3. Calls the `Person` constructor, pushing a new frame onto the stack.
	**Stack Memory (Person constructor’s frame):**
	- `id = 23` (constructor parameter)
	- `name` (reference to "John")
	- `this` (reference to the new Person object being created)
	In the **heap**, a new `Person` object is created:
	- `id` field is set to `23`
	- `name` field is set to point to the same `"John"` string object in the heap
4. After the constructor finishes, its frame pops off the stack.
	`buildPerson` returns the **reference** to the newly created `Person` object in the heap; the frame for `buildPerson` is also popped.
5. In `main`, `person` now holds a reference to the `Person` object in the heap.

- **Primitives** are stored directly in the stack frame.
- **Object references** (like for `Person` and `String`) are pointers on the stack, but the actual objects live in the heap.
- **Heap** is where long-lived objects and shared data exist.
- **Stack** grows and shrinks automatically as functions are called and returned.
# Sources
- [https://namso.me/stack-and-heap-in-programming/](https://namso.me/stack-and-heap-in-programming/)
- [https://mscit.mkcl.org/mscit-for-school-students/computing-resources](https://mscit.mkcl.org/mscit-for-school-students/computing-resources)
- [https://www.simplilearn.com/tutorials/data-structure-tutorial/stacks-vs-heap](https://www.simplilearn.com/tutorials/data-structure-tutorial/stacks-vs-heap)
- [https://www.embedded.com/mastering-stack-and-heap-for-system-reliability-part-1-calculating-stack-size/](https://www.embedded.com/mastering-stack-and-heap-for-system-reliability-part-1-calculating-stack-size/)
- [https://medium.com/swlh/in-depth-introduction-to-call-stack-in-javascript-a07b8513bcc3](https://medium.com/swlh/in-depth-introduction-to-call-stack-in-javascript-a07b8513bcc3)
- [https://stackoverflow.com/questions/16264118/how-jvm-stack-heap-and-threads-are-mapped-to-physical-memory-or-operation-syste](https://stackoverflow.com/questions/16264118/how-jvm-stack-heap-and-threads-are-mapped-to-physical-memory-or-operation-syste)
- [https://www.baeldung.com/java-stack-heap](https://www.baeldung.com/java-stack-heap)