## C++ Arrays
In C++, an array is a data structure that is used to store multiple values of similar data types in a contiguous memory location. 

**Declaration of Arrays**

We can declare an array by simply specifying the data type first and then the name of an array with its size.
```bash
data_type array_name[Size_of_array];
```

**Initialization of Arrays**
* **Initialize Array with Values in C++**:
We have initialized the array with values. The values enclosed in curly braces '{}' are assigned to the array.
    ```cpp
    int arr[5] = {1, 2, 3, 4, 5};
    ```
* **Initialize Array with Values and without Size in C++**:
The length of an array is equal to the number of elements inside curly braces.
    ```cpp
    int arr[] = {1, 2, 3, 4, 5};
    ```
* **Initialize an array partially in C++**:
These values are stored at the first two indices, and at the rest of the indices '0' is stored.
    ```cpp
    int partial_arr[5] = {1, 2};
    ```
* **Initialize the array with zero in C++**:
This will happen in case of zero only if we try to initialize the array with a different value say '2' using this method then '2' is stored at the 0th index only.
    ```cpp
    int zero_arr[5] = {0};
    ```
* **Initialize Array after Declaration (Using Loops)**:
This method is generally used when we want to take input from the user or we cant to assign elements one by one to each index of the array. 
    ```cpp
    for (int i = 0; i < 5; i++) {
        arr[i] = i;
    }
    ```

### Key Properties of Arrays
* An array is a collection of data of the **same data type**, stored at a contiguous memory location.
* Indexing of an array **starts from 0**. It means the first element is stored at the 0th index, the second at 1st, and so on.
* Elements of an array can be **accessed using their indices**.
* Once an array is declared its size **remains constant** throughout the program.
* An array can have **multiple dimensions**.
* The **size** of the array in bytes can be determined by the **sizeof operator** using which we can also find the number of elements in the array.
* We can find the **size** of the type of elements stored in an array by **subtracting adjacent addresses**.
* We can **traverse** over the array with the help of a **loop** using indexing.

**Example**
```cpp
// An array storing different ages
int ages[] = {20, 22, 18, 35, 48, 26, 87, 70};

int i;

// Get the length of the array
int length = sizeof(ages) / sizeof(ages[0]);

// Create a variable and assign the first array element of ages to it
int lowestAge = ages[0];

// Loop through the elements of the ages array to find the lowest age
for (int age : ages) {
  if (lowestAge > age) {
    lowestAge = age;
  }
}

// Print the lowest age
cout << "The lowest age is: " << lowestAge << "\n";
```

***Output***
```
The lowest age is: 18
```

### Relation between Arrays and Pointers in C++
In C++, the array name is **treated as a pointer** that stored the memory address of the first element of the array. So ultimately, arrays are always passed as pointers to the function. In array, elements are stored at contiguous memory locations that's why we can access all the elements of an array using the array name.

**Example**
```cpp
// Passing array as an unsized array argument
void printArray(int arr[], int n)
{
    for (int i = 0; i < n; i++) {
        std::cout << *(arr + i) << " ";
    }
    std::cout << endl;
}

int main()
{
    // Define an array
    int arr[] = { 11, 22, 33, 44 };

    // Print elements of an array
    printArray(arr, 4);

    return 0;
}
```
***Output***
```
11 22 33 44
```

**Advantages of C-style arrays:**

* **Memory Efficiency**: C-style arrays are very lightweight and efficient when it comes to memory usage and performance because additional memory is not needed for metadata or dynamic resizing.
* **Direct Access**: You can efficiently access each element in a C-style array using indexing.
* **Static Size**: The size of a C-style array must be known during compilation, making it useful for applications that require predictable memory usage.

**Disadvantages of C-style arrays:**

* **No Bound Checking**: Accessing elements outside array bounds can lead to undefined behaviors and errors, such as segmentation faults.
* **No Built-in Methods**: C-style arrays don't support standard methods such as pop() and push().
* **Pointer Decay**: When a C-style array is passed to a function, it becomes a pointer to its first element, and the function loses information related to the array's size. You must pass another variable to that function to know the array size.



## C++ std::array

`std::array` is a container that encapsulates fixed size arrays.

