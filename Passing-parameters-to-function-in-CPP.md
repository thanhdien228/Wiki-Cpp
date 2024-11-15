## Overview

In C++, data must be sent to functions when they are called in order to perform operations. This data is called parameters or arguments and there are various parameter passing methods available in C++, each with merits and demerits of its own. In this article, we will discuss various parameter-passing techniques in C++

There are 3 different methods using which we can pass parameters to a function in C++. These are:
1. **Pass by Value**
2. **Pass by Reference**
3. **Pass by Pointers**

## Pass by Value

In **"Pass by Value"** method, a variable's actual value is copied and then passed to the function instead of the original variable. As the result, any changes to the parameter inside the function will not affect the variable's original value outside the function. Although it is easy to understand and implement, this method is not so useful for large size of data structures at it involves copying the value. In addition, it may work unexpectedly in some cases.

This example will demonstrate the troubles when we pass by value in case we need to modify the parameters of a function:

```cpp
#include <iostream> 
using namespace std; 
  
// function to swap variables 
void swap(int a, int b) 
{ 
    int temp = a; 
    a = b; 
    b = temp; 
} 
int main() 
{ 
    int x = 5; 
    int y = 10; 
  
    cout << "Before Swapping:\n"; 
    cout << "x = " << x << ", y = " << y << endl; 
  
    // Call the 'swap' function with pass-by-value 
    // parameters 
    swap(x, y); 
  
    // Print the values of 'x' and 'y' after the 
    // (ineffective) swap The values remain unchanged 
    // because the function works on copies. 
    cout << "After Swapping:\n"; 
    cout << "x = " << x << ", y = " << y << endl; 
  
    return 0; 
}
```

The output will be:
```output
Before Swapping:
x = 5, y = 10
After Swapping:
x = 5, y = 10
```

It is not what we want, because the swap function written has no effect on `a` and `b`. Therefore, pass by value is not a one-for-all solution when passing parameters to function.

## Pass by Reference

### What is Reference in c++

In C++, a reference is an alias for an existing variable. This means that once a reference is initialized to a variable, it becomes another name for that variable. Any operation performed on the reference is actually performed on the original variable. References are created using the ampersand `&` symbol.

**Example**
```cpp
#include <iostream>
using namespace std;

int main()
{
    int x = 10;

    // ref is a reference to x.
    int& ref = x;

    // Value of x is now changed to 20
    ref = 20;
    cout << "x = " << x << '\n';

    // Value of x is now changed to 30
    x = 30;
    cout << "ref = " << ref << '\n';

    return 0;
}
```

**Output**
```
x = 20
ref = 30
```

There are two kinds of references: lvalue references, which refer to a named variable and rvalue references, which refer to a temporary object. The & operator signifies an lvalue reference and the && operator signifies either an rvalue reference, or a universal reference (either rvalue or lvalue) depending on the context. Additionally, a reference holds the address of an object, but behaves syntactically like an object.

Passing by reference allows a function to modify a variable without creating a copy

When a variable is passed as a reference to a function, the address of the variable is stored in a pointer variable inside the function. Hence, the variable inside the function is an alias for the passed variable. Therefore, any operations performed on the variable inside the function will also be reflected in the calling function. 

- This ability to reflect changes could return more than one value by a function. 
- Also, a void function could technically return value/s using this method. 

The & (address of) operator denotes values passed by pass-by-reference in a function. Below is the C++ program to implement pass-by-reference:

```cpp
// C++ program to swap two numbers using
// pass by reference
#include <iostream>
using namespace std;
void swap(int& x, int& y)
{
    int z = x;
    x = y;
    y = z;
}

int main()
{
    int a = 45, b = 35;
    cout << "Before Swap\n";
    cout << "a = " << a << " b = " << b << "\n";

    swap(a, b);

    cout << "After Swap with pass by reference\n";
    cout << "a = " << a << " b = " << b << "\n";
}
```

