# Bubble Sort

The Bubble Sort algorithm is one of the simplest sorting algorithms that can be applied on various data structures. The simplisity of the algorithm is exchanged for its relativly 'bad' time complexity (Bubble Sort is relativly inefficient in conjunction with large data sets). The working of the Bubble Sort algorithm can be best illustrated by an image:

<img src=Bubble-Sort.png width=60% height=60%>

## Characteristics

The Bubble Sort algorithm has a time complexity of O(N^2). This can be proven using above illustration. The illustration shows an array with elements (where N is the amount of elements within the array). Each element within the array requires comparison over the yet unsorted part of the array. This means the time complexity = N*(N-1)*1/2, which is 1/2(N^2-N). Let's break this down. The first 'N' represents each individual element, the 'N-1' represents the number of comparisons for each element ('-1' because we don't compare last element with an 'out-of-bound' value) and the '1/2' represents the 'skip' of the already sorted part of the array. All the constants are disposed to determine the time complexity, which results in a time complexity of O(N^2) for the Bubble Sort algorithm.

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

    std::vector<int> BubbleSortDescending(std::vector<int>&& elements)
    {
        for (auto i = elements.begin(); i != elements.end(); i++) {
            bool sortOccurred = false;
            for (auto j = elements.begin(); j != (elements.end()-1- (i-elements.begin())); j++) {
                if (*j > *(j+1))
                {
                    int temp = *j;
                    *j = *(j+1);
                    *(j+1) = temp;
                    sortOccurred = true;
                }
            }
            if (!sortOccurred) {
                break;
            }
        }
        return elements;
    }

    int main()
    {
        Print(BubbleSortDescending({ 7,6,5,4,3,2 }));
    }

</code>
