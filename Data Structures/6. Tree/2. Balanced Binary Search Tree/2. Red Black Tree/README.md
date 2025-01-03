# Red-Black tree

A Red-Black Tree is a Balanced Binary Search Tree, which has the following properties:
- Each node can have 0, 1 or 2 children nodes
- The left node is always smaller than the parent node
- The right node is always bigger than the parent node
- The left and the right sub-tree have ~even heights, coloring (red/black) is used to maintain height balance
- The Tree uses Left, Right, Left-Right or Right-Left and Color Rotation to remain balanced
- The root and 'Nil' node(s) always have the color black
- Two or more consecutive red nodes is not allowed
- Every path from root to leaf node have the same number of black nodes

<img src=red_black_tree.png width=70% height=70%>

## Characteristics

The time complexity of the insert, search and delete operation on a red-black Tree is defined:
- Insertion: 
    - O(log(N)), where the difference between the height of the left subtree and right subtree could be more significant than the AVL tree. A Red-Black tree allows a height differences of maximum 2, where as the AVL tree allows a height difference of 1.
- Search:
    - O(log(N)), where O(log(N)) time complexity comes from ~halving the number of nodes to visit during each node traversal.
- Deletion:
    - O(log(N)), just like Insertion, Deletion also requires searching for the node that must be deleted. Re-arrange the tree after a node has been deleted is independent of the number of nodes. As for that it is consider O(1), constant time complexity.

The space complexity is defined:
- Insertion, deletion, search: (log(N))

### Comparison

A Red-Black tree is preferred over an AVL tree when:
- Worst case: 
    - the problem has a 'small' data set and is insert intensive 
        - Rationale: The re-balancing 'act' of a red-black tree is less expensive as it has less strict balancing criteria. The Red-Black tree relealizes this less strict balancing by intermittently Color Flipping instead of Rotating. As for that, the Red-Black tree is 'quicker' when inserting in case the data set is small. 
    - the problem is delete intensive
- Average case: 
  - the problem is insert intensive
  - the problem has a 'small' data set and is delete intensive

An AVL tree is preferred over an Red-Black Tree when:
- Worst case:
    - the problem has a 'large' data set and is insert intensive
        - Rationale: As the data set grows Searching becomes more 'dominant' than Rebalancing, because the height of the tree grows proportional to O(log(n)), but the Rebalancing remains ~constant. 
    - the problem is search intensive
- Average case:
    - the problem has a 'large' data set and is delete intensive
    - the problem is search intensive


## Applicability

Standard language applications:

- std::set, std::map, std::multiset, std::multimap in C++ are usually implemented as a Red-Black tree

## Implementation

