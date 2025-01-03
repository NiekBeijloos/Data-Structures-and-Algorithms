# Hash map

## Components

A Hash map is datastructure that uses a hashing function to get and set Key-Value pairs. A hash map has the following key components:
- **A Key-Value pair**: a Key is the element that is used to access the belonging Value. E.g.: a key could be a String: "MyFirstKey" and it's belong value could be an Integer: 9. This means 'prompting' the map with "MyFirstKey" will give the value 9.
- **A Hash Table**: a data stucture, often in the form of a Dynamic Array, to store Key-Value pairs
- **A Bucket**: a data structure, often in the form of a Linked List or Tree, to hold multiple Key-Value pairs that Hash to the same index of the Hash Table. E.g.: a Key: 7 (with a value) and a Key: 14 (with another value) could hash to the same index of the Hash Table, which causes 'collision'. Another data structure is introduced to be able to store two or more values on the same index of the Hash Table.
- **A Hash function**: is a function that translates the given Key into an index of the Hash Table. A 'good' Hash function ensures that the number of key-value pairs are evenly distributed amongst the Buckets. E.g. if we have 5 buckets, then it is undiserable to have 5 Key-Value pairs stored in Bucket 1. This is because a Bucket is often in the form of a Linked List or Tree, which has to be traversed to find the Value belonging to the Key. On the other hand, if we have 5 buckets and each bucket contains 1 Key-Value pair, that would mean we access the Value belonging to a respective Key in constant time.
- **Load Factor**: is the total number of Key-Value pairs devided by the number of Buckets, which will cause the Hash Table to resize. A well defined hash table will distribute Key-value pair evenly accros the buckets, but the problems of having two many Key-Value pairs per bucket can still arise. To identify this problem a Load Factor is being calculated: Key-Value pairs/number of Buckets. This will give the bucket occupation percentage. Often a Load factor of 75% is used, reaching or exceeding this percentage will result in a resize. A resize means, the Hash Table will have more Buckets and all the Key-Value pairs will have to be re-inserted. A load factor of 0.75 is a balance between look-up time and space. 0.75 means we keep the chance of having a collision low (with the assumption we have a 'good' hash function) and not having to much unused memory. This can be proven by using the Poisson distribution.

<img src=Hash_map_components.png width=70% height=70%>

## Hash function in depth

A 'good' Hash function, means a hash function that ensures that key-value pairs are evenly distributed amongst the Buckets. To guarantee this property the hash function makes use of the following traits:
- Prime numbers; prime numbers have the characteristic of decreasing the likelyhood that a specific numerical pattern will be hit
- Modulo; modulo ensures that we keep the hashing output in range with the number of Buckets in the Hash Table

### Prime numbers and hashing

Let's see why prime numbers work so 'well', by keeping the following hash function: **hash_map_index = integer_key $mod$ number_of_buckets** in mind: 
- A none prime number: 9:
    - is divisible by: {1, 3, 9}
- A prime number: 7: 
    - is divisible by: {1,7}

The number of divisors impacts the 'clustering' chance of Key-Value pairs in Buckets. In case of the none-prime number: 9, there is an additional divisor. This means not only multiples of 9 (and 1, but all numbers are multiple of 1) might result in clustering, but also multiples of 3.

To ensure that the number of buckets remain a prime number, to overcome 'clustering' of Key-Value pairs in buckets, we have to calculate the next prime number when we resize the hash map. To calculate the next prime, consider the following:
1. Characteristics of a prime and none prime number are : 
    - Prime number: divisible by itself and 1
    - None prime number: divisible by itself, 1 and another integer
2. Check on the uncommon trait between prime and none prime numbers: 'divisible by another integer', as this will make it easier to separate prime from none prime numbers, i.e. :
    - value 'k' is not a prime, in: k $mod$ i, where i is a number other than itself or 1, will result in '0'.
