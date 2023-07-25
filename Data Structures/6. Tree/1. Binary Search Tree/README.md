# Binary Search Tree

A Binary Search Tree is a tree that has the following properties:
- Each node in a Binary Search Tree can have 0, 1 or 2 children nodes
- The left node is always smaller than the parent node
- The right node is always bigger than the parent node

<img src=BST.png width=70% height=70%>

## Characteristics

The time complexity of the insert, search and delete operation on a Binary Search Tree is defined:
- Insertion: 
    - O(n), in case of an unbalanced tree (where the difference between the height of the left subtree and right subtree is more than x, where x is usually equal to 1). An unbalanced tree has the tendency to become a linked list. O(N) is derived from traversing (almost) each node, like a linear data structure. 
    - O(log(n)), in case of a balanced tree (where the difference between the height of the left subtree and right subtree is not more than x, where x is usally equal to 1). A balanced tree allows ~halving the number of nodes to visit during each node traversal.

    <img src=BalancedAndUnbalancedTree.jpg width=70% height=70%>
- Search:
    - O(n), in case of an unbalanced tree.
    - O(log(n)), in case of a balanced tree.
- Deletion:
    - O(n), in case of an unbalanced tree.
    - O(log(n)), in case of a balanced tree. 

  Just like Insertion, Deletion also requires searching for the node that must be deleted. Re-arrange the tree after a node has been deleted is independent of the number of nodes. As for that it is consider O(1), constant time complexity.

The space complexity is defined:
- Insertion, deletion, search: O(n) in case of a unbalanced tree and O(log(n)) in case of a balanced tree.

## Applicability

Standard language applications:

- std::set & std::map in C++ use a BST variant

## Implementation

<code>

    struct Node {
    public:
        Node(int value)
            :Value(value)
        {
        }
        Node* lhs{};
        Node* rhs{};
        int Value;
    };

    class BST {
    public:
        BST(Node* root)
            :m_root(root)
        {
        }

        void Insert(int value)
        {
            m_root = InsertRecursively(m_root, value);
        }

        Node* InsertRecursively(Node* node, int value)
        {
            if (node == nullptr)
            {
                return new Node(value);
            }

            if (value < node->Value)
            {
                node->lhs = InsertRecursively(node->lhs, value);
            }
            else
            {
                node->rhs = InsertRecursively(node->rhs, value);
            }

            return node;
        }

        Node* FindLeftMostNodeRecursively(Node* node)
        {
            if (!node->lhs) {
                return node;
            }
            return FindLeftMostNodeRecursively(node->lhs);
        }

        void Delete(int value)
        {
            m_root = DeleteRecursively(m_root, value);
        }

        Node* DeleteRecursively(Node* node, int value)
        {
            if (!node) {
                return node; // value does not exist in BST
            }
            if (value < node->Value) {
                node->lhs = DeleteRecursively(node->lhs, value);
                return node;
            }
            else if (value > node->Value) {
                node->rhs = DeleteRecursively(node->rhs, value);
                return node;
            }
            else {
                if (!node->lhs) { //  right child or no children
                    Node* temp = node->rhs;
                    delete node;
                    return temp;
                }
                else if (!node->rhs) { // left child or no children
                    Node* temp = node->lhs;
                    delete node;
                    return temp;
                }
                else { // left and right child
                    Node* minRightSubTree = FindLeftMostNodeRecursively(node->rhs);
                    node->Value = minRightSubTree->Value;
                    node->rhs = DeleteRecursively(node->rhs, node->Value);
                    return node;
                }
            }
        }

        Node* Find(int value)
        {
            return FindRecursively(m_root, value);
        }

        Node* FindRecursively(Node* node, int value)
        {
            if (!node || node->Value == value) {
                return node;
            }
            if (value < node->Value) {
                return FindRecursively(node->lhs, value);
            }
            else {
                return FindRecursively(node->rhs, value);
            }
        }

        void PrintPreOrder() {
            PrintPreOrderRecursively(m_root);
        }

        void PrintPreOrderRecursively(Node* node) {
            if (node == nullptr) {
                return;
            }
            std::cout << (node->Value) << ", ";
            PrintPreOrderRecursively(node->lhs);
            PrintPreOrderRecursively(node->rhs);
        }

    private:
        Node* m_root;
    };

    int main()
    {
        Node* root = new Node(10);
        BST tree { root };
        tree.Insert(13);
        tree.Insert(11);
        tree.Insert(12);
        tree.Insert(14);
        tree.Insert(8);
        tree.Insert(6);
        tree.PrintPreOrder();

        while (true) {
            int number;
            std::cout << "Enter value to delete in the BST:";
            std::cin >> number;
            tree.Delete(number);
            tree.PrintPreOrder();
        }
    }

</code>