# Selection Sort

The selection Sort algorithm is one of the simplest sorting algorithms that can be applied on various data structures. The simplisity of the algorithm is exchanged for its relativly 'bad' time complexity (Selection Sort is relativly inefficient in conjunction with large data sets). The working of the Selection Sort algorithm can be best illustrated by an image:

<img src=Selection-Sort.png width=80% height=80%>

## Characteristics

The Selection Sort algorithm has a time complexity of O(N^2). This can be proven using above illustration. The illustration shows an array with elements (where N is the amount of elements within the array). Each element within the array requires comparison over the yet unsorted part of the array. This means the time complexity = N*(N-1)*1/2, which is 1/2(N^2-N). Let's break this down. The first 'N' represents each individual element, the 'N-1' represents the number of comparisons for each element ('-1' because we don't compare first element with itself) and the '1/2' represents the 'skip' of the already sorted part of the array. All the constants are disposed to determine the time complexity, which results in a time complexity of O(N^2) for the Selection Sort algorithm.

The Selection Sort algorithm is in best case scenario O(N^2), because no 'early' termination is possible. This means in case of a sorted array as input, the algorithm is still O(N^2) complexity. However, the Selection Sort does not perform a swap instruction after each comparison. This contributes to its slightly enhanced efficiency in comparison to Bubble Sort.

The Space Complexity is O(1).

## Applicability

Standard language applications:
- n/a

## Implementation
<code>

    void Print(const std::vector<int>& elements)
    {
        for (auto element : elements) {
            std::cout << element << " ";
        }
        std::cout << "\n" << "------------" << "\n";
    }

    std::vector<int> SelectionSortAscending(std::vector<int>&& elements) 
    {
        for (auto i = elements.begin(); i != elements.end(); i++) 
        {
            std::vector<int>::iterator lowest = i;
            for (auto j = (i+1); j != elements.end(); j++) 
            {    
                if (*i > *j) 
                {
                    lowest = j;
                }
            }
            int temp = *i;
            *i = *lowest;
            *lowest = temp;
        }
        return elements;
    }

    int main()
    {
        Print(SelectionSortAscending({ 4,3,2,1,0,6 }));
    }

</code>
