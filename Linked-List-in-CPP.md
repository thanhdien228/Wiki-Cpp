## What is Linked List in C++
In C++, a linked list is a linear data structure that allows the users to store data in non-contiguous memory locations.

 A linked list is defined as a collection of nodes where each node consists of two members which represents its value and a next pointer which stores the address for the next node.
 
## Types of Linked Lists
### 1. Singly Linked List
The singly linked list is the simplest form of linked list in which the node contain two members data and a next pointer that stores the address of the next node. Each node is a singly linked list is connected through the next pointer and the next pointer of the last node points to NULL denoting the end of the linked list. The following diagram describes the structure of a singly linked list:

<p align="center">
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20240607183512/Sinlgy-Linked-List.png" alt="Singly Linked List" />
</p>

**Example**

```cpp
// Definition of a Node in a singly linked list
struct Node {
  
    // Data part of the node
    int data;

    // Pointer to the next node in the list
    Node* next;

    // Constructor to initialize the node with data
    Node(int data)
    {
        this->data = data;
        this->next = nullptr;
    }
};
```

### 2. Doubly Linked List

The doubly linked list is the modified version of the singly linked list where each node of the doubly linked consists of three data members data, next and prev. The prev is a pointer that stores the address of the previous node in the linked list sequence. Each node in a doubly linked list except the first and the last node is connected with each other through the prev and next pointer. The prev pointer of the first node and the next pointer of the last node points to NULL in the doubly linked list. The following diagram describes the structure of a doubly linked list:

<p align="center">
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20240607183511/Doubly-Linked-List.png" alt="Doubly Linked List" />
</p>

**Example**
```cpp
struct Node {

    // To store the Value or data.
    int data;

    // Pointer to point the Previous Element
    Node* prev;

    // Pointer to point the Next Element
    Node* next;
  
    // Constructor
    Node(int d) {
       data = d;
       prev = next = nullptr;      
    }
};
```
### 3. Circular Linked List

A circular linked list is a special type of linked list where all the nodes are connected to form a circle. Unlike a regular linked list, which ends with a node pointing to NULL, the last node in a circular linked list points back to the first node. This means that you can keep traversing the list without ever reaching a NULL value.

- **Circular Singly Linked List**

In Circular Singly Linked List, each node has just one pointer called the `next` pointer. The next pointer of last node points back to the first node and this results in forming a circle. In this type of Linked list we can only move through the list in one direction.

<p align="center">
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20240607183510/Circular-Linked-List.png" alt="Circular Singly Linked List" />
</p>

- **Circular Doubly Linked List**

In circular doubly linked list, each node has two pointers prev and next, similar to doubly linked list. The prev pointer points to the previous node and the next points to the next node. Here, in addition to the last node storing the address of the first node, the first node will also store the address of the last node.

<p align="center">
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20240607183510/DoublyCircular--Linked-List.png" alt="Doubly Linked List" />
</p>

**Eaxmple**

Insertion in the circular linked list

```cpp
#include <iostream>
using namespace std;

struct Node{
    int data;
    Node *next;
    Node(int value){
        data = value;
        next = nullptr;
    }
};

// Function to insert a node into an empty circular singly linked list
Node *insertInEmptyList(Node *last, int data){
    if (last != nullptr) return last;
    
    // Create a new node
    Node *newNode = new Node(data);
  
    // Point newNode to itself
    newNode->next = newNode;
  
    // Update last to point to the new node
    last = newNode;
    return last;
}

void printList(Node* last){
    if(last == NULL) return;
  
    // Start from the head node
    Node* head = last->next;
    while (true) {
        cout << head->data << " ";
        head = head->next;
        if (head == last->next) break;
    }
    cout << endl;
}

int main(){
    Node *last = nullptr;

    // Insert a node into the empty list
    last = insertInEmptyList(last, 1);

    // Print the list
    cout << "List after insertion: ";
    printList(last);

    return 0;
}
```
**Output**
```bash
List after insertion: 1 
```

## List in C++ Standard Template Library (STL)
Lists are sequence containers that allow non-contiguous memory allocation. As compared to the vector, the list has slow traversal, but once a position has been found, insertion and deletion are quick (constant time). Normally, when we say a List, we talk about a **doubly linked list**. For implementing a singly linked list, we use a **forward_list**.

**Syntax:**

```bash
std::list <data-type> name_of_list;
```

### Some Basic Operations on std::list

* `front()` - Returns the value of the first element in the list.
* `back()` - Returns the value of the last element in the list.
* `push_front()` - Adds a new element at the beginning of the list.
* `push_back()` - Adds a new element at the end of the list.
* `pop_front()` - Removes the first element of the list, and reduces the size of the list by 1.
* `pop_back()` - Removes the last element of the list, and reduces the size of the list by 1.
* `insert()` - Inserts new elements in the list before the element at a specified position.
* `size()` - Returns the number of elements in the list.
* `begin()` - begin() function returns an iterator pointing to the first element of the list.
* `end()` - end() function returns an iterator pointing to the theoretical last element which follows the last element.

**Example**
```cpp
// C++ program to demonstrate the implementation of List
#include <iostream>
#include <iterator>
#include <list>
using namespace std;

// function for printing the elements in a list
void showlist(list<int> g)
{
    list<int>::iterator it;
    for (it = g.begin(); it != g.end(); ++it)
        cout << '\t' << *it;
    cout << '\n';
}

int main()
{

    list<int> gqlist1, gqlist2;

    for (int i = 0; i < 10; ++i) {
        gqlist1.push_back(i * 2);
        gqlist2.push_front(i * 3);
    }
    cout << "\nList 1 (gqlist1) is : ";
    showlist(gqlist1);

    cout << "\nList 2 (gqlist2) is : ";
    showlist(gqlist2);

    cout << "\ngqlist1.front() : " << gqlist1.front();
    cout << "\ngqlist1.back() : " << gqlist1.back();

    cout << "\ngqlist1.pop_front() : ";
    gqlist1.pop_front();
    showlist(gqlist1);

    cout << "\ngqlist2.pop_back() : ";
    gqlist2.pop_back();
    showlist(gqlist2);

    cout << "\ngqlist1.reverse() : ";
    gqlist1.reverse();
    showlist(gqlist1);

    cout << "\ngqlist2.sort(): ";
    gqlist2.sort();
    showlist(gqlist2);

    return 0;
}

```
**Output**
```{bash}
List 1 (gqlist1) is :     0    2    4    6    8    10    12    14    16    18

List 2 (gqlist2) is :     27    24    21    18    15    12    9    6    3    0

gqlist1.front() : 0
gqlist1.back() : 18
gqlist1.pop_front() :     2    4    6    8    10    12    14    16    18

gqlist2.pop_back() :     27    24    21    18    15    12    9    6    3

gqlist1.reverse() :     18    16    14    12    10    8    6    4    2

gqlist2.sort():     3    6    9    12    15    18    21    24    27
```

### Points to Remember about List Container 

* It is generally implemented using a dynamic doubly linked list with traversal in both directions.
* Faster insert and delete operation as compared to arrays and vectors.
* It provides only sequential access. Random Access to any middle element is not possible
* It is defined as a template so it is able to hold any data type.
* It operates as an unsorted list would, which implies that by default, the list's order is not preserved. However, there are techniques for sorting.

## Reference
[Linked List in C++](https://www.geeksforgeeks.org/cpp-linked-list/)<br>
[List in C++ Standard Template Library (STL)](https://www.geeksforgeeks.org/list-cpp-stl/)