# Insertion Sort

The Insertion Sort algorithm is a sorting algorithm that can be applied on various data structures. The simplisity of the algorithm is exchanged for its relativly 'bad' time complexity (Insertion Sort is relativly inefficient in conjunction with large data sets). The working of the Insertion Sort algorithm can be best illustrated by an image:

<img src=Insertion-Sort.png width=80% height=80%>

## Characteristics

The Insertion Sort algorithm has a time complexity of O(N^2). This can be proven using above illustration. The illustration shows an array with elements (where N is the amount of elements within the array). Each element within the array requires comparison over the yet unsorted part of the array. This means the time complexity = N*(N-1)*1/2, which is 1/2(N^2-N). Let's break this down. The first 'N' represents each individual element, the 'N-1' represents the number of comparisons/swaps for each element ('-1' because we don't compare an element with itself) and the '1/2' represents the 'skip' of the already sorted part of the array. All the constants are disposed to determine the time complexity, which results in a time complexity of O(N^2) for the Insertion Sort algorithm.

The Insertion Sort algorithm is in best case scenario O(N), because 'early' termination is possible. This means in case of a sorted array as input, the algorithm will have a linear time complexity. This is an advantage in comparison to Selection Sort, which always takes O(N^2). The benefit of Insertion Sort over Bubble Sort is that Insertion sort has the advantage of early termination of the inner loop, while Bubble Sort only has this advantage within the outer loop.

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

    std::vector<int> InsertionSortAscending(std::vector<int>&& elements)
    {
        for (int i = 1; i < elements.size(); i++) 
        {
            int integerUnderAnalysis = elements.at(i);
            int emptyIndex = i;

            for (int j = i-1; j >= 0; j--) 
            {
                if (elements.at(j) > integerUnderAnalysis)
                {
                    elements.at(emptyIndex) = elements.at(j);
                    emptyIndex--;
                }
                else {
                    break;
                }
            }
            elements.at(emptyIndex) = integerUnderAnalysis;     
        }

        return elements;
    }

    int main()
    {
        Print(InsertionSortAscending({ 3,4,5,1,2,0 }));
    }

</code>
