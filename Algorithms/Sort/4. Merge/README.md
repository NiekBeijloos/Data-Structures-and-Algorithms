# Merge Sort

The Merge Sort algorithm is a sorting algorithm that can be applied on various data structures. The complexity of the algorithm is exchanged for its relative 'good' time complexity (Merge Sort is relativly efficient in conjunction with large data sets). The working of the Merge Sort algorithm can be best illustrated by an image:

<img src=Merge-Sort.png width=80% height=80%>

The Merge Sort algorithm subdivides the original array in sub arrays until the base case, length == 1, is reached. After the array is 'chopped' into smaller pieces, the sorting and merging process starts. This process, called Divide and Conquer, continues until the 'original' array is restored and sorted.

## Characteristics

The Merge Sort algorithm has a time complexity of O(Nlog(N)), where N are the amount of elements in in array. This can be proven by a simple illustration:

<img src=TimeComplexity(tree).png width=80% height=80%>

The illustration simplifies the first illustration of the Merge Sort by combining the Divide and Conquer part of the tree. In worst case (reversed ordered array) on each subsequent level of the Tree, there are O(N-1) comparisons made during the Merge (or Conquer) stage (-1, because the last item does not require comparison). The N shrinks by half on each level relative to the previous level, because the elements are being split. The constants (2 and 4), in the illustration, represent the number of sub problems. E.g. it would require resolving 2 sub problems in case 4 single-element arrays need to be merged to 2 double-element arrays. As can be deducted from the illustration, the amount of comparisons on the i-th level can be expressed in 2^i((n/2^i)-1). 

The height of the tree must be known to express the summation formula of the amount of comparisons that are performed on a respective array of size N. The height of the tree can be expressed by log(N). E.g. an array of 4 elements has 2 levels (=height). I.e. log2(4) is equal to 2. This means that log(N) can be used to express the upperbound of the summation formula for the Merge Sort algorithm. The lower bound is expressed as 1, because without any level there is no tree. The summation of comparisons for the Merge Sort algorithm is defined:

<img src=Summation.png width=40% height=40%>

This can be expressed in two summations:

<img src=SummationSplit.png width=40% height=40%>

the summation of an Arithmetic series where the common difference is zero:

<img src=SummationRuleOfConstants.png width=50% height=50%>

is required to express the first summation:

<img src=DominantSummation.png width=15% height=15%>

into:

<img src=MergeSortBigO.png width=15% height=15%>

the second summation:

<img src=AdjectiveSummation.png width=20% height=20%>

can be expressed as:

<img src=AdjectiveSummationFormula.png width=10% height=10%>

This simplification can achieved by applying Faulhaber's formula.

The total summation of comparisons for the Merge Sort Algorithm can be expressed:

<img src=SummationTimeComplexity.png width=25% height=25%>

In big-O notation only the most dominant term is used to express the time complexity. This would result in:

<img src=MergeSortBigO.png width=15% height=15%>

The Space Complexity is O(N), because of the usage of 'intermediate' arrays and stack frames.

## Applicability

Standard language applications:
- std::stable_sort in C++

## Implementation
<code>

    std::vector<int> Merge(std::vector<int> lhs, std::vector<int> rhs)
    {
        std::vector<int> merged{};
        int rhsi = 0;
        int lhsi = 0;

        while (rhsi < rhs.size() && lhsi < lhs.size())
        {
            if (lhs.at(lhsi) < rhs.at(rhsi))
            {
                merged.push_back(lhs.at(lhsi));
                lhsi++;
            }
            else {
                merged.push_back(rhs.at(rhsi));
                rhsi++;
            }
        }

        while (rhsi < rhs.size()) {
            merged.push_back(rhs.at(rhsi));
            rhsi++;
        }

        while (lhsi < lhs.size()) {
            merged.push_back(lhs.at(lhsi));
            lhsi++;
        }

        return merged;
    }

    std::vector<int> MergeSort(std::vector<int> elements, int begin, int end) 
    {  
        if (end - begin <= 1) {
            return std::vector<int>(elements.begin() + begin, elements.begin() + end);
        }

        int mid = (begin+end)/2;

        auto lhs = MergeSort(elements, begin, mid);
        auto rhs = MergeSort(elements, mid, end);

        return Merge(lhs, rhs);
    }

    std::vector<int> MergeSortAscending(std::vector<int> elements)
    {
        auto sorted = MergeSort(elements, 0, elements.size());

        return sorted;
    }

    int main()
    {
        Print(MergeSortAscending({ 4,3,2,1 }));
    }

</code>
