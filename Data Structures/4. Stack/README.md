# Stack

A stack is a data structure that is often used when the elements must be pushed and popped according to LIFO (= Last In First Out). The internal implementation of a stack varies from a Dynamic Array to usage of a Linked List. 

## Characteristics

The characteristics (time and space complexity) of a Stack are not unique, because the data structure is based on a Dynamic Array or Linked List. The Stack is purely a convenient abstraction to manipulate data in a LIFO approach. 

## Applicability

Standard language applications:
- std::stack in C++

## Implementation
<code>

    //LIFO: Last In First Out
    class Stack {

    public:
        void push(int data) {
            elements.push_back(data);
        }

        void pop() {
            elements.pop_back();
        }

        int top() {
            return elements.back();
        }

        void print() {
            for (auto it = elements.rbegin(); it != elements.rend(); it++) {
                std::cout << *it << std::endl;
            }
        }
    private:
        std::list<int> elements;
    };

    int main()
    {
        Stack stack;
        stack.push(0);
        stack.push(1);
        stack.print();
        std::cout << "-------------\n";
        stack.pop();
        stack.print();
        std::cout << "-------------\n";
        stack.push(2);
        std::cout << stack.top();
    }

</code>
