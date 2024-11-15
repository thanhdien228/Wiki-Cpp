## What is Smart Pointer in C++?

**Problems with Normal Pointers**

- Memory Leaks: This occurs when memory is repeatedly allocated by a program but never freed. This leads to excessive memory consumption and eventually leads to a system crash. 

**Example**

```cpp
// Program with memory leak
#include <bits/stdc++.h>
using namespace std;

// function with memory leak
void func_to_show_mem_leak()
{
    int* ptr = new int(5);

    // body

    // return without deallocating ptr
    return;
}

// driver code
int main()
{

    // Call the function
    // to get the memory leak
    func_to_show_mem_leak();

    return 0;
}
```
- Dangling Pointers: A dangling pointer is a pointer that occurs at the time when the object is de-allocated from memory without modifying the value of the pointer.

**Example**
```cpp
#include <iostream>
using namespace std;

int main()
{
    // Allocating memory
    int* ptr = new int(5);

    // Deallocating memory
    delete ptr;

    // Now, ptr becomes a dangling pointer
    cout << "ptr is now a dangling pointer" << endl;

    return 0;
}
```
- Wild Pointers: Wild pointers are pointers that are declared and allocated memory but the pointer is never initialized to point to any valid object or address.
```cpp
int main()
{
    /* Wild pointer */
    int* p;
    /* Some unknown memory location is being corrupted.
    This should never be done. */
    *p = 12;
    std::cout << p;
    /* This causes segmentation fault */
    return 0;
}
```
- Data Inconsistency: Data inconsistency occurs when some data is stored in memory but is not updated in a consistent manner.
```cpp
int main()
{
    int* p1 = new int(5);
    int* p2 = p1;   // Both shared the same memory

    *p1 = 5;  
    delete p1;      // Memory freed using p1
    *p2 = 10;       // Now p2 points to freed memory, causes unexpected behaviour

    return 0;
}
```
- Buffer Overflow: When a pointer is used to write data to a memory address that is outside of the allocated memory block. This leads to the corruption of data which can be exploited by malicious attackers.
```cpp
int main()
{
    int* p = new int[5];
    
    p[10] = 2;  // This may corrupt other variables or causes unexpected results

    // for (int i = 0; i <= 10; i++) {
    //     std::cout << p[i];
    // }
    return 0;
}
```

A Smart Pointer is a wrapper class over a pointer with an operator like `*` and `->` overloaded. The objects of the smart pointer class look like normal pointers. But, unlike Normal Pointers, it can deallocate and free destroyed object memory.

The idea is to take a class with a pointer, destructor, and overloaded operators like `*` and `->`. Since the destructor is automatically called when an object goes out of scope, the dynamically allocated memory would automatically be deleted (or the reference count can be decremented).

**Example**

```cpp
#include <iostream>
using namespace std;

class SmartPtr {
    int* ptr; // Actual pointer
public:
    // Constructor
    // explicit keyword used to mark constructors to not implicitly convert types
    explicit SmartPtr(int* p = NULL) { ptr = p; }

    // Destructor
    ~SmartPtr() { delete (ptr); }

    // Overloading dereferencing operator
    int& operator*() { return *ptr; }
};

int main()
{
    SmartPtr ptr(new int());
    *ptr = 20;
    cout << *ptr;

    // We don't need to call delete ptr: when the object
    // ptr goes out of scope, the destructor for it is
    // automatically called and destructor does delete ptr.

    return 0;
}
```
**Output**
```
20
```

## What is the advantage of Smart Pointer over normal pointers?

Benefit of Smart Pointers

- **Automatic Memory Management**: Smart pointers automatically manage the lifetime of resources, making sure that they are properly freed when no longer needed. This reduces the risk of memory leaks and dangling pointers.
- **Exception Safety**: Smart pointers provide strong exception safety guarantees. When an exception is thrown, smart pointers make sure that resources are still properly cleaned up, preventing resource leaks in error scenarios.
- **Simplified Code**: By abstracting the complexity of manual memory management, smart pointers make code easier to read, write, and maintain. Developers can focus on the core logic of their applications without worrying about the intricacies of resource management.
- **Improved Safety**: Smart pointers provide various levels of ownership semantics, allowing developers to choose the appropriate type of smart pointer for their use case. This improves the safety and strength of code by preventing common mistakes associated with raw pointers.
- **Performance**: While smart pointers add some overhead due to reference counting and other mechanisms, this overhead is generally outweighed by the benefits of safer and more maintainable code. In many cases, smart pointers can lead to performance improvements by reducing bugs and making code more efficient.

