# Translation Unit

A translation unit is a file containing source code, header files and other dependencies. All of these sources are grouped together to form a single translation unit which can then be used by the compiler to produce one single executable object. It is important to link the sources together in a meaningful way.

# Linkage

Linkage describes how names can or can not refer to the same entity throughout the whole program or one single translation unit.

## Internal Linkage

An identifier implementing internal linkage is not accessible outside the translation unit it is declared in. Any identifier within the unit can access an identifier having internal linkage. <br>
It is implemented by the keyword `static`. An internally linked identifier is stored in initialized or uninitialized segment of RAM. <br>

*Note* <br>
Text Segment: A text segment, also known as a code segment or simply as text, is one of the sections of a program's memory which contains executable instructions.<br>
Initialized Data Segment: is a portion of the virtual address space of a program, which contains the global variables and static variables that are initialized by the programmer.<br>
Uninitialized Data Segment: contains all global variables and static variables that are initialized to zero or do not have explicit initialization in source code.

**Example**
```cpp
// animal.cpp
#include <iostream>

static int animals = 8;
const int i = 5;

static void call_me()
{
    printf("%d %d\n", i, animals);
}
```

```cpp
// feed.cpp
#include <iostream>
#include "animal.cpp"

int main()
{
    call_me();
    animals = 3; 
    call_me();
    return 0;
}

```

**Output**
```{output}
5 8
5 3 
```

### Usage

An internally linked variable is passed by copy.<br>
Thus, if a header file has a function `fun1()` and the source code in which it is included in also has `fun1()` but with a different definition, then the 2 functions will not clash with each other. <br> <br>
Use internal linkage to hide translation-unit-local helper functions from the global scope.<br>
## External Linkage

An identifier implementing external linkage is visible to every translation unit. Externally linked identifiers are shared between translation units and are considered to be located at the outermost level of the program. <br>

In practice, this means that you must define an identifier in a place which is visible to all, such that it has only one visible definition. It is the default linkage for globally scoped variables and functions. Thus, all instances of a particular identifier with external linkage refer to the same identifier in the program.<br>

The keyword extern implements external linkage. When we use the keyword `extern`, we tell the linker to look for the definition elsewhere.<br>

Thus, the declaration of an externally linked identifier does not take up any space. `Extern` identifiers are generally stored in initialized/uninitialized or text segment of RAM.

```cpp
//feed.cpp
#include <iostream>

int animals = 8;
extern const int i = 5;
void call_me()
{
    printf("%d %d\n", i, animals);
}
```

```cpp
// animal.cpp
#include <iostream>
void call_me(); // forward declaration
int main()
{
    call_me(); // call function
    // extern int animals;
    // extern const int i;
    extern int animals = 1; // ERROR: an initializer is not allowed on a local declaration of an extern variableC/C++(2442)
    return 0;
}
```
```{output}
5 8
```

## const
The const keyword specifies that a variable's value is constant and tells the compiler to prevent the programmer from modifying it.

```cpp
// constant_values1.cpp
int main() {
   const int i = 5;
   i = 10;   // C3892: A const variable cannot be changed after it is declared and initialized.
   i++;   // C2105: The operator must have an l-value as operand.
}
```

A const global variable has internal linkage by default. If you want the variable to have external linkage, apply the extern keyword to the definition, and to all other declarations in other files
```cpp
//fileA.cpp
extern const int i = 42; // extern const definition

//fileB.cpp
extern const int i;  // declaration only. same as i in FileA
```

Declaring a member function with the `const` keyword specifies that the function is a "read-only" function that doesn't modify the object for which it's called. <br>
A constant member function can't modify any non-static data members or call any member functions that aren't constant. <br>
To declare a constant member function, place the `const` keyword after the closing parenthesis of the argument list. The `const` keyword is required in both the declaration and the definition. <br>

```cpp
// constant_member_function.cpp
class Date
{
public:
   Date( int mn, int dy, int yr );
   int getMonth() const;     // A read-only function
   void setMonth( int mn );   // A write function; can't be const
private:
   int month;
};

int Date::getMonth() const
{
   return month;        // Doesn't modify anything
}
void Date::setMonth( int mn )
{
   month = mn;          // Modifies data member
}
int main()
{
   Date MyDate( 7, 4, 1998 );
   const Date BirthDate( 1, 18, 1953 );
   MyDate.setMonth( 4 );    // Okay
   BirthDate.getMonth();    // Okay
   BirthDate.setMonth( 4 ); // C2662 Error: the object has type qualifiers that are not compatible with the member function "Date::setMonth"
}
```

