# Unordered_map

## Definition
Unordered maps are associative containers that store elements formed by the combination of a key value and a mapped value, and which allows for fast retrieval of individual elements based on their keys. <br>

In an unordered_map, the key value is generally used to uniquely identify the element, while the mapped value is an object with the content associated to this key. Types of key and mapped value may differ.

## Usage
Use when:
- **You need to keep count of some data (Example - strings) and no ordering is required.**

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;
int main ()
{
  unordered_map<string,double> mymap = {
     {"Burger",2.99},
     {"Fries",1.99},
     {"Soda",1.50} };

  for (auto& x: {"Burger","Pizza","Salad","Soda"}) {
    if (mymap.count(x)>0)
      cout << "mymap has " << x << endl;
    else
      cout << "mymap has no " << x << endl;
  }

  return 0;
}

```

Output:
```{output}
mymap has Burger
mymap has no Pizza
mymap has no Salad
mymap has Soda
```

- **You need single element access (no traversal).**

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;
int main() {

  unordered_map<string, int> people = { {"John", 32}, {"Adele", 45}, {"Bo", 29} };

  cout << "Adele is: " << people.at("Adele") << "\n"; 

  cout << "Bo is: " << people.at("Bo") << "\n";

  return 0;
}
```

Output
```{output}
Adele is: 45
Bo is: 29
```

| Methods/Functions    | Description |
| -------- | ------- |
| at()  | This function in C++ unordered_map returns the reference to the value with the element as key k    |
| begin() | Returns an iterator pointing to the first element in the container in the unordered_map container     |
| end()    | Returns an iterator pointing to the position past the last element in the container in the unordered_map container    |
| bucket()    | Returns the bucket number where the element with the key k is located in the map    |
| bucket_count    | Bucket_count is used to count the total no. of buckets in the unordered_map. No parameter is required to pass into this function    |
| bucket_size    | Returns the number of elements in each bucket of the unordered_map    |
| count()    | Count the number of elements present in an unordered_map with a given key    |
| equal_range    | Return the bounds of a range that includes all the elements in the container with a key that compares equal to k    |
| find()    | Returns iterator to the element    |
| empty()    | Checks whether the container is empty in the unordered_map container    |
| erase()    | Erase elements in the container in the unordered_map container    |
| insert()    | Inserts elements in the container in the unordered_map container    |
| find()    |  Searches the container for an element with key and returns an iterator to it if found, otherwise it returns an iterator to unordered_map::end (the element past the end of the container).    |

```cpp

#include <iostream>
#include <unordered_map>
using namespace std;

int main()
{
  unordered_map<string, int> umap;

  umap["Contribute"] = 30;
  umap["GeeksforGeeks"] = 10;
  umap["Practice"] = 20;
  umap["Contribute"] = 60;
  umap["Contribute"] = 90;

  for (auto x : umap)
    cout << x.first << " " << 
            x.second << endl;
}

```

**Output**
```{output}
Practice 20
GeeksforGeeks 10
Contribute 90
```

## Comparison

| Unordered_map    | Map |
| -------- | ------- |
| The unordered_map key can be stored in any order  | The map is an ordered sequence of unique keys     |
| Unordered_Map implements an unbalanced tree structure due to which it is not possible to maintain order between the elements | Map implements a balanced tree structure which is why it is possible to maintain order between the elements (by specific tree traversal)     |
| The time complexity of unordered_map operations is O(1) on average    | The time complexity of map operations is O(log n)    |

## Reference

[std::unordered_map](https://cplusplus.com/reference/unordered_map/unordered_map/)<br>
[unordered_map in C++ STL](https://www.geeksforgeeks.org/unordered_map-in-cpp-stl/)<br>
[C++ Maps](https://www.w3schools.com/cpp/cpp_maps.asp)