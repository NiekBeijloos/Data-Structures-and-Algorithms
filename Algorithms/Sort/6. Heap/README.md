# Heap Sort

The Heap Sort algorithm is a sorting algorithm that can be applied on various data structures. The algorithm's relative 'good' time complexity comes at the cost of its complexity. Heap Sort is relatively efficient in conjunction with large data sets. The working of the Heap Sort algorithm can be best illustrated by an image:

<img src=Heap-Sort.png width=70% height=70%>

The Heap Sort algorithm prescribes building a 'Heap' data structure prior to the actual sorting phase. This can be a max or a min Heap, depending if the sort needs to happen in ascending or descending order. The Heap is a tree-based representation of an array. The characteristic of a Heap is that all the child nodes are smaller (in case of a max Heap) or larger (in case of a min Heap) than the parent node. This ultimately results in that the largest or smallest number of the sequence is defined at the root of the tree.

After the Heap is constructed, the sorting phase begins. The Heap's characteristic of the largest or smallest element at the root can be used to sort the sequence of elements. At each sort-iteration the first elements is swapped with the last and the heap is restored. This continues until all nodes are at the correct spot.

## Characteristics

The Heap Sort algorithm consists of two stages: 
- Heap construction phase
- Extraction phase

### Heap construction 
The Heap construction phase has a time complexity of O(N). Intuitively, the complexity should be O(Nlog(N)), because the tree's height equals log(N) and the number of leafs equals N. However, this time complexity is not true. 

To prove that the Heap construction phase is O(N), the work at each level of the tree must be known. Given:
- The heap construction always starts at the middle of the array, because buttom leafs do not have anything to compare against
- Each node has to do approximantly 2 comparisons to perform a swap on a single level:
    1. Compare left and right child
    2. Compare parent with biggest child

<img src=Tree.png width=70% height=70%>

The above image represents the number of comparisons on each tree level to shape the Heap. The root node has to compare against its children and the childeren of its children. I.e. it might be required to 'bubble' the root node all the way down. This means at each level there are 2 comparisons and there is only a single root node. Starting the analysis from the root node and expressing this in N, the number of elements, will result in 4 comparisons (2 comparisons on 2 levels) multiplied by the number of nodes which is N/8 (~1). Moving down in the tree will result in fewer levels, but more nodes. The final nodes do not perform any comparison, because these nodes do not have anything to compare against. The summation sequence of comparisons can be deducted from above tree:

<img src=BuildHeapSummationSequence.png width=30% height=30%>

This can be expressed into a more generic term:

<img src=BuildHeapSummationFormula.png width=30% height=30%>

Simplification of above summation is required to prove that this will become O(n). 'n' Is independent of 'i' and can be extracted from the summation:

<img src=BuildHeapSummationFormulaSimplificationStep1.png width=30% height=30%>

The summation has been simplified and a geometric series is recognized. The formula of a geometric series is expressed as:

<img src=FiniteGeometricSeriesFormula.png width=30% height=30%>

The formula seems complicated, since big-O only requires an approximation of the time complexity. To simplify even more, the upperbound of the summation can be set to infinity. This will give the following geometric series formula:

<img src=InfiniteGeometricSeriesFormula.png width=30% height=30%>

This only holds true if  r < 1. The r can be identified by providing a different view on the heap build summation:

<img src=BuildHeapSummationFormulaSimplificationStep2.png width=20% height=20%>

As can be deducted from above image, the r is equal to 1/2. This means r < 1 holds true. Unfortunately the representation of the build heap formula does not comply with the representation of a generic geometric series formula. This means a new representation must be found that 'approaches' the build heap summation more closely. Applying differentiation on the generic infinite geometric series provides a different power series, which might provide the 'appropriate' fit: 

<img src=InfiniteGeometricSeriesFormulaDerivative.png width=40% height=40%>

The derivative of the infinite geometric series formula now has the same signature (or structure) as the build heap summation:

<img src=BuildHeapSummationVSGemeotricSeriesDerivative.png width=40% height=40%>

This means the 'appropriate' fit is found and 'r' can be substituted by 1/2:

<img src=BuildHeapSummationFormulaSimplificationStep3.png width=20% height=20%>

The summation converges to 4 and can be substituted in the time complexity analysis of the heapify algorithm:

<img src=BuildHeapSummationFormulaSimplificationStep4.png width=20% height=20%>

As can be conducted from above image, the time complexity is O(N) for the build heap phase of the heap sort algorithm.

### Extraction

The Extraction phase is the second phase of the heap sort algorithm. In this phase the characteristics of the heap are used extensively. As known, the largest (or smallest) number is at the root of the heap. This means the first element (=root) of the array must be swapped with the last element to sort the first element. Then, the last element becomes the first element and needs to be 'heapified' again. 