<code>

    enum class color { red,black };

    struct Node {
        Node(int value) :Value(value) {}
        Node(int value, color color) :Value(value), Color(color) {}
        int Value{};
        color Color = color::red;
        Node* Lhs{};
        Node* Rhs{};
        Node* Parent{};
    };

    class RedBlackTree {
    public:
        
        void Delete(int value) 
        {
            Node* node = m_root;
            while (node) {
                if (node->Value == value) {
                    break;
                }
                else if (value < node->Value) {
                    node = node->Lhs;
                }
                else {
                    node = node->Rhs;
                }
            }
            Node* child{};
            Node* parent{};
            color color_deleted_node = GetColor(node);
            if (!node) {
                return;
            }
            else if (!node->Lhs) { // node has right child or no children
                parent = node->Parent;
                if (parent->Lhs == node) {
                    parent->Lhs = node->Rhs;
                }
                else {
                    parent->Rhs = node->Rhs;
                }
                if (node->Rhs) {
                    node->Rhs->Parent = node->Parent;
                    child = node->Rhs;
                }
                delete node;
            }
            else if (!node->Rhs) { // node has left child or no children
                parent = node->Parent;
                if (parent->Lhs == node) {
                    parent->Lhs = node->Lhs;
                }
                else {
                    parent->Rhs = node->Lhs;
                }
                if (node->Lhs) {
                    node->Lhs->Parent = node->Parent; // cannot be done when no children
                    child = node->Lhs;
                }
                
                delete node;
            }
            else {
                Node* leftMostLeafRhs = FindLeftMostLeaf(node->Rhs);
                int value = leftMostLeafRhs->Value;
                Delete(leftMostLeafRhs->Value);
                node->Value = value;
            }
            if (color_deleted_node == color::black)
            {
                RebalanceDeletion(child, parent);
            }
        }

        void RebalanceDeletion(Node* node, Node *parent)
        {
            while (parent && GetColor(node) == color::black)
            {
                Node* sibling{};
                if (node == parent->Lhs) {
                    sibling = parent->Rhs;
                    if (GetColor(sibling) == color::red)
                    {
                        // We know that the sibling's children must be black as we need to comply to red property (no consequitive red nodes)
                        // As a black node has been removed from one side, we will need to rotate to rebalance the black property
                        LeftRotate(parent);
                        MakeBlack(sibling->Rhs); // rotation re-colors rhs of sibling as well this should be undone.
                        sibling = parent->Rhs; // the node has a new sibling after rotation
                    }
                    if (GetColor(sibling->Rhs) == color::black && GetColor(sibling->Lhs) == color::black) 
                    {
                        //restore black node property (removal of a black node on one side requires re-colloring the other side)
                        //re-colloring to red is allowed as the sibling's children are black
                        sibling->Color = color::red;
                        node = parent;
                        parent = node->Parent;
                    }
                    else {
                        if (GetColor(sibling->Rhs) == color::black) {
                            //sibling has red left child and a black right child (possible nill node), this means a right rotation must be performed
                            //prior to the left rotation, as the left rotation is otherwise redundant.
                            RightRotate(sibling);
                            sibling = sibling->Parent;
                        }
                        
                        //left rotation as the sibling has a red right child and possibily also a left red child
                        //left rotation must be performed to adhere to the black property of a red black tree as re-coloring would violate the red property
                        LeftRotate(parent);
                        sibling->Color = parent->Color;
                        MakeBlack(parent);
                        MakeBlack(sibling->Rhs);
                        node = m_root; //re-balancing completed
                    }
                }
                else {
                    sibling = parent->Lhs;
                    if (GetColor(sibling) == color::red) {
                        RightRotate(parent);
                        MakeBlack(sibling->Lhs);
                        sibling = parent->Lhs;
                    }
                    if (GetColor(sibling->Lhs) == color::black && GetColor(sibling->Rhs) == color::black) {
                        MakeRed(sibling);
                        node = parent;
                        parent = node->Parent;
                    }
                    else {
                        if (GetColor(sibling->Lhs) == color::black) {
                            LeftRotate(sibling);
                            sibling = sibling->Parent; // new sibling after rotation
                        }

                        RightRotate(parent);
                        sibling->Color = parent->Color;
                        MakeBlack(parent); // recoloring required as it might be that this case is hit while the sibling has 2 red childs
                        MakeBlack(sibling->Lhs);
                        node = m_root; // end of re-balance
                    }
                }
            }
            MakeBlack(node);
        }

        Node* FindLeftMostLeaf(Node* tree)
        {
            Node* node = tree;
            while (node->Lhs) {
                node = node->Lhs;
            }

            return node;
        }

        void Insert(int value) 
        {
            Node* node = m_root;
            Node* parentNode = nullptr;
            while (node) {
                if (value < node->Value) {
                    parentNode = node;
                    node = node->Lhs;
                }
                else{
                    parentNode = node;
                    node = node->Rhs;
                }
            }
            if (!parentNode) {
                m_root = new Node(value);
                MakeBlack(m_root);
            }
            else if (value < parentNode->Value) {
                parentNode->Lhs = new Node(value);
                parentNode->Lhs->Parent = parentNode;
                RebalanceInsertion(parentNode->Lhs);
            }
            else if (value >= parentNode->Value) {
                parentNode->Rhs = new Node(value);
                parentNode->Rhs->Parent = parentNode;
                RebalanceInsertion(parentNode->Rhs);
            }
            else {}

        }

        void RebalanceInsertion(Node* node) 
        {
            if (node == m_root || node->Parent->Color != color::red) {
                return;
            }

            if (GetColor(node->Parent->Parent->Rhs) == color::red && 
                node->Parent->Parent->Lhs == node->Parent) 
            {
                ColorFlip(node->Parent->Parent);
                print("Color flip");
                RebalanceInsertion(node->Parent->Parent);
            }
            else if (GetColor(node->Parent->Parent->Lhs) == color::red && 
                node->Parent->Parent->Rhs == node->Parent) 
            {
                ColorFlip(node->Parent->Parent);
                print("Color flip");
                RebalanceInsertion(node->Parent->Parent);
            }
            else if (GetColor(node->Parent->Parent->Lhs) == color::black && 
                node->Parent->Parent->Rhs == node->Parent) 
            {
                if (node == node->Parent->Lhs) {
                    node = node->Parent; // this allows left rotation to work 'correctly'
                    RightRotate(node);
                    print("Right Rotate");
                }
                LeftRotate(node->Parent->Parent);
                print("Left Rotate");
                RebalanceInsertion(node);
            }
            else if (GetColor(node->Parent->Parent->Rhs) == color::black && 
                node->Parent->Parent->Lhs == node->Parent) 
            {
                if (node == node->Parent->Rhs) {
                    node = node->Parent; // this allows right rotation to work 'correctly'
                    LeftRotate(node->Parent);
                    print("Left Rotate");
                }
                RightRotate(node->Parent->Parent);
                print("Right Rotate");
                RebalanceInsertion(node);
            }
        }

        void print(const std::string& message) {
            std::cout << message << std::endl;
        }

        Node* ColorFlip(Node* node) {
            if (node == m_root) {
                MakeBlack(node);
            }
            else {
                MakeRed(node);
            }
            MakeBlack(node->Rhs);
            MakeBlack(node->Lhs);
            return node;
        }

        void RightRotate(Node* oldParent) {
            Node* newParent = oldParent->Lhs;
            oldParent->Lhs = newParent->Rhs;
            if (oldParent->Lhs)
            {
                oldParent->Lhs->Parent = oldParent;
            }

            newParent->Parent = oldParent->Parent;
            if (!newParent->Parent) { // if node does not have parent, it is the root node
                m_root = newParent;
            }
            else if (oldParent == oldParent->Parent->Lhs) {
                oldParent->Parent->Lhs = newParent;
            }
            else if (oldParent == oldParent->Parent->Rhs) {
                oldParent->Parent->Rhs = newParent;
            }

            newParent->Rhs = oldParent;
            oldParent->Parent = newParent;

            MakeBlack(newParent);
            MakeRed(newParent->Rhs);
            MakeRed(newParent->Lhs);
        }

        void LeftRotate(Node* oldParent) {
            Node* newParent = oldParent->Rhs;
            oldParent->Rhs = newParent->Lhs;

            if (oldParent->Rhs)
            {
                oldParent->Rhs->Parent = oldParent;
            }

            newParent->Parent = oldParent->Parent;
            if (!newParent->Parent) // if node does not have a parent, it is the root node
            {
                m_root = newParent;
            }
            else if (oldParent == oldParent->Parent->Lhs) {
                oldParent->Parent->Lhs = newParent; // ensure parent points to new child
            }
            else {
                oldParent->Parent->Rhs = newParent; // ensure parent points to new child
            }

            newParent->Lhs = oldParent;
            oldParent->Parent = newParent;

            MakeBlack(newParent);
            MakeRed(newParent->Rhs);
            MakeRed(newParent->Lhs);
        }

        color GetColor(Node* node) {
            if (!node) {
                return color::black;
            }
            return node->Color;
        }

        void MakeRed(Node* node) {
            if (!node) {
                return;
            }
            node->Color = color::red;
        }

        void MakeBlack(Node* node) {
            if (!node) {
                return;
            }
            node->Color = color::black;
        }

        void PrintPreOrder() {
            m_root = PrintPreOrderRecursively(m_root);
        }

        std::string EnumToString(color c) {
            if (c == color::red) {
                return "Red";
            }
            else {
                return "Black";
            }
        }

        Node* PrintPreOrderRecursively(Node* node) {
            if (!node) {
                return node;
            }
            std::cout << "{" << node->Value << "," << EnumToString(node->Color) << "},";
            node->Lhs = PrintPreOrderRecursively(node->Lhs);
            node->Rhs = PrintPreOrderRecursively(node->Rhs);
            return node;
        }

        std::vector<Node*> GetTreePreOrder() 
        {
            std::vector<Node*> nodes;
            m_root = CreateTreePreOrderVector(m_root, nodes);
            return nodes;
        }

        Node* CreateTreePreOrderVector(Node* node, std::vector<Node*>& nodes) 
        {
            if (!node) {
                return node;
            }

            nodes.push_back(node);
            node->Lhs = CreateTreePreOrderVector(node->Lhs, nodes);
            node->Rhs = CreateTreePreOrderVector(node->Rhs, nodes);

            return node;
        }

    private:
        Node* m_root{};
    };

    int main() {
        
        RedBlackTree tree;
        tree.Insert(10);
        tree.Insert(5);
        tree.Insert(15);

        tree.PrintPreOrder();

        while (true) {
            int value;
            std::cout << "Delete value in red-black tree: ";
            std::cin >> value;
            tree.Delete(value);
            tree.PrintPreOrder();
            std::cout << "\n";
        }
    }