## constexpr 

Like `const`, it can be applied to variables: A compiler error is raised when any code attempts to modify the value. <br>
Unlike `const`, `constexpr` can also be applied to functions and class constructors. `constexpr` indicates that the value, or return value, is constant and, where possible, is computed at compile time.<br>
A `constexpr` integral value can be used wherever a const integer is required, such as in template arguments and array declarations. And when a value is computed at compile time instead of run time, it helps your program run faster and use less memory.

```cpp
constexpr float x = 42.0;
constexpr float y{108};
constexpr float z = exp(5, 3);
constexpr int i; // Error! Not initialized
int j = 0;
constexpr int k = j + 1; //Error! j not a constant expression

constexpr float exp(float x, int n)
{
    return n == 0 ? 1 :
        n % 2 == 0 ? exp(x * x, n / 2) :
        exp(x * x, (n - 1) / 2) * x;
}
```

The compiler always gave a `constexpr` variable internal linkage, even when the variable was marked `extern`.
```cpp
extern constexpr int x = 10; //error LNK2005: "int const x" already defined
```

A `constexpr` function or constructor is implicitly `inline`.<br>

The following rules apply to `constexpr` functions:
- A `constexpr` function must accept and return only literal types.
- A `constexpr` function can be recursive.
- Before C++20, a `constexpr` function can't be virtual, and a constructor can't be defined as `constexpr` when the enclosing class has any virtual base classes. In C++20 and later, a `constexpr` function can be virtual. Visual Studio 2019 version 16.10 and later versions support `constexpr` virtual functions when you specify the /std:c++20 or later compiler option.
- The body can be defined as `= default` or = `delete`.
- The body can contain no `goto` statements or `try` blocks.
- An explicit specialization of a non-`constexpr` template can be declared as `constexpr`:
- An explicit specialization of a `constexpr` template doesn't also have to be `constexpr`:

The following rules apply to `constexpr` functions in Visual Studio 2017 and later:
- It may contain `if` and `switch` statements, and all looping statements including `for`, range-based `for`, `while`, and `do-while`.
- It may contain local variable declarations, but the variable must be initialized. It must be a literal type, and can't be `static` or thread-local. The locally declared variable isn't required to be `const`, and may mutate.
- A `constexpr` non-`static` member function isn't required to be implicitly `const`.

## inline
The `inline` keyword suggests that the compiler substitute the code within the function definition in place of each call to that function.<br>
In theory, using inline functions can make your program faster because they eliminate the overhead associated with function calls. Calling a function requires pushing the return address on the stack, pushing arguments onto the stack, jumping to the function body, and then executing a return instruction when the function finishes. This process is eliminated by inlining the function. The compiler also has different opportunities to optimize functions expanded inline versus those that aren't. A tradeoff of inline functions is that the overall size of your program can increase.<br>
A function defined in the body of a class declaration is implicitly an inline function.

```cpp
// account.h
class Account
{
public:
    Account(double initial_balance)
    {
        balance = initial_balance;
    }

    double GetBalance() const;
    double Deposit(double amount);
    double Withdraw(double amount);

private:
    double balance;
};

inline double Account::GetBalance() const
{
    return balance;
}

inline double Account::Deposit(double amount)
{
    balance += amount;
    return balance;
}

inline double Account::Withdraw(double amount)
{
    balance -= amount;
    return balance;
}
```

### Usage

Inline functions are best used for small functions, such as those that provide access to data members. <br>
Short functions are sensitive to the overhead of function calls. <br>
Longer functions spend proportionately less time in the calling and returning sequence and benefit less from inlining. <br>

```cpp
class Point
{
public:
    // Define "accessor" functions
    // as reference types.
    unsigned& x();
    unsigned& y();

private:
    unsigned _x;
    unsigned _y;
};

inline unsigned& Point::x()
{
    return _x;
}

inline unsigned& Point::y()
{
    return _y;
}
```

Specifying the two accessor functions (x and y in the preceding example) as inline typically saves the overhead on:
- Function calls (including parameter passing and placing the object's address on the stack)
- Preservation of caller's stack frame
- New stack frame setup
- Return-value communication
- Restoring the old stack frame
- Return