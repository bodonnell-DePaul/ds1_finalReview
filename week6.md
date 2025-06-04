# csc402_week6
# Binary Trees
Imagine a tree in your backyard. It has a trunk, branches, and leaves. A Binary Tree is like that tree, but it has a special rule: each branch (or node) can only have up to two smaller branches (or children).

# Important Points:
Node: Each point on the tree is called a node. It can hold a value.
Root: The top node of the tree.
Children: The nodes that come out from another node.
Leaf: A node with no children.
Parent: A node that has branches coming out of it.
Example:
```
       1
      / \
     2   3
    / \
   4   5
```
In this tree:

1 is the root.
2 and 3 are children of 1.
4 and 5 are children of 2.
4, 5, and 3 are leaves.

# Binary Search Trees (BST)
A Binary Search Tree is a special kind of binary tree. It follows these rules:

The left child of a node has a value less than its parent.
The right child of a node has a value greater than its parent.
Example:
```
       10
      /  \
     5   15
    / \    \
   3   7   20
```
In this tree:

5 is less than 10 and is on the left.
15 is greater than 10 and is on the right.
3 and 7 are less than 5 and are on the left and right of 5, respectively.
20 is greater than 15 and is on the right.

```Java
class TreeNode {
    int value;
    TreeNode left;
    TreeNode right;

    TreeNode(int value) {
        this.value = value;
        left = null;
        right = null;
    }
}
```
```Java
class BinarySearchTree {
    TreeNode root;

    BinarySearchTree() {
        root = null;
    }

    void insert(int value) {
        root = insertRec(root, value);
    }

    TreeNode insertRec(TreeNode root, int value) 
    {
        if (root == null) {
            root = new TreeNode(value);
            return root;
        }

        if (value < root.value) {
            root.left = insertRec(root.left, value);
        } else if (value > root.value) {
            root.right = insertRec(root.right, value);
        }

        return root;
    }

    boolean search(int value) {
        return searchRec(root, value);
    }

    boolean searchRec(TreeNode root, int value) 
    {
        if (root == null) {
            return false;
        }

        if (root.value == value) {
            return true;
        }

        if (value < root.value) {
            return searchRec(root.left, value);
        } else {
            return searchRec(root.right, value);
        }
    }

}
```

# Summary
Binary Trees are like trees with nodes that can have up to two children.
Binary Search Trees are special binary trees where the left child is smaller and the right child is larger than the parent.
Java code can help create and manage these trees.


# Algorithmic Complexity of Binary Search
Time Complexity
Best Case: \( O(1) \)
This occurs when the middle element is the target element.

Average Case: \( O(\log n) \)
This is because with each step, the algorithm divides the search interval in half.

Worst Case: \( O(\log n) \)
This occurs when the target element is not in the list, or is located at the end of the search interval.

Space Complexity
Space Complexity: \( O(1) \)
Binary search only requires a constant amount of additional space for variables, regardless of the input size.

# Binary Search Trees (BSTs) have several practical use cases due to their efficient search, insertion, and deletion operations. Here are a few examples:
1. Database Indexing
BSTs are often used in databases to implement indexing. Indexes help in quickly locating data without having to search every row in a database table. For example, a BST can be used to index a column of a database table, allowing for fast retrieval of records.
2. File Systems
File systems use BSTs to manage directories and files. For instance, the B-tree, a type of self-balancing BST, is used in many file systems to keep track of files and directories, allowing for efficient file retrieval and storage.
3. Autocomplete Features
BSTs can be used to implement autocomplete features in search engines and text editors. By storing words in a BST, the system can quickly find and suggest words that match the user's input.
4. Network Routing Algorithms
BSTs are used in network routing algorithms to manage and optimize the paths that data packets take across a network. They help in efficiently finding the shortest path between nodes in a network.
5. Symbol Tables in Compilers
Compilers use BSTs to implement symbol tables, which store information about variables, functions, and other entities in a program. This allows the compiler to quickly look up and manage these entities during the compilation process.

# Differences Between Binary Search Trees (BSTs) and Balanced Search Trees
Binary Search Trees (BSTs) and Balanced Search Trees are both data structures used for organizing and managing data efficiently. However, they have some key differences, particularly in how they handle the balance of the tree, which affects their performance.
# Binary Search Trees (BSTs)
## Structure:
A BST is a tree where each node has at most two children.
For any given node, the left child's value is less than the node's value, and the right child's value is greater.
## Performance:
The time complexity for search, insertion, and deletion operations in a BST is \(O(h)\), where \(h\) is the height of the tree.
In the worst case, the height of the tree can be as large as \(n\) (where \(n\) is the number of nodes), making the operations \(O(n)\). This happens when the tree becomes skewed (like a linked list).
## Balancing:
A standard BST does not automatically balance itself. The structure depends on the order of insertion of elements.

# Balanced Search Trees
Balanced Search Trees are a type of BST that maintains a balanced structure to ensure efficient operations. Examples include AVL trees, Red-Black trees, and B-trees.
## Structure:
These trees use additional properties and rotations to keep the tree balanced.
The height of the tree is kept as small as possible, typically \(O(\log n)\).
## Performance:
The time complexity for search, insertion, and deletion operations is \(O(\log n)\) due to the balanced nature of the tree.
This ensures consistently efficient performance even in the worst case.
## Balancing:
Balanced trees automatically adjust their structure during insertions and deletions to maintain balance.
For example, AVL trees use rotations to maintain balance, ensuring that the heights of the two child subtrees of any node differ by at most one.