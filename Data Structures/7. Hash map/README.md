# Hash map

A Hash map is datastructure that uses a hashing function to store Key-Value pairs. A hash map has the following key components:
- A Key-Value pair: a Key is the element that is used to access the belonging Value. E.g.: a key could be a String: "MyFirstKey" and it's belong value could be an Integer: 9. This means 'prompting' the map with "MyFirstKey" will give the value 9.
- A Hash Table: a data stucture, often in the form of a Dynamic Array, to store Key-Value pairs
- A Bucket: a data structure, often in the form of a Linked List or Tree, to hold multiple Key-Value pairs that Hash to the same index of the Hash Table. E.g.: a Key: 7 (with a value) and a Key: 14 (with another value) could hash to the same index of the Hash Table, which causes 'collision'. Another data structure is introduced to be able to store two or more values on the same index of the Hash Table.
- A Hash function: is a function that translates the given Key into an index of the Hash Table. A 'good' Hash function ensures that the number of key-value pairs are evenly distributed amongst the Buckets. E.g. if we have 5 buckets, then it is undiserable to have 5 Key-Value pairs stored in Bucket 1. This is because a Bucket is often in the form of a Linked List or Tree, which has to be traversed to find the Value belonging to the Key. On the other hand, if we have 5 buckets and each bucket contains 1 Key-Value pair, that would mean we access the Value belonging to a respective Key in constant time.
- Load Factor: is the total number of Key-Value pairs devided by the number of Buckets, which will cause the Hash Table to resize. A well defined hash table will distribute Key-value pair evenly accros the buckets, but the problems of having two many Key-Value pairs per bucket can still arise. To identify this problem a Load Factor is being calculated: Key-Value pairs/number of Buckets. This will give the bucket occupation percentage. Often a Load factor of 75% is used, reaching or exceeding this percentage will result in a resize. A resize means, the Hash Table will have more Buckets and all the Key-Value pairs will have to be re-inserted. A load factor of 0.75 is a balance between look-up time and space. 0.75 means we keep the chance of having a collision low (with the assumption we have a 'good' hash function) and not having to much unused memory. This can be proven by using the Poisson distribution.

<img src=Hash_map_components.png width=70% height=70%>

## Characteristics

- Insertion: 
    - O(1), when there is no collision and no resizing
    - O(N), 
         - when all Key-Value pairs are stored in the same Bucket, i.e. there is a collision. O(N) time complexity stems from traversing the Bucket for a duplicate.
         - when resizing is required. O(N) time complexity stems from re-hashing all the Key-Value pairs.
- Search:
    - O(1), when there are no colliding Key-Value pairs in a Bucket
    - O(N), when all Key-Value pairs are stored in the same Bucket, i.e. there are colliding Key-Value pairs. O(N) time complexity stems from traversing the Bucket.
- Deletion:
    - O(1), when there are no colliding Key-Value pairs in a Bucket
    - O(N), when all Key-Value pairs are stored in the same Bucket, i.e. there are colliding Key-Value pairs. O(N) time complexity stems from traversing the Bucket. 
        - Note: resizing is uncommon for hash maps

The space complexity is defined:
- Insertion, deletion, search: O(1), the space complexity is constant for each individual operation. Insertion might temporarly have a space complexity of O(N) due to resizing.


## Applicability


## Implementation