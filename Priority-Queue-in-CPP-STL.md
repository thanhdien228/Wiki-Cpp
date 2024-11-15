## `heap` - the base concept

A Heap is a special Tree-based Data Structure that has the following properties:
- It is a Complete Binary Tree.
- It either follows max heap or min heap property.

**Max-Heap:** The value of the root node must be the greatest among all its descendant nodes and the same thing must be done for its left and right sub-tree also.

**Min-Heap:** The value of the root node must be the smallest among all its descendant nodes and the same thing must be done for its left and right sub-tree also.

Properties of `heap`:
- The minimum or maximum element is always at the root of the heap, allowing constant-time access (O(1) time complexity).
- As the tree is complete binary, all levels are filled except possibly the last level. And the last level is filled from left to right.
- Insertion, deletion and heapify function all have an O(logn) time complexity.

## `priority_queue` - the implementation of `heap`

- A C++ priority queue is a type of container adapter, specifically designed such that the first element of the queue is either the greatest or the smallest of all elements in the queue
- Priority queues are built on the top of the max heap and use an array or vector as an internal structure. In simple terms, STL Priority Queue is **the implementation of Heap Data Structure.**

## `priority_queue` in C++ STL

### Declaration

**Max Heap:**

Syntax:
```cpp
std::priority_queue<int> pq;
```

Example:
```cpp
#include <iostream>
#include <queue>
using namespace std;

int main()
{
    int arr[6] = { 10, 2, 4, 8, 6, 9 };

    // defining priority queue
    priority_queue<int> pq;

    // printing array
    cout << "Array: ";
    for (auto i : arr) {
        cout << i << ' ';
    }
    cout << endl;
    // pushing array sequentially one by one
    for (int i = 0; i < 6; i++) {
        pq.push(arr[i]);
    }

    // printing priority queue
    cout << "Priority Queue: ";
    while (!pq.empty()) {
        cout << pq.top() << ' ';
        pq.pop();
    }

    return 0;
}
```

Output:
```
Array: 10 2 4 8 6 9 
Priority Queue: 10 9 8 6 4 2 
```

Min Heap:

Syntax:
```cpp
priority_queue <int, vector<int>, greater<int>> gq;
```

Example:
```cpp
#include <iostream>
#include <queue>
using namespace std;

void showpq(
    priority_queue<int, vector<int>, greater<int> > g)
{
    while (!g.empty()) {
        cout << ' ' << g.top();
        g.pop();
    }
    cout << '\n';
}

void showArray(int* arr, int n)
{
    for (int i = 0; i < n; i++) {
        cout << arr[i] << ' ';
    }
    cout << endl;
}

int main()
{
    int arr[6] = { 10, 2, 4, 8, 6, 9 };
    priority_queue<int, vector<int>, greater<int> > gquiz(
        arr, arr + 6);

    cout << "Array: ";
      showArray(arr, 6);

    cout << "Priority Queue : ";
    showpq(gquiz);

    return 0;
}
```

Output:
```
Array: 10 2 4 8 6 9 
Priority Queue :  2 4 6 8 9 10
```

### Methods on Priority Queue in C++:

These are some methods that are usually used when working with `std::priority_queue`:

| Method    | Definition                                                                      |
|-----------|---------------------------------------------------------------------------------|
| empty()   | Returns whether the pq is empty.                                                |
| size()    | Returns the size of the pq.                                                     |
| top()     | Returns a reference to the topmost element of the pq.                           |
| push()    | Pushes an object to the pq and sorts the pq.                                    |
| pop()     | Deletes the first element of the pq.                                            |
| swap()    | Swaps the content of 2 pq. They must be of the same type.                       |
| emplace() | Receives parameters, create and insert a new object pq and  sorts the container |

Examples of blocks of code that used some methods above:

Example 1:
```cpp
#include <iostream>
#include <queue>
using namespace std;

// Implementation of priority queue
void showpq(priority_queue<int> gq)
{
    priority_queue<int> g = gq;
    while (!g.empty()) {
        cout << ' ' << g.top();
        g.pop();
    }
    cout << '\n';
}

// Driver Code
int main()
{
    priority_queue<int> gquiz;
    // used in inserting the element
    gquiz.push(10);
    gquiz.push(30);
    gquiz.push(20);
    gquiz.push(5);
    gquiz.push(1);

    cout << "The priority queue gquiz is : ";
    
    // used for highlighting the element
    showpq(gquiz);

    // used for identifying the size 
    // of the priority queue
    cout << "\ngquiz.size() : " << 
             gquiz.size();
    // used for telling the top element 
    // in priority queue
    cout << "\ngquiz.top() : " << 
             gquiz.top();

    // used for popping the element 
    // from a priority queue
    cout << "\ngquiz.pop() : ";
    gquiz.pop();
    showpq(gquiz);

    return 0;
}
```

Output:
```
The priority queue gquiz is :  30 20 10 5 1

gquiz.size() : 5
gquiz.top() : 30
gquiz.pop() :  20 10 5 1
```

Example 2:
```cpp
#include <bits/stdc++.h>
using namespace std;

// Print elements of priority queue
void print(priority_queue<int> pq)
{
    while (!pq.empty()) {
        cout << pq.top() << " ";
        pq.pop();
    }
    cout << endl;
}

int main()
{
    // priority_queue container declaration
    priority_queue<int> pq1;
    priority_queue<int> pq2;

    // pushing elements into the 1st priority queue
    pq1.push(1);
    pq1.push(2);
    pq1.push(3);
    pq1.push(4);

    // pushing elements into the 2nd priority queue
    pq2.push(3);
    pq2.push(5);
    pq2.push(7);
    pq2.push(9);

    cout << "Before swapping:-" << endl;
    cout << "Priority Queue 1 = ";
    print(pq1);
    cout << "Priority Queue 2 = ";
    print(pq2);

    // using swap() function to swap elements of priority
    // queues
    pq1.swap(pq2);

    cout << endl << "After swapping:-" << endl;
    cout << "Priority Queue 1 = ";
    print(pq1);
    cout << "Priority Queue 2 = ";
    print(pq2);
    return 0;
}
```

Output:
```
efore swapping:-
Priority Queue 1 = 4 3 2 1 
Priority Queue 2 = 9 7 5 3 

After swapping:-
Priority Queue 1 = 9 7 5 3 
Priority Queue 2 = 4 3 2 1 
```

Feel free to discover links in the references for more details.

## References:
- [Heaps and Priority Queues in C++](https://www.fluentcpp.com/2018/03/20/heaps-and-priority-queues-in-c-part-3-queues-and-priority-queues/)
- [Priority Queue in C++ Standard Template Library (STL)](https://www.geeksforgeeks.org/priority-queue-in-cpp-stl/)
- [std::priority_queue](https://en.cppreference.com/w/cpp/container/priority_queue)

