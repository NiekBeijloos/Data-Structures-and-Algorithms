# Doubly-Linked List

A Doubly-Linked List is a data structure that is often used when the number of elements, that must be stored, is unknown at compile time and the elements must be traversed bi-directional. A Doubly-Linked List is especially convenient for **inserting & deleting** elements at the **front & back** of the List. However, the convenience of insertion at the back requires a 'Tail' node to be used (which is often applicable in Doubly Linked List implementation).

## Characteristics

The time complexity is defined:
- Insertion:
    - O(n), in case searching for a node is required, which might be applicable to part of Linked List:
        - Middle 
        - Back (without a tail node)
    - O(1), in case no searching for a node is required, which might be applicable to part of Linked List:
        - Front 
        - Back (with a tail node)
- Deletion:
    - O(n), in case searching for a node is required, which might be applicable to part of Linked List:
        - Middle
    - O(1), in case no searching for a node is required, which might be applicable to:
        - Front
        - Back
- Search:
    - O(n), searching for a particular node requires linear time

The space complexity is defined:
- Insertion, deletion, search: O(1), the space complexity is constant for each individual operation

## Applicability

Standard language applications:
- std::list in C++

## Implementation

<code>

    class DoublyLinkedList {
    public:
        void push_back(int* data)
        {
            Node* node = new Node{ data };
            if(m_tail)
            {
                node->previous = m_tail; // assign previous to 'old' tail
                m_tail->next = node; // assign next for 'old' tail to new node
                m_tail = node; // tail is new node
            }
            else
            {
                m_head = node;
                m_tail = node;
            }
        }

        void push_front(int* data) 
        {
            Node* node = new Node{ data };
            node->next = m_head;
            m_head->previous = node;
            m_head = node;
            if (!m_tail) {
                m_tail = m_head;
            }
        }
        
        void pop_back() {
            Node* oldTail = m_tail;
            m_tail = oldTail->previous;
            delete oldTail;
            m_tail->next = nullptr;
        }

        void pop_front() {
            Node* oldHead = m_head;
            m_head = oldHead->next;
            delete oldHead;
            m_tail->previous = nullptr;
        }

        void insert_at(int index, int* data) 
        {
            int i = 0;
            Node* shiftedNode = m_head;
            
            while (i != index) { 
                shiftedNode = shiftedNode->next;
                i++;
            }

            Node* newNode = new Node{ data };
            Node* nodeBeforeShiftedNode = shiftedNode->previous;
            
            newNode->next = shiftedNode;
            newNode->previous = nodeBeforeShiftedNode;
            shiftedNode->previous = newNode;
            nodeBeforeShiftedNode->next = newNode;
        }

        void delete_at(int index) 
        {
            int i = 0;
            Node* deletedNode = m_head;

            while (i != index) {
                deletedNode = deletedNode->next;
                i++;
            }

            deletedNode->previous->next = deletedNode->next;
            deletedNode->next->previous = deletedNode->previous;

            delete deletedNode;
        }

        void Print()
        {
            Node* node = m_head;
            while(node)
            {
                std::cout << *node->data << std::endl;
                node = node->next;
            }
        }
    private:
        struct Node
        {
            int* data = nullptr;
            Node* next = nullptr;
            Node* previous = nullptr;
        };
        Node* m_head{};
        Node* m_tail{};
    };


    int main()
    {
        DoublyLinkedList list;
        list.push_back(new int(1));
        list.push_back(new int(2));
        list.push_back(new int(3));
        list.push_front(new int(0));
        list.insert_at(1, new int(55));
        list.delete_at(1);
        list.insert_at(1, new int(33));
        list.pop_back();
        list.pop_front();
        list.Print();
    }

</code>