</code>

## Research (in progress)

I know this question has been asked frequently, but all the answers do not really seem to satisfy me. Knowing when to choose a Red-Black tree over an AVL and vice versa is not as easy as it seems. A lot of factors come in to play to provide an accurate answer. I found two sources which actually come close to the answer:

- https://stackoverflow.com/a/28846533/10061169
- https://codedeposit.wordpress.com/2015/10/07/red-black-vs-avl/

I think benchmarking is a 'good' approach, however it doesn't explain 'why' we see what we see. What I concluded so far from my research:

1 Worst Case

1.1 Insert

The definition of 'worst-case', in case of the red-black tree and avl tree 'Insert' operation, is when data gets inserted on one side of the tree. This means data gets inserted in ascending or descending order.

When Inserting 7 nodes in a Red-Black Tree this means in worst-case:

- Rotation, Color Flip, Rotation, Color Flip, Rotation (use https://www.cs.usfca.edu/~galles/visualization/RedBlack.html for visualization)
When Inserting 7 nodes in an AVL tree this means in worst-case:

- Rotation, Rotation, Rotation, Rotation (use https://www.cs.usfca.edu/~galles/visualization/AVLtree.html for visualization)
In addition, when inserting the 7th node, the Red-Black tree has to compare 1 node more than the AVL tree, which means the search takes longer.

To conclude which tree has the best time complexity (in worst-case) for the Insert operation, a 'weight' factor has to be added to the Rotation, Color flip and Search operation in relationship to the size of the data set:

- When the data set is small, the Rebalancing (=Rotation and Color Flip) 'act' seems to outweigh the Search 'act'. This is because the height of the tree is relatively small.
- When the data set is large, the Search 'act' seems to outweigh the Rebalancing 'act'. This is because the height grows, but the Rebalancing 'act' remains constant.

The two sources (the answer froms stack overflow and the benchmark) seem to match in that regard. The benchmark shows that initially the Red-Black tree has a better time complexity, however when the data-set grows and searching becomes more 'dominant', then the AVL tree becomes 'quicker'.

1.2 Search

The definition of 'worst-case', in case of the red-black tree and avl tree 'Search' operation, is when data is distributed in such a way that it causes the heighest unbalance factor between the left and right sub tree.

When Searching a Red-Black tree, this means in worst-case:

- a height difference of 2 between the left and right sub tree
When Searching a n AVL tree, this means in worst-case :

- a height difference of 1 between the left and right sub tree
A Red-Black tree has to peform one additional compare during a 'Search' in worst-case. This makes the AVL tree 'quicker' with respect to Searching in worst-case for both smaller and larger data sets.

The two sources (the answer from Stack Overflow and the benchmark) seem to match in that regard.

2 Average Case

2.1 Insert

The definition of 'average-case', in case of the red-black tree and avl tree 'Insert' operation, is when data gets inserted on a random side of the tree. In case of the bench mark this means using a random number generator.

When 'Inserting' in a Red-Black tree, this means in average-case:

- The chance of a Color Flip becomes heigher (in comparison to a worst-case scenario) when 'Inserting' randomly on the left or right side of a Red-Black tree. As a result, less Rotations will take place in a Red-Black tree. In general, Rebalancing will be done less frequent in comparison to worst-case.
- The chance of inbalance becomes smaller (in comparison to a worst-case scenario) as nodes are more distributed. This means 'Searching' become a less 'dominant' factor.

When 'Inserting' in a AVL tree, this means in average-case:

- The AVL Tree will only benifit from the random distribution as it only has a single 'way' of rebalancing.

Random Insertion that causes inbalance will make an AVL tree Rotate, while it might make the Red-Black tree to Color Flip. Color flips are less 'expensive' than Rotations. The 'Searching' act, during Insertion, becomes less relevant as data will be more distributed on the right and left side of the tree. I.e. the chance of a height difference of 2 between right and left sub tree, in case of a Red-Black tree, becomes smaller (in comparison to an worst-case).

The Red-Black tree in the average case for the 'Insert' operation is 'quicker' than the AVL tree and is independent from the number of elements in the data set. The bench mark aligns with this statement. However, the https://stackoverflow.com/a/28846533/10061169 does not.

2.2 Search

The definition of 'average-case', in case of the Red-Black tree and AVL tree 'search' operation, is when nodes are randomly distributed between the left and right trees. In case of the bench mark this means using a random number generator.

As data gets inserted in a more distributed fashion, the chance of inbalance will reduce. This means the Red-Black tree will probably encounter a height difference of 2 less frequent in comparison to the worst-case. As 'extreme' inbalance in the Red-Black tree will arise less frequent the AVL and Red-Black tree will perform identical for the Average case 'Search' operation.

The benchmark seems to align with the AVL and Red-Black tree performing identical in the average-case for the 'Search' operation. However, the stack overflow post does not. It states that the AVL tree is always faster in terms of 'Searching'.

Questions:

1. What seems unclear to me is why the Red-Black tree in worst case is quicker for the 'Insert' operation? I.e. Why are 2 color flips and 1 additional compare in the Red-Black tree faster than 1 rotation from the AVL tree?
2. The bench mark shows that the Red-Black tree is 'quicker' in the average-case for the 'Insert' operation than the AVL tree. However, the https://stackoverflow.com/a/28846533/10061169 indicates it is not the case for larger data sets. Does that mean the stackoverflow answer didn't take the average-case into account or am I missing something?
3. The bench mark shows that the Red-Black tree and AVL tree peform similair in the average-case for the 'Search' operation. However, the https://stackoverflow.com/a/28846533/10061169 disagrees. Does that mean the stackoverflow answer didn't take the average-case into account or am I missing something?
4. Does anyone have an AVL vs Red-Black tree bench mark of the 'Delete' operation both for average and worst-case?