3. Find the upper and lower bound for 'i' in k $mod$ i. Let's derive these bounds by means of an example:
    -  For k=36, the number of pairs we can make to get to 36 is: {1,36}, {2,18}, {3,12}, {4,9}, {6,6}. i.e. multiplying these pairs with each other results in 36. 
    - As stated before, the goal to determine if 'k' is a prime we need to know if it is divisble by another number than itself or 1. Inserting the pairs above for i will result in '0', e.g.: 36 $mod$ 2 or 36 $mod$ 18, both result in '0', which means 36 is not a prime.
    - A characteristic of these pairs is that one number proofs the other. E.g. pair {2,18}, means 36 $mod$ 2 proofs 36 $mod$ 18. Due to this characteristic the following bounds can be set for 'i' in k $mod$ i, to proof k is a prime:
        - lower bound for i equals 2, as for prime and none prime numbers 1 will always result in '0'
        - upper bound for i equals to $\sqrt{k}$, because {6,6} is the last pair of the sequence. i = 6 and k = 36, their relationship can be described as i = $\sqrt{k}$, because $\sqrt{36}$ = 6
4. In mathematical terms: if k $mod$ i $\neq$ 0  $\forall$ i  $\in$ $[2,\lfloor \sqrt{k} \rfloor]$, then k is prime

### Hash function for Integer Keys
An example of a hash function, called 'Hashing by division', used for a Keys of type Integer:
- H(s) = $k$ $mod$ $b$, where:
    - $b$ is the number of buckets
    - $k$ is the key of type Integer
    - H is the generated hash value


### Hash function for String Keys
An example of a hash function, called 'Polynomial Rolling', for a Key of type String:

- H(s) = $(\sum_{i=0}^{n-1} s[i] *base^i)$ $mod$ $p$

where:
- H, is the hash value, representing the hash index in the hash map
- n, is the length of Key of type String
- i, is the index of a character of the Key
- s, is the key of type String
- base, a prime number to add weight to each character of the Key
- p, a prime number to scale the hash

How it works:
- The polynomial rolling hash function takes the integer representation of the respective character $s[i]$
- Multiplied by the $base^i$ to add weight to the respective character in a distributed way:
    - to use '31' for the $base$ is conventional (however not a must), the reason for it to be 31 is:
        - it is a prime number, this ensure more randomness, which prevents clustering
        - it's close to a power of 2, that means that multiplication can be accomplished with a bit shift and a substraction, i.e. computational efficiency. However, in modern cpus this becomes less relevant.
        - it's close to the English alphabet size (26)
    - the power of the base ensures Strings like: "abc" and "cba" will not add up to the same hash.
- Modulus $p$, where $p$ is a prime number representing the number of buckets in the hash table, the reason for that is:
    - a prime number, ensures more randomness, which prevents clustering
    - to normalize the hash to a specific range, to overcome overflowing a respective size

## Load factor in depth

The reason why 0.75 is consider an 'ideal' load factor can be proven by using the Poisson distribution.

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

Standard language applications:

- std::unordered_map in C++ use a Hash map

## Implementation

