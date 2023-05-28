# Binary search

Binary search is a search algorithm that is applicable on sorted data structures. Binary search has relativily quick search in exchange for slow element insertion/deletion (because the data has to remain sorted).   

## Characteristics

The time complexity of Binary search is O(log(N)). Each time that N doubles, the amount of steps increase by 1. E.g. N = 2 requires 1 step, N = 4 requires 2 steps, etc. The space complexity is O(N).

## Applicability

Standard language applications:
- std::binary_search in C++

## Implementation
<code>

    struct Item {
        int data = 0;
    };

    class BinarySearch {
    public:
        static Item* Search(std::vector<Item*> sortedList, int data) {
            return SearchRecursive(sortedList, data, 0, sortedList.size() - 1);
        }

    private:
        static Item* SearchRecursive(std::vector<Item*> sortedList, int data, int start, int end)
        {
            if (start > end) {
                return nullptr;
            }

            int mid = start + ((end - start) / 2.0);
            
            if (data < sortedList[mid]->data)
            {
                return SearchRecursive(sortedList, data, start, mid-1);
            }
            else if (data > sortedList[mid]->data)
            {
                return SearchRecursive(sortedList, data, mid+1, end);
            }
            else
            {
                return sortedList[mid];
            }
        }
    };

    int main()
    {
        std::cout << (BinarySearch::Search({new Item{1},new Item{2},new Item{3},new Item{4},new Item{5}}, 1))->data;
    }
</code>
