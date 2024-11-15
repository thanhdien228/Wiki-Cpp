## Stack, Queue, Deque
### Stack
#### what is stack?
Stacks are a type of container adaptors with LIFO(Last In First Out) type of working, where a new element is added at one end (top) and an element is removed from that end only.
<p align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/20230726165552/Stack-Data-Structure.png" />
</p>
#### Stack syntax
The functions associated with stack are: 

- **empty()** Returns whether the stack is empty - Time Complexity : O(1) 
 
- **size()** Returns the size of the stack - Time Complexity : O(1) 
 
- **top()** Returns a reference to the top most element of the stack - Time Complexity : O(1) 
 
- **push(g)** Adds the element 'g' at the top of the stack - Time Complexity : O(1) 

- **pop()** Deletes the most recent entered element of the stack - T
ime Complexity : O(1) 

```cpp
#include <iostream> 
#include <stack>
using namespace std;
int main() {
    stack<int> stack;
    stack.push(21);
    stack.push(22);
    stack.push(24);
    stack.push(25);
    int num=0;
      stack.push(num);
    stack.pop();
    stack.pop();
      stack.pop();
  
    while (!stack.empty()) {
        cout << stack.top() <<" ";
        stack.pop();
    }
}
```
Output:
```cpp
22 21 
```
## Queue
### what is queue?
Queues are a type of container adaptors that operate in a first in first out (FIFO) type of arrangement. Elements are inserted at the back (end) and are deleted from the front.
<p align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/20230726165642/Queue-Data-structure1.png" />
</p>
#### Stack syntax
The functions associated with queue are: 

- **empty()** Returns whether the queue is empty - Time Complexity : O(1) 
 
- **size()** Returns the size of the queue - Time Complexity : O(1) 
 
- **push(g)** Adds the element 'g' at the top of the queue - Time Complexity : O(1) 

- **pop()** Deletes the most recent entered element of the queue - Time Complexity : O(1) 
- **swap()** Exchange the contents of two queues but the queues must be of the same data type, although sizes may differ. - Time Complexity : O(1) 
- **emplace()** Insert a new element into the queue container, the new element is added to the end of the queue. - Time Complexity : O(1) 
- **front()** Returns a reference to the first element of the queue. - Time Complexity : O(1) 
- **back()** Returns a reference to the last element of the queue. - Time Complexity : O(1) 
```cpp
#include <iostream>
#include <queue>
using namespace std;

// function prototype for display_queue utility
void display_queue(queue<string> q);

int main() {

  // create a queue of string
  queue<string> animals;

  // push element into the queue
  animals.push("Cat");
  animals.push("Dog");
  animals.push("Fox");
  
  cout << "Initial Queue: ";
  display_queue(animals);
  
  // remove element from queue
  animals.pop();
  
  cout << "Final Queue: ";
  display_queue(animals);
  
  return 0;
}

// utility function to display queue
void display_queue(queue<string> q) {
  while(!q.empty()) {
    cout << q.front() << ", ";
    q.pop();
  }

  cout << endl;
};
}
```
Output:
```cpp
Initial Queue: Cat, Dog, Fox, 
Final Queue: Dog, Fox, 
```
## Deque
### what is deque? 
Double-ended queues are sequence containers with the feature of expansion and contraction on both ends. They are similar to vectors, but are more efficient in case of insertion and deletion of elements.
<p align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/uploads/anod.png" />
</p>
#### Stack syntax
- **empty()** Returns whether the stack is empty - Time Complexity : O(1) 
 
- **size()** Returns the size of the stack - Time Complexity : O(1) 
 
- **push_front(g)** Adds the element 'g' at the front of the queue - Time Complexity : O(1)

- **push_back(g)** Adds the element 'g' at the back of the queue - Time Complexity : O(1) 

- **pop()_front** Deletes the front element of the queue - Time Complexity : O(1) 

- **pop()_back** Deletes the back element of the queue - Time Complexity : O(1) 

- **swap()** Exchange the contents of two queues but the queues must be of the same data type, although sizes may differ. - Time Complexity : O(1) 

- **emplace()_front** Insert a new element into the queue container, the new element is added to the front of the queue. - Time Complexity : O(1) 
- **emplace()_back** Insert a new element into the queue container, the new element is added to the back of the queue. - Time Complexity : O(1) 
- **front()** Returns a reference to the first element of the queue. - Time Complexity : O(1) 
- **back()** Returns a reference to the last element of the queue. - Time Complexity : O(1) 
```cpp
#include <iostream>
#include <deque>
using namespace std;

int main() {

  deque<int> nums {2, 3};

  cout << "Initial Deque: ";
  for (const int& num : nums) {
    cout << num << ", ";
  }
  
  // add integer 4 at the back
  nums.push_back(4);
   
  // add integer 1 at the front
  nums.push_front(1);
  
  cout << "\nFinal Deque: ";
  for (const int& num : nums) {
    cout << num << ", ";
  }
  
  return 0;
}
```

```cpp
Initial Deque: 2, 3, 
Final Deque: 1, 2, 3, 4,
```
## Reference
[Stack GeeksforGeeks](https://www.geeksforgeeks.org/stack-data-structure/)

[Queue GeeksforGeeks](https://www.geeksforgeeks.org/queue-cpp-stl/)

[Deque GeeksforGeeks](https://www.geeksforgeeks.org/deque-cpp-stl/)