**Output**
```
Before Swap
a = 45 b = 35
After Swap with pass by reference
a = 35 b = 45
```
## Pass by pointer
### What is Pass by Pointer?
In the function, we pass memory address (**pointer**) of a variable rather than than passing the actual value of variable. This passing technique allows the function to access and modify the content at that particular memory location. It is also called the **call by pointer** method.
### Pass by Pointer (PbP) vs Pass by Reference (PbR)
<table>
<tr>
    <td>Parameters</td>
    <td>Pass by Pointer</td>
    <td>Pass by Reference</td>
    <td>Point for PbP ( + or - )</td>
</tr>
<tr>
    <td>Passing Arguments</td>
    <td>- We pass the <strong>address</strong> of arguments in the function call.</td>
    <td>- We pass the <strong>arguments</strong> in the function call.</td>
    <td>none</td>
</tr>
<tr>
    <td>Accessing Values</td>
    <td>- The value of the arguments is accessed via the <strong>dereferencing operator *</strong></td>
    <td>- The reference name can be used to <strong>implicitly reference </strong> a value.</td>
    <td > - </td>
</tr>
<tr>
    <td>Reassignment</td>
    <td>- Passed parameters <strong>can</strong> be moved/reassigned to a different memory location.</td>
    <td>- Parameters <strong>can't</strong> be moved/reassigned to another memory address.</td>
    <td> + </td>
</tr>
<tr>
    <td>Allowed Values</td>
    <td>- Pointers <strong>can</strong> contain a NULL value, so a passed argument may point to a NULL or even a garbage value.</td>
    <td>- References <strong>cannot</strong> contain a NULL value, so it is guaranteed to have some value.</td>
    <td> + </td>
</tr>
</table>

### Conclusion
Although Pass by pointer entails a series of complex, error-prone operations. The execution time required to pass data by pointer is independent of the data's size. The size of a pointer or an address is small and fixed (e.g., the size of a pointer to an int is the same as a pointer to an enormous structure), so pass-by-pointer is fast and efficient even when passing large data items


### Examples
```cpp
#include <iostream>

// Function to swap the values of two integers using pointers
void swap(int* x, int* y) {
    int temp = *x;
    *x = *y;
    *y = temp;
}

int main() {
    int a = 10, b = 20;
    std::cout << "Before swapping: a = " << a << ", b = " << b << std::endl;
    // Passing the addresses of a and b to the swap function
    swap(&a, &b);
    std::cout << "After swapping: a = " << a << ", b = " << b << std::endl;
    return 0;
}
```
**Output**
```
Before swapping: a = 10, b = 20
After swapping: a = 20, b = 10
```

### Attributes
- **Does not make a copy of the object being pointed to**: Copying a `std::string` may be expensive, so that's something we want to avoid. When we pass a `std::string` by address, we're not copying the actual `std::string` object - we're just copying the pointer (holding the address of the object) from the caller to the called function. Since an address is typically only 4 or 8 bytes, a pointer is only 4 or 8 bytes, so copying a pointer is always fast.
- **Allows the function to modify the argument's value**: When we pass an object by pointer, the function receives the address of the passed object, which it can access via dereferencing. Because this is the address of the actual argument object being passed (not a copy of the object), if the function parameter is a pointer to non-const, the function can modify the argument via the pointer parameter.
- **Null checking**: When passing a parameter by address, care should be taken to ensure the pointer is not a null pointer before you dereference the value.

## References:
- [References in C++](https://www.geeksforgeeks.org/references-in-cpp/)
- [References (C++)](https://learn.microsoft.com/en-us/cpp/cpp/references-cpp?view=msvc-170)
- [Parameter Passing Techniques](https://www.geeksforgeeks.org/parameter-passing-techniques-in-cpp/)
- [Passing By Pointer vs Passing By Reference in C++](https://www.geeksforgeeks.org/passing-by-pointer-vs-passing-by-reference-in-cpp/)