DS1 - Week2
Recursion
Certainly! Here's a detailed explanation of recursion, including when to use it, its complexities, processing efficiencies, and best practices, along with Java examples.

---

## **What is Recursion?**
Recursion is a programming technique where a method calls itself to solve a problem. Each recursive call works on a smaller instance of the problem until it reaches a base case, which stops the recursion.

---

## **When to Use Recursion**
Recursion is ideal for problems that can be broken down into smaller, similar subproblems. Common use cases include:
1. **Divide-and-Conquer Algorithms**: Examples include merge sort, quicksort, and binary search.
2. **Tree Traversals**: Preorder, inorder, and postorder traversals of binary trees.
3. **Mathematical Problems**: Calculating factorials, Fibonacci numbers, or solving the Tower of Hanoi.
4. **Backtracking Problems**: Solving mazes, generating permutations, or the N-Queens problem.

---

## **Complexities and Processing Efficiencies**
1. **Time Complexity**: 
   - Recursion can lead to exponential time complexity if not optimized (e.g., naive Fibonacci calculation).
   - Optimized recursion (e.g., memoization) can reduce time complexity significantly.

2. **Space Complexity**:
   - Each recursive call adds a new frame to the call stack, which can lead to high space usage.
   - Tail recursion (if supported by the compiler) can optimize space usage by reusing stack frames.

3. **Efficiency**:
   - Recursion is elegant and reduces code complexity for problems like tree traversals.
   - However, iterative solutions are often more efficient in terms of memory usage.

---

## **Best Practices**
1. **Always Define a Base Case**: Ensure there is a condition to stop the recursion to avoid infinite loops.
2. **Avoid Redundant Calculations**: Use memoization or dynamic programming to store results of subproblems.
3. **Consider Iterative Solutions**: For problems where recursion leads to high memory usage, consider iterative approaches.
4. **Be Mindful of Stack Overflow**: For deep recursion, ensure the problem size is manageable or use tail recursion if supported.

---

## **Examples in Java**

### 1. **Factorial Calculation**
```java
public class RecursionExample {
    // Recursive method to calculate factorial
    public static int factorial(int n) {
        if (n == 0) { // Base case
            return 1;
        }
        return n * factorial(n - 1); // Recursive call
    }

    public static void main(String[] args) {
        int number = 5;
        System.out.println("Factorial of " + number + " is: " + factorial(number));
    }
}
```

### 2. **Fibonacci Sequence (With and Without Optimization)**
#### Naive Recursive Fibonacci
```java
public class Fibonacci {
    public static int fibonacci(int n) {
        if (n <= 1) { // Base cases
            return n;
        }
        return fibonacci(n - 1) + fibonacci(n - 2); // Recursive calls
    }

    public static void main(String[] args) {
        int n = 10;
        System.out.println("Fibonacci number at position " + n + " is: " + fibonacci(n));
    }
}
```

---
Recursion is widely used in software development for solving problems that have a natural hierarchical or repetitive structure. Here are some real-world applications:

---

### **1. File System Traversal**
- **Use Case**: Navigating through directories and files.
- **Example**: Recursively listing all files and subdirectories in a directory.
- **Why Recursion?**: File systems are hierarchical, making recursion a natural fit.

```java
import java.io.File;

public class FileTraversal {
    public static void listFiles(File dir) {
        if (dir.isDirectory()) {
            for (File file : dir.listFiles()) {
                listFiles(file); // Recursive call for subdirectories
            }
        }
        System.out.println(dir.getAbsolutePath()); // Print file or directory path
    }

    public static void main(String[] args) {
        File root = new File("/path/to/directory");
        listFiles(root);
    }
}
```

---

### **2. Tree Data Structures**
- **Use Case**: Operations like traversals (inorder, preorder, postorder), searching, and modifying binary trees.
- **Example**: Parsing XML/JSON data, implementing decision trees, or managing hierarchical data like organizational charts.
```java
class TreeNode {
    int val;
    TreeNode left, right;

    TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}

public class TreeTraversal {
    public static void inorderTraversal(TreeNode root) {
        if (root == null) { // Base case
            return;
        }
        inorderTraversal(root.left); // Traverse left subtree
        System.out.print(root.val + " "); // Visit node
        inorderTraversal(root.right); // Traverse right subtree
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        System.out.println("Inorder Traversal:");
        inorderTraversal(root);
    }
}
```

---

