# Auto

## Definition

- The `auto` keyword in C++ specifies that the type of the variable that is being declared will be automatically deducted from its initializer.
- In the case of functions, if their return type is `auto` then that will be evaluated by return type expression at runtime.

## Benefit

- Robustness: If the expression's type is changed—including when a function return type is changed—it just works.
- Performance: You're guaranteed that there's no conversion.
- Usability: You don't have to worry about type name spelling difficulties and typos.
- Efficiency: Your coding can be more efficient.

## Usage

- The `auto` keyword is a placeholder for a type, but it isn't itself a type. Therefore, the `auto` keyword can't be used in casts or operators such as `sizeof` and (for C++/CLI) `typeid`.
- To avoid long initializations when creating iterators for containers.

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main()
{
    vector<string> vt({ "Hello", "mother", "father"});

    // "it" is std::vector<int>::iterator
    for (auto it = vt.begin(); it != vt.end(); it++)
        cout << *it << endl;

    return 0;
}
```

**Output**

```{output}
Hello
mother
father
```

- Using `auto` drops references, `const` qualifiers, and `volatile` qualifiers.
To make it reference type, we use `auto&`.

```cpp
#include <iostream>

using namespace std;

int main( )
{
    int count = 10;
    int& countRef = count;
    auto myAuto = countRef;

    countRef = 11;
    cout << count << " ";

    myAuto = 12;
    cout << count << endl;
}
```

**Output**

```{output}
11 11
```

```cpp
#include <iostream>

using namespace std;

int main( )
{
    int count = 10;
    int& countRef = count;
    auto& myAuto = countRef;

    countRef = 11;
    cout << count << " ";

    myAuto = 12;
    cout << count << endl;
}
```

**Output**

```{output}
11 12
```

# Lambda

## Definition

- A lambda expression is a convenient way of defining an anonymous function object right at the location where it's invoked or passed as an argument to a function.
- Syntax

```{output}
[ capture clause ] (parameters) -> return-type  
{   
   definition of method   
} 
```

### Capture clause

- Specifying which variables are captured, and whether the capture is by value or by reference.
    + `[ ]`: body of the lambda expression accesses no variables in the enclosing scope.
    + `[&]`: all variables that you refer to are captured by reference.
    + `[=]`: all variables are captured by value. Can use a default capture mode.
- To use lambda expressions in the body of a class member function, pass the `this` pointer to the capture clause to provide access to the member functions and data members of the enclosing class.
- An identifier or `this` can't appear more than once in a capture clause.

Example: `total` by reference and `factor` by value
```cpp
[&total, factor]
[factor, &total]
[&, factor]
[=, &total]

[&, &total] // ERROR
[=, factor] // ERROR

[=, this]{};   // ERROR: this when = is the default
[=, *this]{ }; // OK: captures this by value.
```

### Parameter list

- A parameter list is optional and in most aspects resembles the parameter list for a function.

```cpp
auto y = [] (int first, int second)
{
    return first + second;
};
```

### Return type

- You can omit the return type because it is automatically deduced.
- If the lambda body contains one return statement, the compiler deduces the return type from the type of the return expression. Otherwise, the compiler deduces the return type as `void`.
- A lambda expression can take another lambda expression as its argument or returns a lambda expression.

**Example**
```cpp
#include <iostream>
#include <functional>

int main()
{
    using namespace std;

    // The following code declares a lambda expression that returns
    // another lambda expression that adds two numbers.
    // The returned lambda expression captures parameter x by value.
    auto addtwointegers = [](int x) -> function<int(int)> {
        return [=](int y) { return x + y; };
    };

    // The following code declares a lambda expression that takes another
    // lambda expression as its argument.
    // The lambda expression applies the argument z to the function f
    // and multiplies by 2.
    auto higherorder = [](const function<int(int)>& f, int z) {
        return f(z) * 2;
    };

    // Call the lambda expression that is bound to higherorder.
    auto answer = higherorder(addtwointegers(7), 8);

    // Print the result, which is (7+8)*2.
    cout << answer << endl;
}
```

**Output**

```{output}
30
```

### Lambda body

- The lambda body of a lambda expression is a compound statement. It can contain anything that's allowed in the body of an ordinary function or member function.
- The body can access these kinds of variables:
    + Captured variables from the enclosing scope, as described previously.
    + Parameters.
    + Locally declared variables.
    + Class data members, when declared inside a class and `this` is captured.
    + Any variable that has static storage duration—for example, global variables.
- The `mutable` specification enables the body of a lambda expression to modify variables that are captured by value.

```cpp
#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   // mutable allows n (read only) to change value
   [&, n] (int a) mutable { m = ++n + a; }(4);
   cout << m << endl << n << endl;
}
```

**Output**
```{output}
5
0
```

**Example of using a Lambda Expression in a Function**
- Use `this` explicitly
```cpp
// capture "this" by reference
void ApplyScale(const vector<int>& v) const
{
   for_each(v.begin(), v.end(),
      [this](int n) { cout << n * _scale << endl; });
}

