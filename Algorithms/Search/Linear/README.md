# Linear Search

Linear Search is the most simplistic search algorithm applicable to Linear data structures. The algorithm iterates over each element within the Linear data structure until the the desired element is found. 

## Characteristics

The time complexity, as the name implies, is O(N). The space complexity of Linear Search is O(N), depending on the implementation.

## Implementation
<code>

    int SearchForElementIn(const std::vector<int>& elements, int element) {
        for (auto item : elements) {
            if (item == element) {
                return element;
            }
        }
        return -1;
    }

    int main()
    {
        std::cout << SearchForElementIn({ 1,2,3 }, 1) << "\n";
    }

</code>