This container is an aggregate type with the same semantics as a struct holding a C-style array `T[N]` as its only non-static data member. Unlike a C-style array, it doesn't decay to `T*` automatically. As an aggregate type, it can be initialized with aggregate-initialization given at most `N` initializers that are convertible to `T`:
```cpp
std::array<int, 3> a = {1, 2, 3};
```

The struct combines the performance and accessibility of a C-style array with the benefits of a standard container, such as knowing its own size, supporting assignment, random access iterators, etc.


**Element access**

* **at()** - access specified element with bounds checking
* **operator[]** - access specified element
* **front()** - access the first element
* **back()** - access the last element
* **data()** - direct access to the underlying contiguous storage

**Iterators**

* **begin()** - Returns an iterator pointing to the first element in the vector
* **end()** - Returns an iterator pointing to the theoretical element that follows the last element in the vector
* **cbegin()** - Returns a constant iterator pointing to the first element in the vector.
* **cend()** - Returns a constant iterator pointing to the theoretical element that follows the last element in the vector.
* **rbegin()** - returns a reverse iterator to the beginning
* **rend()** - returns a reverse iterator to the end 

**Capacity**

* **size()** - Returns the number of elements in the vector.
* **max_size()** - Returns the maximum number of elements that the vector can hold.
* **empty()** - Returns whether the container is empty.

**Operations**

* **fill()** - fill the container with specified value
* **swap()** - swaps the contents

**Example**

In C++, we can use the Standard Template Library to implement some of the commonly used algorithms. 

```cpp
#include <iostream>
#include <algorithm>
#include <numeric>
#include <array>

int main(){
    std::array<int, 5> age = {45, 23, 66, 87, 21};

    // sorting
    sort(age.begin(), age.end());

    // print the sorted array
    std::cout << "The sorted array is: ";
    for(const int elt: age){
        std::cout << elt << " ";
    }
    std::cout << std::endl;

    // searching
   // checking whether the number 5  exists or not
    auto found = std::find(age.begin(), age.end(), 5);

    if (found != age.end()) std::cout << "5 was Found" << std::endl;
    else std::cout << "5 was Not Found" << std::endl;

    // summing
    int sum = std::accumulate(age.begin(), age.end(), 0);

    std::cout << "The sum of the element of array is : " << std::sum;
}
```

***Output***
```
The sorted array is: 21 23 45 66 87 
5 was Not Found
The sum of the element of array is : 242
```

**Advantages of std::array:**

* **Efficiency**: C++ arrays are more efficient than vectors due to the absence of dynamic sizing overhead.
* **Bounds Checking**: In C++11, compile-time bounds checking was added to the std::array container.
* **Memory Safety**: std::array allocates memory on the stack, so you don't have to manage memory allocation manually.

**Disadvantages of std::array:**

* **Fixed Size**: Like C-style arrays, the size of a std::array must be known during compilation.
* **Memory Overhead**: Due to the safety features and metadata, std::array has a slight overhead compared to C-style arrays.
* **Incompatible with C Libraries**: std::array are specific to C++ and cannot be used directly with C-specific libraries. 



## C++ std::vector

Vectors are the same as dynamic arrays with the ability to resize themselves automatically when an element is inserted or deleted, with their storage being handled automatically by the container. Vector elements are placed in contiguous storage so that they can be accessed and traversed using iterators.

**Key Characteristics of Vectors**
* **Dynamic Sizing:** Vectors automatically resize when you add or remove elements. This flexibility is one of the main reasons vectors are preferred over traditional arrays in many situations.
* **Efficient Access:** Since vector elements are stored in contiguous memory locations, you can access elements in constant time using their index, similar to arrays.
* **Insertion and Deletion:** Insertion of elements is typically done at the end of the vector, where it operates in constant time. However, inserting or deleting elements at the beginning or middle of the vector takes linear time due to the need to shift elements

**Iterators**

* begin() - Returns an iterator pointing to the first element in the vector
* end() - Returns an iterator pointing to the theoretical element that follows the last element in the vector


