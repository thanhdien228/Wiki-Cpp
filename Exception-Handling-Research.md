## What is a Exception ? What is Exception Handling ?

- An **exception** is a problem that arises during the execution of a program. A C++ exception is a response to an exceptional circumstance that arises while a program is running, such as an attempt to divide by zero. Exception occurs during the running of the program (runtime).

- **Exception handling** is the process of handling those exceptions in order to keep our program running when an exception occurs.

- There are 2 types of exceptions:
    - **Synchronous exception:** Exceptions that happen when something goes wrong because of a mistake in the input data or when the program is not equipped to handle the current type of data it's working with
    - **Asynchronous exception:** Exceptions that are beyond the program's control, such as disc failure, keyboard interrupts... 
    
## Try and Catch

- C++ provides an inbuilt feature for Exception Handling. It can be done using the following specialized keywords: try, catch, and throw with each having a different purpose.
- Syntax of try-catch in C++
```cpp
try {         
     // Code that might throw an exception
     throw SomeExceptionType("Error message");
 } 
catch( ExceptionName e1 )  {   
     // catch block catches the exception that is thrown from try block
 } 
```

### 1. try in C++

- The try keyword represents a block of code that may throw an exception placed inside the try block. It's followed by one or more catch blocks. If an exception occurs, try block throws that exception.

### 2. catch in C++

- The catch statement represents a block of code that is executed when a particular exception is thrown from the try block. The code to handle the exception is written inside the catch block.

### 3. throw in C++

- An exception in C++ can be thrown using the throw keyword. 
- When a program encounters a throw statement, then it immediately terminates the current function and starts finding a matching catch block to handle the thrown exception.
- Multiple catch statements can be used to catch different type of exceptions thrown by try block.
- The try and catch keywords come in pairs: We use the try block to test some code and If the code throws an exception we will handle it in our catch block.

## Why do we need Exception Handling in C++?


Suppose we have **caculate** calling **devide** with some input. Now, suppose **devide** fails for some reason.

Suggestion is to handle the failure within **devide**, and then return to **caculate**. How will **caculate** "know" what error (if any) has occurred in **devide** and how to proceed from that point?

The first solution that comes to mind is an error-code that **devide** will return, where typically, a zero value will represent "OK", and each of the other (non-zero) values will represent a specific error that has occurred.

```cpp
#include <iostream>
using namespace std;
int devide(int numerator, int denominator){
    if(denominator==0)
        return -1;
    else return numerator/denominator;
}

int caculate(int firstNumber, int secondNumber, char sign){
    int res;
    if(sign = '/'){
        res = devide(firstNumber,secondNumber);
    }
    return res;
}

int main()
{
	int res = caculate(10,0,'/');
    cout << res;

	return 0;
} 
```

The problem with this mechanism is that it limits our flexibility in adding / handling new error-codes.

With the exception mechanism, you have a generic Exception object, which can be extended to any specific type of exception. In a way, it is similar to an error-code, but it can contain more information (for example, an error-message string).


## Properties of Exception Handling in C++

### Property 1

**There is a special catch block called the 'catch-all' block, written as `catch(…)`, that can be used to catch all types of exceptions.**

In the following program, an int is thrown as an exception, but there is no catch block for int, so the `catch(…)` block will be executed. 

```cpp
#include <iostream>

int main() {
    try {
        // Attempt to perform some operation that may throw an exception
        throw 42; // Throwing an integer as an example

    } catch (const std::invalid_argument& e) {
        std::cout << "Caught invalid_argument: " << e.what() << std::endl;

    } catch (const std::runtime_error& e) {
        std::cout << "Caught runtime_error: " << e.what() << std::endl;

    } catch (...) {
        // Catch all other types of exceptions
        std::cout << "Caught an unknown exception" << std::endl;
    }

    return 0;
}
```

**Output**
```
Caught an unknown exception
```

### Property 2

**Implicit type conversion doesn't happen for primitive types.**

In the following program, 'a' is not implicitly converted to int.

```cpp
#include <iostream>
using namespace std;

int main()
{
    try {
        throw 'a';
    }
    catch (int x) {
        cout << "Caught " << x;
    }
    catch (...) {
        cout << "Default Exception\n";
    }
    return 0;
}
```

**Output**
```
Default Exception
```

### Property 3

**If an exception is thrown and not caught anywhere, the program terminates abnormally.**

```cpp

// C++ program to demonstate property 3: If an exception is
// thrown and not caught anywhere, the program terminates
// abnormally in exception handling.

#include <iostream>
using namespace std;

int main()
{
    try {
        throw 'a';
    }
    catch (int x) {
        cout << "Caught ";
    }
    return 0;
}

```
**Output**
```bash
terminate called after throwing an instance of 'char'
```

### Property 4

**In C++, all exceptions are unchecked, i.e., the compiler doesn't check whether an exception is caught or not. So, it is not necessary to specify all uncaught exceptions in a function declaration. However,exception-handling it's a recommended practice to do so.**

```cpp
// C++ program to demonstate property 4 in better way

#include <iostream>
using namespace std;

// Here we specify the exceptions that this function
// throws.
void fun(int* ptr, int x) throw(
    int*, int) // Dynamic Exception specification
{
    if (ptr == NULL)
        throw ptr;
    if (x == 0)
        throw x;
    /* Some functionality */
}

int main()
{
    try {
        fun(NULL, 0);
    }
    catch (...) {
        cout << "Caught exception from fun()";
    }
    return 0;
}

```
**Output**
```bash
Caught exception from fun()
```

### Property 5

In C++, try/catch blocks can be nested. Also, an exception can be re-thrown using "throw; ". 

```cpp
#include <iostream>
using namespace std;

int main()
{

    // nesting of try/catch
    try {
        try {
            throw 20;
        }
        catch (int n) {
            cout << "Handle Partially ";
            throw; // Re-throwing an exception
        }
    }
    catch (int n) {
        cout << "Handle remaining ";
    }
    return 0;
}
```
The output will be:
```{bash}
Handle Partially Handle remaining 
```
### Property 6

When an exception is thrown, all objects created inside the enclosing try block are destroyed before the control is transferred to the catch block.

```cpp
// C++ program to demonstrate

#include <iostream>
using namespace std;

// Define a class named Test
class Test {
public:
    // Constructor of Test
    Test() { cout << "Constructor of Test " << endl; }
    // Destructor of Test
    ~Test() { cout << "Destructor of Test " << endl; }
};

int main()
{
    try {
        // Create an object of class Test
        Test t1;

        // Throw an integer exception with value 10
        throw 10;
    }
    catch (int i) {
        // Catch and handle the integer exception
        cout << "Caught " << i << endl;
    }
}
```
The output will be:
```{bash}
Constructor of Test 
Destructor of Test 
Caught 10
```

## Limitations of Exception Handling

The exception handling also have few limitations:

- Exceptions may break the structure or flow of the code as multiple invisible exit points are created in the code which makes the code hard to read and debug.
- If exception handling is not done properly can lead to resource leaks as well.
- It's hard to learn how to write Exception code that is safe.
- There is no C++ standard on how to use exception handling, hence many variations in exception-handling practices exist.

## References
- [Exception Handling in C++](https://www.geeksforgeeks.org/exception-handling-c/)
- [8.1 Synchronous and Asynchronous Exceptions](https://docs.oracle.com/cd/E19205-01/819-5267/bkahc/index.html)