### **3. Divide-and-Conquer Algorithms**
- **Use Case**: Algorithms that split problems into smaller subproblems.
- **Examples**:
  - **Merge Sort**: Sorting an array by dividing it into halves.
  - **Quicksort**: Partitioning an array and sorting recursively.
  - **Binary Search**: Searching in a sorted array.
```Java
  import java.util.Arrays;

public class MergeSort {
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;

            mergeSort(arr, left, mid); // Sort left half
            mergeSort(arr, mid + 1, right); // Sort right half
            merge(arr, left, mid, right); // Merge sorted halves
        }
    }

    public static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        int[] leftArr = new int[n1];
        int[] rightArr = new int[n2];

        for (int i = 0; i < n1; i++) leftArr[i] = arr[left + i];
        for (int i = 0; i < n2; i++) rightArr[i] = arr[mid + 1 + i];

        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k++] = leftArr[i++];
            } else {
                arr[k++] = rightArr[j++];
            }
        }

        while (i < n1) arr[k++] = leftArr[i++];
        while (j < n2) arr[k++] = rightArr[j++];
    }

    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6, 7};
        mergeSort(arr, 0, arr.length - 1);
        System.out.println("Sorted array: " + Arrays.toString(arr));
    }
}
```

#### Binary Search
```Java
public class BinarySearch {
    public static int binarySearch(int[] arr, int target, int left, int right) {
        if (left > right) { // Base case: target not found
            return -1;
        }
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) { // Base case: target found
            return mid;
        } else if (arr[mid] > target) {
            return binarySearch(arr, target, left, mid - 1); // Search left half
        } else {
            return binarySearch(arr, target, mid + 1, right); // Search right half
        }
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9, 11};
        int target = 7;
        int result = binarySearch(arr, target, 0, arr.length - 1);
        if (result != -1) {
            System.out.println("Target found at index: " + result);
        } else {
            System.out.println("Target not found.");
        }
    }
}
```
---

### **4. Backtracking**
- **Use Case**: Solving constraint-based problems by exploring all possibilities.
- **Examples**:
  - **Sudoku Solver**: Filling a Sudoku grid by trying all valid numbers.
  - **N-Queens Problem**: Placing queens on a chessboard without conflicts.
  - **Maze Solving**: Finding a path through a maze.

```Java
public class NQueens {
    public static void solveNQueens(int n) {
        int[] board = new int[n];
        placeQueens(board, 0, n);
    }

    private static void placeQueens(int[] board, int row, int n) {
        if (row == n) {
            printBoard(board, n);
            return;
        }
        for (int col = 0; col < n; col++) {
            if (isSafe(board, row, col)) {
                board[row] = col;
                placeQueens(board, row + 1, n);
            }
        }
    }

    private static boolean isSafe(int[] board, int row, int col) {
        for (int i = 0; i < row; i++) {
            if (board[i] == col || Math.abs(board[i] - col) == Math.abs(i - row)) {
                return false;
            }
        }
        return true;
    }

    private static void printBoard(int[] board, int n) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i] == j) {
                    System.out.print("Q ");
                } else {
                    System.out.print(". ");
                }
            }
            System.out.println();
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int n = 4; // Change this value for different board sizes
        solveNQueens(n);
    }
}
```
---

### **5. Dynamic Programming**
- **Use Case**: Solving optimization problems by breaking them into overlapping subproblems.
- **Examples**:
  - **Fibonacci Sequence**: Using recursion with memoization.
  - **Knapsack Problem**: Finding the optimal subset of items to maximize value.
  - **Longest Common Subsequence**: Comparing two strings.

```Java
import java.util.HashMap;

public class FibonacciMemoization {
    private static HashMap<Integer, Integer> memo = new HashMap<>();

    public static int fibonacci(int n) {
        if (n <= 1) { // Base cases
            return n;
        }
        if (memo.containsKey(n)) { // Check if result is already computed
            return memo.get(n);
        }
        int result = fibonacci(n - 1) + fibonacci(n - 2); // Recursive calls
        memo.put(n, result); // Store result in memo
        return result;
    }

    public static void main(String[] args) {
        int n = 10;
        System.out.println("Fibonacci number at position " + n + " is: " + fibonacci(n));
    }
}
```
---

### **6. Parsing and Compilers**
- **Use Case**: Parsing expressions, syntax trees, or grammar rules.
- **Examples**:
  - **Expression Evaluation**: Parsing mathematical expressions like `3 + (2 * 5)`.
  - **Abstract Syntax Trees (AST)**: Representing code structure in compilers.