**Capacity**
* size() - Returns the number of elements in the vector.
* max_size() - Returns the maximum number of elements that the vector can hold.
* resize(n) - Resizes the container so that it contains 'n' elements.
* empty() - Returns whether the container is empty.
* reserve() - Requests that the vector capacity be at least enough to contain n elements.

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> g1;

    for (int i = 1; i <= 5; i++)
        g1.push_back(i);

    cout << "Size : " << g1.size();
    cout << "\nMax_Size : " << g1.max_size();

    // resizes the vector size to 4
    g1.resize(4);

    // prints the vector size after resize()
    cout << "\nSize : " << g1.size();

    // checks if the vector is empty or not
    if (g1.empty() == false)
        cout << "\nVector is not empty";
    else
        cout << "\nVector is empty";

    cout << "\nVector elements are: ";
    for (auto it = g1.begin(); it != g1.end(); it++)
        cout << *it << " ";

    return 0;
}
```
Output
```
Size : 5
Max_Size : 4611686018427387903
Size : 4
Vector is not empty
Vector elements are: 1 2 3 4
```
**Modifiers**
* assign() - It assigns new value to the vector elements by replacing old ones 
* push_back() - It push the elements into a vector from the back 
* pop_back() - It is used to pop or remove elements from a vector from the back.
* insert() - It inserts new elements before the element at the specified position 
* erase() - It is used to remove elements from a container from the specified position or range.
* swap() - It is used to swap the contents of one vector with another vector of same type.Sizes may differ.
* clear() - It is used to remove all the elements of the vector container 

```cpp
// C++ program to illustrate the
// Modifiers in vector
#include <bits/stdc++.h>
#include <vector>
using namespace std;

int main()
{
    // Assign vector
    vector<int> v;

    // fill the vector with 10 five times
    v.assign(5, 10);

    cout << "The vector elements are: ";
    for (int i = 0; i < v.size(); i++)
        cout << v[i] << " ";

    // inserts 15 to the last position
    v.push_back(15);
    int n = v.size();
    cout << "\nThe last element is: " << v[n - 1];

    // removes last element
    v.pop_back();

    // prints the vector
    cout << "\nThe vector elements are: ";
    for (int i = 0; i < v.size(); i++)
        cout << v[i] << " ";

    // inserts 5 at the beginning
    v.insert(v.begin(), 5);

    cout << "\nThe first element is: " << v[0];

    // removes the first element
    v.erase(v.begin());

    cout << "\nThe first element is: " << v[0];

    // erases the vector
    v.clear();
    cout << "\nVector size after clear(): " << v.size();

    // two vector to perform swap
    vector<int> v1, v2;
    v1.push_back(1);
    v1.push_back(2);
    v2.push_back(3);
    v2.push_back(4);

    cout << "\n\nVector 1: ";
    for (int i = 0; i < v1.size(); i++)
        cout << v1[i] << " ";

    cout << "\nVector 2: ";
    for (int i = 0; i < v2.size(); i++)
        cout << v2[i] << " ";

    // Swaps v1 and v2
    v1.swap(v2);

    cout << "\nAfter Swap \nVector 1: ";
    for (int i = 0; i < v1.size(); i++)
        cout << v1[i] << " ";

    cout << "\nVector 2: ";
    for (int i = 0; i < v2.size(); i++)
        cout << v2[i] << " ";
}

```
Output
```
The vector elements are: 10 10 10 10 10 
The last element is: 15
The vector elements are: 10 10 10 10 10 
The first element is: 5
The first element is: 10
Vector size after erase(): 0

Vector 1: 1 2 
Vector 2: 3 4 
After Swap 
Vector 1: 3 4 
Vector 2: 1 2
```

**Advantages of std::vector:**

* **Dynamic Size**: You can change the size of a vector at runtime by adding or removing elements. This is ideal in situations where the size of an array may not be known during compilation.
* **Memory Management**: Internally, vectors are created on the heap memory, and they are automatically managed, preventing any memory leaks.
* **Standard Functions**: You don't have to implement basic operations, as Vectors have built-in methods for searching, erasing, and inserting elements.

**Disadvantages of std::vector:**

* **Operational Overhead**: Vectors have slower performance compared to C-style arrays due to the overhead caused by dynamic sizing and safety features.
* **Fragmentation**: On frequent sizing changes in a vector, the memory has to be reallocated multiple times, leading to gaps in the primary memory.
* **Incompatible with C Libraries**: Vectors are specific to C++ and cannot be used directly with C-specific libraries. 