<code>

    typedef enum
    {
        string,
        integer,
    } type;

    typedef struct {
        const void* key;
        const void* value;
        void* next_map_pair;
        void* previous_map_pair;
    } map_pair;

    typedef struct{
        map_pair** items;
        int number_of_buckets;
        int number_of_items;
        int number_of_occupied_buckets;
        type key_type;
    } map;

    int hash_int_key(const int* key, int number_of_buckets)
    {
        if (number_of_buckets == 0) 
        {
            return -1;
        }

        return *key % number_of_buckets;
    }

    int hash_string_key(const char* s, int64_t number_of_buckets)
    {
        if (number_of_buckets <= 0)
        {
            return -1;
        }

        int64_t h = 0;
        const int64_t base = 31;
        const int64_t p = 1000000007;

        for (int i = 0; i < strlen(s); i++)
        {
            h = (h * base + s[i]) % p;
        }

        return h % number_of_buckets;
    }

    static float load_factor = 0.75f;

    map* map_ctr(type type)
    {
        map* m = malloc(sizeof(map));
        m->items = NULL;
        m->number_of_buckets = 0;
        m->number_of_items = 0;
        m->number_of_occupied_buckets = 0;
        m->key_type = type;

        return m;
    }

    void map_dtr(map* m)
    {
        for (int i = 0; i < m->number_of_buckets; i++)
        {
            map_pair* it = m->items[i];
            while(it != NULL)
            {
            map_pair* next = it->next_map_pair;
            free(it);
            it = next;
            }
        }
        free(m);
    }

    static int hash(map* map, const void* key)
    {
        int hashed_index = -1;
        switch (map->key_type)
        {
        case integer:
            hashed_index = hash_int_key(key, map->number_of_buckets);
            break;
        case string:
            hashed_index = hash_string_key(key, map->number_of_buckets);
            break;
        }

        return hashed_index;
    }

    static bool resize_is_required(map* map)
    {
        if(map->number_of_buckets == 0)
        {
            return true;
        }

        return ((float)map->number_of_items / (float)map->number_of_buckets) >= load_factor;
    }

    static bool is_prime(int number)
    {
        if (number <= 1)
        {
            return false;
        }

        for (int i = 2; i <= (int)(floor(sqrt(number))); i++)
        {
            if (number % i == 0)
            {
                return false;
            }
        }

        return true;
    }

    static int generate_next_prime(int current_prime)
    {
        int new_prime = -1;

        for (int i = current_prime + 1; i < INT_MAX; i++)
        {
            if (is_prime(i))
            {
                new_prime = i;
                break;
            }
        }

        return new_prime;
    }

    static void resize(map* map)
    {
        const int new_size = generate_next_prime(map->number_of_buckets *2);
        map_pair** new_pairs = calloc(new_size, sizeof(map_pair*));

        for (int i = 0; i < map->number_of_occupied_buckets; i++)
        {
            const int new_index = hash(map, map->items[i]->key); // rehash
            new_pairs[new_index] = map->items[i];
        }
        free(map->items);
        map->number_of_buckets = new_size;
        map->items = new_pairs;
    }

    bool try_find(map* map, const void* key, map_pair** out)
    {
        const int index = hash(map, key);
        if(index < 0)
        {
            return false;
        }
        bool key_exists_in_map = false;
        map_pair* it = map->items[index];
        while (it != NULL)
        {
            switch (map->key_type)
            {
                case integer:
                {
                    const int* key_in_map = it->key;
                    if (*key_in_map == *(const int*)(key))
                    {
                        *out = it;
                        key_exists_in_map = true;
                    }
                }
                break;
                case string:
                {
                    if (strcmp(it->key, key) == 0)
                    {
                        *out = it;
                        key_exists_in_map = true;
                    }
                }
                break;
            }
            if(key_exists_in_map)
            {
                break;
            }
            it = map->items[index]->next_map_pair;
        }
        return key_exists_in_map;
    }

    static bool insert_item(map* map, map_pair* pair)
    {
        map_pair* out = NULL;
        if(try_find(map, pair->key, &out))
        {
            return false; // key already exists
        }

        const int index = hash(map, pair->key);
        bool result = true;

        if(index < 0)
        {
            result = false;
        }
        else if(map->items[index] != NULL)
        {
            map_pair* old_head = map->items[index];
            map_pair* new_head = pair;

            new_head->next_map_pair = old_head;
            new_head->previous_map_pair = NULL;
            old_head->previous_map_pair = new_head;
            old_head->next_map_pair = NULL;

            map->items[index] = new_head;
            map->number_of_items++;
        }
        else
        {
            map->items[index] = pair;
            map->number_of_occupied_buckets++;
            map->number_of_items++;
        }

        return result;
    }

    static map_pair* create_pair(const void* key, const void* value)
    {
        map_pair* pair = calloc(1, sizeof(map_pair));
        pair->key = key;
        pair->value = value;

        return pair;
    }

    bool try_add(map* map, const void* key, const void* value)
    {
        map_pair* pair = create_pair(key, value);

        if (resize_is_required(map))
        {
            resize(map);
        }

        return insert_item(map, pair);
    }

    static void delete_item(map* map, map_pair* pair)
    {
        map_pair* previous_pair = pair->previous_map_pair;
        map_pair* next_pair = pair->next_map_pair;
        const void* key = pair->key;

        if (previous_pair != NULL)
        {
            previous_pair->next_map_pair = next_pair;
        }

        free(pair);

        if (previous_pair == NULL && next_pair == NULL)
        {
            map->items[hash(map, key)] = NULL;
        }
    }

    bool try_remove(map* map, const void* key)
    {
        map_pair* pair;
        if(!try_find(map,key, &pair))
        {
            return false;
        }

        delete_item(map, pair);

        return true;
    }

    bool is_empty(map* map)
    {
        for(int i = 0; i < map->number_of_buckets; i++)
        {
            if(map->items[i] != NULL)
            {
                return false;
            }
        }
        return true;
    }

<code>