Consider the table that summarizes the key difference between normal pointers and smart pointers in C++:

Features | Normal Pointers | Smart Pointers
---|---|---
Memory Management | Manual (using new & delete) | Automatic
Ownership | No explicit | Encapsulate
Exception Safety | Prone to memory leaks if exception occurs | Generally, exception-safe
Resource Management | Requires manual handling | Used with built-in mechanisms

## Types of Smart Pointer

### Auto Pointer

A Smart pointer is a composition class that is designed to manage dynamically allocated memory and ensure that memory gets deleted when the smart pointer object goes out of scope

**A critical flaw**
```cpp
#include <iostream>

// Same as above
template <typename T>
class Auto_ptr1
{
	T* m_ptr {};
public:
	Auto_ptr1(T* ptr=nullptr)
		:m_ptr(ptr)
	{
	}

	~Auto_ptr1()
	{
		delete m_ptr;
	}

	T& operator*() const { return *m_ptr; }
	T* operator->() const { return m_ptr; }
};

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};

int main()
{
	Auto_ptr1<Resource> res1(new Resource());
	Auto_ptr1<Resource> res2(res1); // Alternatively, don't initialize res2 and then assign res2 = res1;

	return 0;
}
```
Output
```
Resource acquired
Resource destroyed
Resource destroyed
```
Very likely (but not necessarily) your program will crash at this point. See the problem now? Because we haven't supplied a copy constructor or an assignment operator, C++ provides one for us. And the functions it provides do shallow copies. So when we initialize res2 with res1, both Auto_ptr1 variables are pointed at the same Resource. When res2 goes out of the scope, it deletes the resource, leaving res1 with a dangling pointer. When res1 goes to delete its (already deleted) Resource, undefined behavior will result (probably a crash)!

**Why was auto_ptr removed?**

auto_ptr was depreciated in C++ 11 and removed in C++ 17. The removal of the auto_ptr was due to the following limitations:

- An auto_ptr can't be used to point to an array. While deleting the pointer to an array, we need to use delete[] but in auto_ptr we can only use delete.
- An auto_ptr can not be used with STL Containers because the containers, or algorithms manipulating them, might copy the stored elements. Copies of auto_ptrs aren't equal because the original is set to NULL after being copied.
- Due to the above limitations, auto_ptr was removed from C++ and later replaced with unique_ptr.
### Unique Pointer
- **unique_ptr** stores one pointer only. We can assign a different object by removing the current object from the pointer. 
<p align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/uploads/20191202223147/uniquePtr.png" />
</p>


Now let's create another pointer p2 and we will try to copy the pointer p1 using the assignment operator(=). 
```cpp
#include <iostream>
#include <memory> // for std::unique_ptr
class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};
int main()
{
	std::unique_ptr<Resource> res1{ new Resource{} }; // Resource created here
   std::unique_ptr<Resource> res2 = res1;
	return 0;
} 
```

Output
```
uniquePointer.cpp:12:37: error: use of deleted function 'std::unique_ptr<_Tp, _Dp>::unique_ptr(const std::unique_ptr<_Tp, _Dp>&) [with _Tp = Resource; _Dp = std::default_delete<Resource>]'
   12 |    std::unique_ptr<Resource> res2 = res1;
```
 _**We cannot assign pointer p2 to p1 in case of unique pointers**_

**Unlike std::auto_ptr, std::unique_ptr properly implements move semantics.**
```cpp
#include <iostream>
#include <memory> // for std::unique_ptr
#include <utility> // for std::move

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};

int main()
{
	std::unique_ptr<Resource> res1{ new Resource{} }; // Resource created here
	std::unique_ptr<Resource> res2{}; // Start as nullptr

	std::cout << "res1 is " << (res1 ? "not null\n" : "null\n");
	std::cout << "res2 is " << (res2 ? "not null\n" : "null\n");

	// res2 = res1; // Won't compile: copy assignment is disabled
	res2 = std::move(res1); // res2 assumes ownership, res1 is set to null

	std::cout << "Ownership transferred\n";

	std::cout << "res1 is " << (res1 ? "not null\n" : "null\n");
	std::cout << "res2 is " << (res2 ? "not null\n" : "null\n");

	return 0;
} // Resource destroyed here when res2 goes out of scope
```

Output
```
Resource acquired
res1 is not null
res2 is null
Ownership transferred
res1 is null
res2 is not null
Resource destroyed
```

Because std::unique_ptr is designed with move semantics in mind, copy initialization and copy assignment are disabled. If you want to transfer the contents managed by std::unique_ptr, you must use move semantics. In the program above, we accomplish this via std::move (which converts res1 into an r-value, which triggers a move assignment instead of a copy assignment).

