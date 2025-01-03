# Balanced Binary Search Tree (AVL)

A Balanced Binary Search Tree is a tree that has the following properties:
- Each node in a Binary Search Tree can have 0, 1 or 2 children nodes
- The left node is always smaller than the parent node
- The right node is always bigger than the parent node
- The left and the right sub-tree have ~even heights (expressed by the Balanced Factor, which is equal to -1/+1 in case of an Balanced Binary Search Tree)
- The Tree uses Left, Right, Left-Right or Right-Left Rotation to remain balanced

<img src=BST.png width=70% height=70%>

## Characteristics

The time complexity of the insert, search and delete operation on a Balanced Binary Search Tree is defined:
- Insertion: 
    - O(log(N)), where the difference between the height of the left subtree and right subtree is not more than x, where x is usually equal to 1. A balanced tree allows ~halving the number of nodes to visit during each node traversal.

    <img src=BalancedAndUnbalancedTree.jpg width=70% height=70%>
- Search:
    - O(log(N))
- Deletion:
    - O(log(N)), just like Insertion, Deletion also requires searching for the node that must be deleted. Re-arrange the tree after a node has been deleted is independent of the number of nodes. As for that it is consider O(1), constant time complexity.

The space complexity is defined:
- Insertion, deletion, search: (log(N))

## Applicability

Standard language applications:

- std::set & std::map in C++ use a BST variant

## Implementation

<code>

    struct Node {
    public:
        Node(int value)
            :Value(value)
            ,Height(0)
        {
        }
        Node* lhs{};
        Node* rhs{};
        int Value;
        int Height;
    };

    class BST {
    public:
        BST()
        {
        }

        void Insert(int value)
        {
            m_root = InsertRecursively(m_root, value);
        }

        Node* RightRotate(Node* oldRoot)
        {
            Node* newRoot = oldRoot->lhs;
            oldRoot->lhs = newRoot->rhs;
            newRoot->rhs = oldRoot;

            oldRoot->Height = std::max(Height(oldRoot->lhs), Height(oldRoot->rhs)) + 1;
            newRoot->Height = std::max(Height(newRoot->lhs), Height(newRoot->lhs)) + 1;

            return newRoot;
        }

        Node* LeftRotate(Node* oldRoot)
        {
            Node* newRoot = oldRoot->rhs;
            oldRoot->rhs = newRoot->lhs;
            newRoot->lhs = oldRoot;

            oldRoot->Height = std::max(Height(oldRoot->lhs), Height(oldRoot->rhs)) + 1;
            newRoot->Height = std::max(Height(newRoot->lhs), Height(newRoot->lhs)) + 1;

            return newRoot;
        }

        int BalanceFactor(Node* node) {

            return (Height(node->lhs) - Height(node->rhs));
        }

        int Height(Node* node) {
            if (!node) {
                return -1; 
            }
            return node->Height;
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

            node->Height = std::max(Height(node->lhs), Height(node->rhs)) + 1;

            if (BalanceFactor(node) > 1 && value < node->lhs->Value) {
                return RightRotate(node);
            }
            if (BalanceFactor(node) < -1 && value > node->rhs->Value) {
                return LeftRotate(node);
            }
            if (BalanceFactor(node) > 1 && value > node->lhs->Value) {
                node->lhs = LeftRotate(node->lhs);
                return RightRotate(node);
            }
            if (BalanceFactor(node) < -1 && value < node->rhs->Value) {
                node->rhs = RightRotate(node->rhs);
                return LeftRotate(node);
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
            }
            else if (value > node->Value) {
                node->rhs = DeleteRecursively(node->rhs, value);
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
                }
            }

            node->Height = std::max(Height(node->lhs), Height(node->rhs)) + 1;

            if (BalanceFactor(node) > 1 && BalanceFactor(node->lhs) >= 0) {
                return RightRotate(node);
            }
            if (BalanceFactor(node) > 1 && BalanceFactor(node->lhs) < 0) {
                node->lhs = LeftRotate(node->lhs);
                return RightRotate(node);
            }
            if (BalanceFactor(node) < -1 && BalanceFactor(node->rhs) <= 0) {
                return LeftRotate(node);
            }
            if (BalanceFactor(node) < -1 && BalanceFactor(node->rhs) > 0) {
                node->rhs = RightRotate(node->rhs);
                return LeftRotate(node);
            }
            return node;
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
        Node* m_root{};
    };

    int main()
    {
        BST tree {};
        tree.Insert(10);
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