```Java
public class ExpressionEvaluation {
    public static int evaluate(String expr) {
        if (expr.length() == 1) {
            return Integer.parseInt(expr);
        }
        int operatorIndex = expr.length() / 2;
        char operator = expr.charAt(operatorIndex);
        int left = evaluate(expr.substring(0, operatorIndex));
        int right = evaluate(expr.substring(operatorIndex + 1));
        return applyOperator(left, right, operator);
    }

    private static int applyOperator(int left, int right, char operator) {
        switch (operator) {
            case '+': return left + right;
            case '-': return left - right;
            case '*': return left * right;
            case '/': return left / right;
            default: throw new IllegalArgumentException("Invalid operator");
        }
    }

    public static void main(String[] args) {
        String expr = "3+2*5"; // Simplified example
        System.out.println("Result: " + evaluate(expr));
    }
}
```
---

### **7. Game Development**
- **Use Case**: Implementing AI or solving game-related problems.
- **Examples**:
  - **Minimax Algorithm**: Used in games like chess or tic-tac-toe for decision-making.
  - **Pathfinding**: Recursive algorithms like A* or DFS for finding paths.

  ```Java
    public class MinimaxExample {
        public static int minimax(int depth, boolean isMaximizingPlayer, int[] scores, int left, int right) {
            if (left == right) { // Base case: leaf node
                return scores[left];
            }

            if (isMaximizingPlayer) {
                int maxEval = Integer.MIN_VALUE;
                for (int i = left; i <= right; i++) {
                    int eval = minimax(depth + 1, false, scores, i, i);
                    maxEval = Math.max(maxEval, eval);
                }
                return maxEval;
            } else {
                int minEval = Integer.MAX_VALUE;
                for (int i = left; i <= right; i++) {
                    int eval = minimax(depth + 1, true, scores, i, i);
                    minEval = Math.min(minEval, eval);
                }
                return minEval;
            }
        }

        public static void main(String[] args) {
            int[] scores = {3, 5, 2, 9, 12, 5, 23, 23}; // Example leaf node scores
            int result = minimax(0, true, scores, 0, scores.length - 1);
            System.out.println("Optimal value: " + result);
        }
    }
    ```

---

### **8. Graphics and Fractals**
- **Use Case**: Generating complex patterns or shapes.
- **Examples**:
  - **Fractals**: Drawing fractal patterns like the Mandelbrot set or Sierpinski triangle.
  - **Recursive Image Rendering**: Breaking an image into smaller parts for rendering.

---

### **9. Network Protocols**
- **Use Case**: Traversing or processing hierarchical data in network protocols.
- **Examples**:
  - **DNS Resolution**: Recursively resolving domain names to IP addresses.
  - **Packet Routing**: Traversing network paths recursively.

  ```Java
    import java.util.HashMap;

    public class DNSResolver {
        private static HashMap<String, String> dnsTable = new HashMap<>();

        static {
            // Simulated DNS records
            dnsTable.put("www.example.com", "192.168.1.1");
            dnsTable.put("example.com", "192.168.1.2");
            dnsTable.put("com", "192.168.1.3");
        }

        public static String resolve(String domain) {
            if (dnsTable.containsKey(domain)) {
                return dnsTable.get(domain); // Base case: domain found
            }
            int dotIndex = domain.indexOf('.');
            if (dotIndex == -1) {
                return "Domain not found"; // Base case: no more subdomains
            }
            String parentDomain = domain.substring(dotIndex + 1);
            return resolve(parentDomain); // Recursive call to resolve parent domain
        }

        public static void main(String[] args) {
            String domain = "www.example.com";
            String ip = resolve(domain);
            System.out.println("IP address for " + domain + ": " + ip);
        }
    }
    ```

---

### **10. Mathematical Computations**
- **Use Case**: Solving mathematical problems with recursive definitions.
- **Examples**:
  - **Factorials**: Computing `n!`.
  - **Power Functions**: Calculating `x^n`.
  - 

---

## **Key Takeaways**
- Recursion simplifies code for problems with a natural hierarchical structure.
- Always ensure a base case to prevent infinite recursion.
- Optimize recursive solutions using memoization or dynamic programming when possible.
- Be cautious of stack overflow for deep recursion; consider iterative solutions or tail recursion.