// capture "this" by value (Visual Studio 2017 version 15.3 and later)
void ApplyScale2(const vector<int>& v) const
{
   for_each(v.begin(), v.end(),
      [*this](int n) { cout << n * _scale << endl; });
}
```

- Use `this` implicity
```cpp
void ApplyScale(const vector<int>& v) const
{
   for_each(v.begin(), v.end(),
      [=](int n) { cout << n * _scale << endl; });
}
```

**Full code**
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

class Scale
{
public:
    // The constructor.
    explicit Scale(int scale) : _scale(scale) {}

    // Prints the product of each element in a vector object
    // and the scale value to the console.
    void ApplyScale(const vector<int>& v) const
    {
        for_each(v.begin(), v.end(), [*this](int n) { cout << n * _scale << endl; });
    }

private:
    int _scale;
};

int main()
{
    vector<int> values;
    values.push_back(1);
    values.push_back(2);
    values.push_back(3);
    values.push_back(4);

    // Create a Scale object that scales elements by 3 and apply
    // it to the vector object. doesn't modify the vector.
    Scale s(3);
    s.ApplyScale(values);
}
```

**Output**
```{output}
3
6
9
12
```

## Benefit & Usage

- Lambda expressions can be defined inline where they are used which makes the code more readable as the operation is defined exactly where it is invoked.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector<int> nums = { 1, 2, 3, 4, 5 };

    for_each(nums.begin(), nums.end(),
             [](int n) { cout << n << " "; });

    return 0;
}
```

**Output**
```{output}
1 2 3 4 5 
```

- If an operation is only used in a limited scope and doesn't need to be reused elsewhere, a lambda expression can be a more concise option than functions.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector<int> vec = { 1, 2, 3, 4, 5 };
    cout << "Even Numbers: ";

    for_each(vec.begin(), vec.end(), [](int x) {

        if (x % 2 == 0) {
            cout << x << " ";
        }
    });
    cout << endl;
    return 0;
}
```

**Output**
```{output}
Even Numbers: 2 4 
```

- Lambda expressions can capture local variables from their enclosing scope, which isn't possible with normal functions and it is very useful when the operation depends on some local state.

```cpp
#include <iostream>
using namespace std;

int main()
{

    int multiplier = 5;
    // Define a lambda function capturing the local variable
    // 'multiplier'
    auto multiply
        = [multiplier](int n) { return n * multiplier; };

    cout << "3 multiplied by 5 is " << multiply(3) << endl;
    return 0;
}
```

**Output**
```{output}
3 multiplied by 5 is 15
```

- Many standard library algorithms like std::sort, std::for_each, std::count_if that take functions as arguments there lambda expressions can be used to pass custom operations to these algorithms.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int main()
{

    vector<int> nums = { 1, 2, 3, 4, 5 };

    cout << "Count of even numbers: "
         << count_if(nums.begin(), nums.end(),
                     [](int n) { return n % 2 == 0; })
         << endl;
    return 0;
}
```

**Output**
```{output}
Count of even numbers: 2
```

- Lambda expressions are essentially unnamed function objects and they can be stored and passed around like objects, and invoked later.

```cpp
#include <functional>
#include <iostream>
using namespace std;

int main()
{
    // Define a function object using std::function that
    // holds a lambda function
    function<void()> printHello
        = []() { cout << "Hello, World!" << endl; };

    // Call the lambda function through the function object
    printHello();
    return 0;
}
```

**Output**
```{output}
Hello, World!
```
# Reference
[auto (C++)](https://learn.microsoft.com/en-us/cpp/cpp/auto-cpp?view=msvc-170) <br>
[Type Inference in C++ (auto and decltype)](https://www.geeksforgeeks.org/type-inference-in-c-auto-and-decltype/) <br>
[Lambda expression in C++_GeeksforGeeks](https://www.geeksforgeeks.org/lambda-expression-in-c/) <br>
[Lambda expression in C++_MS](https://learn.microsoft.com/en-us/cpp/cpp/lambda-expressions-in-cpp?view=msvc-170) <br>
[When to Use Lambda Expressions Instead of Functions in C++?](https://www.geeksforgeeks.org/when-to-use-lambda-expressions-instead-of-functions-in-cpp/)