Each element except, the final element, requires comparison. This means the lowerbound of the extraction phase summation is equal to 1 and the upperbound is equal to (n-1). The number of comparisons to heapify the swapped node into the correct place is dependent on the height of the tree. The height of a balanced tree is equal to log(n), since big-O requires an approximation, this is assummed in determining the extraction phase. At each level 2 comparisons are performed. This will give:

<img src=ExtractionPhaseInitialSummation.png width=30% height=30%>

'-1', because at the final level no comparisons are performed. The summation indicates that the number of comparisons is constant, because it is independent from 'i'. Starting from the root and bubbling the node down to the leaf, means 2 comparisons times the height of the tree. However, the statement: 'the number of comparisons is constant', is not fully correct, because during each sort action, the number of unsorted elements becomes smaller. This means, for example, the second-last node will not have to perform 2 * height of tree -1 comparisons. Taken this in consideration, would mean that the number of comparisons is dependent on 'i':

<img src=ExtractionPhaseActualSummation.png width=20% height=20%>

The summation seems to fit:

<img src=SummationOfLogarithms.png width=20% height=20%>

Using this generic formula and applying it on the summation of the heap extraction phase:

<img src=ExtractionPhaseFormula.png width=17% height=20%>

This 'form' is still too generic and needs to be simplified further by applying Stirling's approximation. Stirling's approximation is commonly used when approximating factorials. This approximation can be convenient for expressing the time complexity in big-O. Stirling's formula applied on logarithmic factorials is expressed as:

<img src=StirlingsApproximationLogarithms.png width=40% height=40%>

Applying this formula on the extraction phase formula of the heap sort algorithm, gives:

<img src=ExtractionPhaseFormulaStirlingsApproximation.png width=45% height=45%>

Ignoring the constanst and adjective terms, will give the worst case time complexity of the heap sort extraction phase:

<img src=ExtractionPhaseTimeComplexity.png width=30% height=30%>

This means the initial approximation was quite close to correctness, however now the correctness has been proven

### Conclusion

The overall time time complexity, expressed in the amount of comparisons using 'Big-O', of the Heap sort algorithm is:

<img src=HeapSortOverallTimeComplexity.png width=35% height=35%>

This formula contains the build and extraction phase of the heap sort algorithm. 'Big-O' states to 'ignore' adjective terms, which results in: 

<img src=HeapSortTimeComplexity.png width=30% height=30%>

This is the time complexity of the Heap sort algorithm in worst, average and best case. The space complexity is O(1), constant, because the algorithm does not use any auxiliary space.

### Comparison

The Heap sort algorithm is prefered over the Quick and Merge sort, because:
 - it does not 'occupy' additional auxiliary space, which merge sort does
 - it has a constant time complexity, which Quick sort does not have
 
The Quick sort algorithm is prefered in case the dataset grows, because:
- the algorithm performs only 'required' swaps, which is not the case for Heap sort or Merge sort. Heap sort builds a heap, even when the data is already sorted. Merge sort always copies its data into temporary memory, even when the data is already sorted.

## Applicability

Standard language applications:
- std::sort or std::sort_heap in C++

## Implementation

<code>

    void Print(const std::vector<int>& elements)
    {
        for (auto element : elements) {
            std::cout << element << " ";
        }
        std::cout << "\n" << "------------" << "\n";
    }

    void MaxHeapify(std::vector<int>& elements, int nodeIndex, int size)
    {
        int largestIndex = nodeIndex;

        int lhsChildIndex = 2 * nodeIndex + 1;
        int rhsChildIndex = 2 * nodeIndex + 2;

        if (lhsChildIndex < size && elements.at(lhsChildIndex) > elements.at(largestIndex))
        {
            largestIndex = lhsChildIndex;
        }
        if (rhsChildIndex < size && elements.at(rhsChildIndex) > elements.at(largestIndex))
        {
            largestIndex = rhsChildIndex;
        }
        if (largestIndex != nodeIndex)
        {
            int temp = elements.at(nodeIndex);
            elements[nodeIndex] = elements.at(largestIndex);
            elements[largestIndex] = temp;
            nodeIndex = largestIndex;
            MaxHeapify(elements, nodeIndex, size);
        }
    }

    void HeapSortAscending(std::vector<int>& elements)
    {
        int midIndex = (elements.size() / 2) - 1;
        for (int i = midIndex; i >= 0; i--)
        {
            MaxHeapify(elements, i, elements.size());
        }

        for (int i = elements.size()-1; i > 0; i--) {
            int largest = *elements.begin();
            *elements.begin() = elements.at(i);
            elements.at(i) = largest;
            MaxHeapify(elements, 0, i);
        }
    }

    int main()
    {
        std::vector<int> elements = { 1,2,3,4 };
        HeapSortAscending(elements);
        Print(elements);
    }

</code>