**Passing std::unique_ptr to a function**

If you want the function to take ownership of the contents of the pointer, pass the std::unique_ptr by value. Note that because copy semantics have been disabled, you'll need to use std::move to actually pass the variable in.

```cpp

// This function takes ownership of the Resource, which isn't what we want
void takeOwnership(std::unique_ptr<Resource> res)
{
     if (res)
          std::cout << *res << '\n';
} // the Resource is destroyed here

int main()
{
    auto ptr{ std::make_unique<Resource>() };

//    takeOwnership(ptr); // This doesn't work, need to use move semantics
    takeOwnership(std::move(ptr)); // ok: use move semantics

    std::cout << "Ending program\n";

    return 0;
}
```
Output
```
Resource acquired
I am a resource
Resource destroyed
Ending program
```

Note that in this case, ownership of the Resource was transferred to takeOwnership(), so the Resource was destroyed at the end of takeOwnership() rather than the end of main().

However, most of the time, you won't want the function to take ownership of the resource.

Although you can pass a std::unique_ptr by const reference (which will allow the function to use the object without assuming ownership), it's better to just pass the resource itself (by pointer or reference, depending on whether null is a valid argument). This allows the function to remain agnostic of how the caller is managing its resources.

To get a raw pointer from a std::unique_ptr, you can use the get() member function:

```cpp

// The function only uses the resource, so we'll accept a pointer to the resource, not a reference to the whole std::unique_ptr<Resource>
void useResource(const Resource* res)
{
	if (res)
		std::cout << *res << '\n';
	else
		std::cout << "No resource\n";
}

int main()
{
	auto ptr{ std::make_unique<Resource>() };

	useResource(ptr.get()); // note: get() used here to get a pointer to the Resource

	std::cout << "Ending program\n";

	return 0;
} // The Resource is destroyed here
```
Ouput
```
Resource acquired
I am a resource
Ending program
Resource destroyed
```

**std::make_unique**
Use std::make_unique() instead of creating std::unique_ptr and using new yourself.

```cpp
#include <memory> // for std::unique_ptr and std::make_unique
#include <iostream>

class Fraction
{
private:
	int m_numerator{ 0 };
	int m_denominator{ 1 };

public:
	Fraction(int numerator = 0, int denominator = 1) :
		m_numerator{ numerator }, m_denominator{ denominator }
	{
	}

	friend std::ostream& operator<<(std::ostream& out, const Fraction &f1)
	{
		out << f1.m_numerator << '/' << f1.m_denominator;
		return out;
	}
};


int main()
{
	// Create a single dynamically allocated Fraction with numerator 3 and denominator 5
	// We can also use automatic type deduction to good effect here
	auto f1{ std::make_unique<Fraction>(3, 5) };
	std::cout << *f1 << '\n';

	// Create a dynamically allocated array of Fractions of length 4
	auto f2{ std::make_unique<Fraction[]>(4) };
	std::cout << f2[0] << '\n';

	return 0;
}
```
Output:
```
3/5
0/1
```

### Shared Pointer

`std::shared_ptr` is a smart pointer that retains shared ownership of an object through a pointer. Several `shared_ptr` objects may own the same object. The object is destroyed and its memory deallocated when either of the following happens:

- the last remaining `shared_ptr` owning the object is destroyed;
- the last remaining `shared_ptr` owning the object is assigned another pointer via `operator=` or `reset()`. 

<p align="center">
  <img src="https://learn.microsoft.com/en-us/cpp/cpp/media/shared_ptr.png?view=msvc-170" />
</p>

