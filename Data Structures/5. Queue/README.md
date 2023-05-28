# Queue

A Queue is a data structure that is often used when the elements must be pushed and popped according to FIFO (= First In First Out). The queue uses internally a Doubly Linked List as its data structure.

## Characteristics

The characteristics (time and space complexity) of a Queue are not unique, because the data structure is based on a Doubly Linked List. The Queue is purely a convenient abstraction to manipulate data in a FIFO approach. 

## Applicability

Standard language applications:
- std::queue in C++

## Implementation
<code>

    class Queue {
    public:
        void push(int data) {
            elements.push_back(data);
        }

        void pop() {
            elements.pop_front();
        }

        int front() {
            return elements.front();
        }

        int back() {
            return elements.back();
        }

        void print() {
            for (auto element : elements) {
                std::cout << element << std::endl;
            }
        }

    private:
        std::list<int> elements;
    };

    int main()
    {
        Queue queue;
        queue.push(1);
        queue.push(2);
        queue.pop();
        queue.push(3);
        queue.print();
        std::cout << "------------------------\n";
        std::cout << queue.front() << "\n";
        std::cout << queue.back() << "\n";
    }

</code>
