# 365 Days Coding Challange

## Data Structures and Algorithms in C++ [Copyrighted to AlphaDecodeX]

### Day 1. Chain Implementation to Avoid Collison in Hashing (Open Hashing)

- Linked List Implementation is Used
- Self Balancing trees like (AVL and Red Black Trees) Could also be used.

```CPP

#include <bits/stdc++.h>
using namespace std;

// Implementing the Chaining .....
struct MyHash{
    int BUCKET;
    list<int> *table;
    MyHash(int b){
        BUCKET = b;
        table = new list<int>[b];
    }
    int h(int x){
        return x % BUCKET;
    }
    void insert(int key){
        int block_index = h(key);
        table[block_index].push_back(key);
    }
    void remove(int key){
        int block_index = h(key);
        table[block_index].remove(key);
    }
    bool search(int key){
        int block_index = h(key);
        for(auto x: table[block_index]){
            if(x==key){
                return true;
            }
        }
        return false;
    }
    void printHashTable(){
        
        for(int index = 0; index<BUCKET; index++){
            for(auto x: table[index]){
                cout<<x<<"-->";
            }
            cout<<endl;
        }
    }  
};

int main()
{
    MyHash hT(7);
    hT.insert(50);
    hT.insert(21);
    hT.insert(58);
    hT.insert(17);
    hT.insert(15);
    hT.insert(49);
    hT.insert(56);
    hT.printHashTable();
    hT.remove(17);
    hT.printHashTable();
    return 0;
}

```

#### Open Hashing or Separate Chaining is

- Not cache friendly
- Keys can go outside the Hash table
- Keys can be larger in number than the size of HashTable
- Deletion is Easier
- Some buckets would never be used and leads to the wastage of storage

### Day 2. Linear Probing Hashing (Closed Hashing or Open Addressing)

- Keys stored can be atmost the size of hashtable.
- Caching friendly, No keys stored outside the Table
- But It results to the Primary clustring 

```CPP

#include <bits/stdc++.h>
using namespace std;

struct LinearProbing{
  int Bucket;
  vector<int> table;
  
  LinearProbing(int size){
      Bucket = size;
      for(int index = 0; index<Bucket; index++){
          table.push_back(-1);
      }
  }
  int h(int x, int i){
      return (x%Bucket + i)%Bucket;
  }
  void insert(int key){
    int index = key%Bucket;
    if(table[index] && table[index] != -1){ // Not Deleted/Empty means element is present
        int i = 1;
        while(true){
            int temp_index = h(key, i);
            if(index == temp_index){
                return;
            }else{
                if(table[temp_index] == -1 || !table[temp_index]){
                    table[temp_index] = key;
                    return;
                }
            }
            i+=1; // Collison Factor
        }
    }else if(!table[index] || table[index] == -1){
        table[index] = key;
        return;
    }
  }
  int search(int key){
      if(table[key%Bucket] == key){
          return key%Bucket;
      }
      int i = 1;
      int start = key%Bucket;
      while(true){
          int temp_index = h(key, i);
          if(table[temp_index] != key && temp_index == start){
              return -1;
          }else if(table[temp_index] == key){
              return temp_index;
          }else if(table[temp_index]!=key && temp_index != start){
              i++;
          }
      }
  }
  void deleteItem(int key){
      int found_index = search(key);
      if(found_index == -1){
          return;
      }
      table[found_index] = -1;
      return;
  }
  void printHashTable(){
    for(auto x: table){
        cout<<x<<" ";
    }  
  }
  
};

int main()
{
    
    LinearProbing lp(5);
    lp.insert(11);
    lp.insert(22);
    lp.insert(32);
    lp.insert(33);
    lp.insert(36);
    lp.printHashTable();
    cout<<endl;
    cout<<lp.search(33)<<endl;
    cout<<lp.search(100)<<endl;
    lp.deleteItem(11);
    lp.printHashTable();
    return 0;
}
```
### Day 3. Quadratic Probing Hashing (Closed Hashing or Open Addressing)

- Clustering Problem is still there
- But Secondary Clusters which are better than Primary Clusters

```CPP
/******************************************************************************

Welcome to GDB Online.
GDB online is an online compiler and debugger tool for C, C++, Python, Java, PHP, Ruby, Perl,
C#, VB, Swift, Pascal, Fortran, Haskell, Objective-C, Assembly, HTML, CSS, JS, SQLite, Prolog.
Code, Compile, Run and Debug online from anywhere in world.

*******************************************************************************/
#include <bits/stdc++.h>

using namespace std;

struct QuadProbing{
    int bucket;
    vector<int> table;
    
    QuadProbing(int size){
        this->bucket = size;
        for(int index = 0; index<size;index++){
            table.push_back(-1);
        }
    }
    int h(int x, int i){
        return (x%bucket + i*i)%bucket;
    }
    void insert(int item){
        int index = item%bucket;
        if(table[index] && table[index]!=-1){ // Element is present and not deleted or empty .....
            int i = 1;
            while(true){
                int temp_index = h(item, i);
                if(temp_index == index){
                    return;
                }else if(table[temp_index] == -1){
                    table[temp_index] = item;
                }
                i++; // Collison Factor
            }
        }else{
            table[index] = item;
        }
    }int search(int key){
      if(table[key%bucket] == key){
          return key%bucket;
      }
      int i = 1;
      int start = key%bucket;
      while(true){
          int temp_index = h(key, i);
          if(table[temp_index] != key && temp_index == start){
              return -1;
          }else if(table[temp_index] == key){
              return temp_index;
          }else if(table[temp_index]!=key && temp_index != start){
              i++;
          }
      }
  }
  void deleteItem(int key){
      int found_index = search(key);
      if(found_index == -1){
          return;
      }
      table[found_index] = -1;
      return;
  }
  void printHashTable(){
    for(auto x: table){
        cout<<x<<" ";
    }  
  }
  
};

int main()
{
    
    QuadProbing qp(5);
    qp.insert(11);
    qp.insert(22);
    qp.insert(32);
    qp.insert(33);
    qp.insert(36);
    qp.printHashTable();
    cout<<endl;
    cout<<qp.search(33)<<endl;
    cout<<qp.search(100)<<endl;
    qp.deleteItem(11);
    qp.printHashTable();
    return 0;
}
```
### Day 3. Double Hashing

- It involves less clustering 
- Contains two hash functions Computations with collison factor as in Linear and Quad. Probing
- Only hash_function(x, i) needs to be changed as:- 
- hash(x,i) = (h1(key) + i * h2(key)) % m