This example below show us some basic usages of `shared_ptr`:
```cpp
#include <memory>
#include <iostream>
#include <cassert>
#include "MyClass.h"
using namespace std;

void passByValue(shared_ptr<MyClass> sp) {
    cout << "Use count after pass by value: " << sp.use_count() << endl;
}

void passByRef(shared_ptr<MyClass> &sp) {
    cout << "Use count after pass by reference: " << sp.use_count() << endl;
}

int main() {
    // Declaring a shared pointer for an object, using `make_shared` is highly recommended.
    shared_ptr<MyClass> sp1 = make_shared<MyClass>(123, "Content 1");

    // Another way, but not as highly recommended as the fiest approach
    shared_ptr<MyClass> sp2(new MyClass(234, "Content 2"));

    cout << "Current use count of sp1: " << sp1.use_count() << endl;

    // Declaring a shared pointer that take ownership from existing shared_pointer
    // Via copy constructor
    shared_ptr<MyClass> sp3(sp1);

    // Via assignment
    shared_ptr<MyClass> sp4 = sp1;

    cout << "Current use count of sp1: " << sp1.use_count() << endl;

    // Modify the content using 1 shared pointer will affect all other pointers refer to the object
    cout << "Modify using sp3" << endl;
    sp3.get()->setId(1123);
    cout << "Print using sp1: " << endl;
    sp1.get()->printObject();
    
    // Use the reset() function to release the reference of a shared_ptr
    sp4.reset();
    if (sp4.get() == nullptr) {
        cout << "sp4 is now null" << endl;
    }
    cout << "Current use count of sp1: " << sp1.use_count() << endl;

    // Use the swap() function to swaps the managed object of 2 shared_ptr
    sp4.swap(sp3);
    if (sp3.get() == nullptr) {
        cout << "sp3 is now null" << endl;
    }
    cout << "Print by sp4: " << endl;
    sp4.get()->printObject();
    

    // Difference between pass a shared_ptr by value and by reference to a function
    cout << "Use count before pass by value: " << sp1.use_count() << endl;
    passByValue(sp1);
    cout << "Use count before pass by reference: " << sp1.use_count() << endl;
    passByRef(sp1);

    return 0;
}
```

The expected output should be:

```
Current use count of sp1: 1
Current use count of sp1: 3
Modify using sp3
Print using sp1: 
Id: 1123, Content: Content 1
sp4 is now null
Current use count of sp1: 2
sp3 is now null
Print by sp4: 
Id: 1123, Content: Content 1
Use count before pass by value: 2
Use count after pass by value: 3
Use count before pass by reference: 2
Use count after pass by reference: 2
```

More details can be find [here](https://learn.microsoft.com/en-us/cpp/cpp/how-to-create-and-use-shared-ptr-instances?view=msvc-170), refers to the official documents of Microsoft
### Weak Pointer

The `weak_ptr` is one of the smart pointers that provide the capability of a pointer with some reduced risks as compared to the raw pointer. The `weak_ptr`, just like `shared_ptr` has the capability to point to the resource owned by another `shared_ptr` but without owning it. In other words, they are able to create a non-owning reference to the object managed by `shared_ptr`.

To understand the need for `weak_ptr`, we need to first understand the use case of `shared_ptr` which leads to a common problem called a circular link. It occurs when two or more objects reference each other using a `shared_ptr`. For example, if `ObjectA` has a `shared_ptr` to `ObjectB` and `ObjectB` has a `shared_ptr` to `ObjectA`, they form a circular reference. This can be problematic because neither `ObjectA` nor `ObjectB` will ever be deleted, leading to a memory leak.

The following example demonstrates how the weak_ptr solves the circular reference problem:

```cpp
#include <iostream>
#include <memory>
 
using namespace std;
 
// declaring a dummy object
class Object {
public:
    Object(int value) : data(value) {
        cout << "Object created with value: " << data
             << endl;
    }
 
    ~Object() {
        cout << "Object destroyed with value: " << data
             << endl;
    }
    int data;
};
 
int main() {
    // creating shared pointer with resource ownership
    shared_ptr<Object> sharedObjectA = make_shared<Object>(42);
 
    // creating weak pointer to the previously created
    // shared objects
    weak_ptr<Object> weakObjectA = sharedObjectA;
 
    // Access objects using weak_ptr
    if (!weakObjectA.expired()) {
        cout << "The value stored in sharedObjectA: " << (*weakObjectA.lock()).data << endl;
    }
 
    // deleting object
    sharedObjectA.reset();
    cout << "End of the Program";
 
    return 0;
}
```

The expected output should be as follows:

```
Object created with value: 42
The value stored in sharedObjectA: 42
Object destroyed with value: 42
End of the Program
```

For deeper understanding, check out the links attached in the [References](#references) part.

## References:
- [What is Smart Pointer in C++](https://www.geeksforgeeks.org/smart-pointers-cpp/)
- [How to: Create and use shared_ptr](https://learn.microsoft.com/en-us/cpp/cpp/how-to-create-and-use-shared-ptr-instances?view=msvc-170)
- [std::shared_ptr - cppreference.com](https://en.cppreference.com/w/cpp/memory/shared_ptr)
- [auto_ptr in C++](https://www.geeksforgeeks.org/auto-ptr-in-cpp/)
- [22.5 - std::unique_ptr](https://www.learncpp.com/cpp-tutorial/stdunique_ptr/)
- [C++ Functions - Pass By Reference](https://www.geeksforgeeks.org/cpp-functions-pass-by-reference/)