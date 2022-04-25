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
      fill_n(table.begin(), Becket, -1);
  }
  int h(int x, int i){
      return (x%Bucket + i)%Bucket;
  }
  void insert(int key){
    int index = key%Bucket;
    if(table[index] && table[index] != -1){ // Not Deleted means element is present
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
  void search(int key){
      
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
    lp.insert(5);
    lp.insert(11);
    lp.insert(22);
    lp.printHashTable();
    return 0;
}


```

