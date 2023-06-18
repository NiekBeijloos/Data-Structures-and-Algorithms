# Quick Sort

The Quick Sort algorithm is a sorting algorithm that can be applied on various data structures. The algorithm's relative 'good' time complexity comes at the cost of its complexity. Quick Sort is relativly efficient in conjunction with large data sets. The working of the Quick Sort algorithm can be best illustrated by an image:

<img src=Quick-Sort.png width=80% height=80%>

The Quick Sort Algorithm uses the so-called Divide and Conquer strategy. This strategy prescribes dividing a problem into sub problems. In case of Quick Sort, this means sorting a single element, called a Pivot (=Conquer), prior to splitting the array (=Divide). The split is arranged around the sorted element (=Pivot).

## Characteristics

The Quick Sort algorithm's time complexity is in worst case O(N^2), where N are the number of elements in an array. This can be proven by a time complexity analysis. In the first iteration N-1 comparisons are performed ('-1' because the 'Pivot' is not compared with itself). In the second iteration N-2 comparisons are performed, because 1 element is already sorted and there is no self comparison performed on the Pivot. The continous untill the base case is hit. The summation of comparisons is: 

<img src=WorstCaseSummation.png width=25% height=25%>

which can be expressed as:

<img src=WorstCaseSummationSigma.png width=10% height=10%>

where 'i' is the number of comparisons. The summation expression can be expressed into the formula:

<img src=WorstCaseSummationFormula.png width=20% height=20%>

The formula can be derived by using the summation rule of an arithmetic progression:

<img src=SummationRuleArithmeticProgression.png width=30% height=30%>

Simplifying the formula of the summation of comparisons of the Quick Sort algorithm, will give:

<img src=WorstCaseSummationFormulaSimplified.png width=15% height=15%>

Ignoring the constanst and adjective terms, will give the worst case time complexity of Quick Sort:

<img src=WorstCaseTimeComplexity.png width=5% height=5%>

In best and average case, where the 'Pivot' is chosen optimally, the Quick Sort algorithm has a time complexity of O(nlog(n)). This time complexity is similair as that of the Merge sort algorithm. As for that, the complete time complexity analysis can be found on the Merge sort page.

Quick sort is prefered over merge sort when:
- Space is essential, i.e. Quick sort uses less auxiliary space, to sort the respective data set, than Merge sort.
- Speed is essential, i.e. Quick sort can profit from 'cache locality', because the elements that are sorted are all in a contiguous block of memory. This benifit accounts less for Merge sort, as Merge sort uses different memory blocks (=sub arrays).
- Sorting an array.

Merge sort is prefered over quick sort when:
- Speed stability is essential, i.e. Merge sort can garuantee nlog(n) time complexity in best, average and worst cases.
- Random access to disk is slow, i.e. Merge sort uses auxilary space to store sub parts of the total data set. This means the data is copied into RAM, which might be much faster than accessing disk memory continuously.
- Sorting stability is essential, i.e. Merge sort ensures that the order of identical elements  is preserved. This does not apply to Quick sort. 
- Sorting a linked list.

The Space Complexity is O(N) in worst case, due to the stack frames used by recursion.

## Applicability

Standard language applications:
- std::sort in C++

## Implementation
<code>

    void Print(const std::vector<int>& elements)
    {
        for (auto element : elements) {
            std::cout << element << " ";
        }
        std::cout << "\n" << "------------" << "\n";
    }

    void QuickSortAscending(std::vector<int>& elements, int start, int end)
    {
        if (end <= start) {
            return;
        }

        int pivotIndex = end;
        int pivotValue = elements.at(end);
        int i = start-1;

        for (int j = start; j < end; j++) 
        {
            if (elements.at(j) <= pivotValue)
            {
                i++;
                int temp = elements.at(i);
                elements[i] = elements.at(j);
                elements[j] = temp;
            }
        }
    
        int newPivotIndex = i + 1;
        int temp = elements.at(newPivotIndex);
        elements[newPivotIndex] = elements.at(pivotIndex);
        elements[pivotIndex] = temp;

        QuickSortAscending(elements, start, newPivotIndex-1);
        QuickSortAscending(elements, newPivotIndex+1, end);
    }

    int main()
    {
        std::vector<int> elements = { 4,3,2,1 };
        QuickSortAscending(elements, 0, 3);
        Print(elements);
    }

</code>
