# Singly-Linked List

A Singly-Linked List is a data structure that is often used when the number of elements, that must be stored, is unknown at compile time and the elements must be traversed in a single direction. A Singly-Linked List is especially convenient for **inserting & deleting** elements at the **front** of the List. 

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
        - Back (with a tail node)
    - O(1), in case no searching for a node is required, which might be applicable to:
        - Front
        - Back (without a tail node)
- Search:
    - O(n), searching for a particular node requires linear time

The space complexity is defined:
- Insertion, deletion, search: O(1), the space complexity is constant for each individual operation

## Applicability

Standard language applications:
- std::forward_list in C++

## Implementation

<code>

    class LinkedList {
    public:
        void push_back(int* data)
        {
            Node* node = new Node{ data };
            if(m_tail)
            {
                m_tail->next = node;
                m_tail = node;
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
            m_head = node;
            if (!m_tail) {
                m_tail = m_head;
            }
        }
        
        void pop_back() {
            Node* nodeInFrontOfRemovedTail = m_head;
            while (nodeInFrontOfRemovedTail->next != m_tail) {
                nodeInFrontOfRemovedTail = nodeInFrontOfRemovedTail->next;
            }
            delete m_tail;
            m_tail = nodeInFrontOfRemovedTail;
            m_tail->next = nullptr;
        }

        void pop_front() {
            Node* nodeToBeDeleted = m_head;
            m_head = nodeToBeDeleted->next;
            delete nodeToBeDeleted;
        }

        void insert_at(int index, int* data) 
        {
            int i = 0;
            Node* nodeBeforeShiftedNode = m_head;
            
            while (i != index-1) {
                nodeBeforeShiftedNode = nodeBeforeShiftedNode->next;
                i++;
            }

            Node* newNode = new Node{ data };
            newNode->next = nodeBeforeShiftedNode->next;
            nodeBeforeShiftedNode->next = newNode;
        }

        void delete_at(int index) 
        {
            int i = 0;
            Node* nodeBeforeDeletedNode = m_head;

            while (i != index - 1) {
                nodeBeforeDeletedNode = nodeBeforeDeletedNode->next;
                i++;
            }

            Node* nodeToBeDeleted = nodeBeforeDeletedNode->next;
            Node* nodeAfterDeletedNode = nodeToBeDeleted->next;
            nodeBeforeDeletedNode->next = nodeAfterDeletedNode;

            delete nodeToBeDeleted;
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
        };
        Node* m_head{};
        Node* m_tail{};
    };

    int main()
    {
        LinkedList list;
        list.push_back(new int(1));
        list.push_back(new int(2));
        list.push_back(new int(3));
        list.push_front(new int(0));
        list.insert_at(1, new int(52));
        list.insert_at(1, new int(54));
        list.delete_at(1);
        list.pop_back();
        list.Print();
    }

</code>
