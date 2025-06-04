Collecting workspace informationHere is a focused set of seven practice problems that touch every major data-structure discussed in the notes.  
Each task contains:

• A real-world scenario  
• A “standard-library” version to warm-up  
• A “from-scratch” requirement (no `java.util.*`)  
• A stub that you complete  

References to lecture material are provided for quick review.

---

## 1. Real-Time Stock Ticker – Min/Max Heap

**What You're Building**  
A financial data structure that maintains the highest and lowest stock prices in real-time. Your heap will support:
• Adding new stock prices as they arrive from market feeds
• Instantly querying the current maximum price (for sell decisions)
• Extracting prices in sorted order (for analytics)
• Handling thousands of price updates per second efficiently

**Real-World Context**  
Trading algorithms need sub-millisecond responses. A poorly implemented priority queue could cost millions in missed opportunities. Your heap must maintain O(log n) insertion while providing O(1) peak access.

Implementation Road-map  

Phase 0 – Design sketch  
• Draw the array layout after 6 inserts, mark parent/child indices.  
• List every public method with its Big-O target.

Phase 1 – Library warm-up (≤ 30 min)  
1. `PriceStream` class with  
   ```java
   private final PriorityQueue<Double> min = new PriorityQueue<>();
   private final PriorityQueue<Double> max = new PriorityQueue<>(Comparator.reverseOrder());
   ```  
2. Maintain both queues plus a `Deque<Double>` sized `k` to compute a rolling median (use two-heap trick).

Phase 2 – Build your own binary **max**-heap  
1. Start with `double[] heap = new double[16];  int size = 0;`  
2. `void add(double x)`  
   • place at `heap[size]`, `size++`, percolate-up.  
3. `double extractMax()`  
   • swap `heap[0]` ↔ `heap[size-1]`, `size--`, percolate-down.  
4. Write `private void grow()` that doubles capacity when `size == heap.length`.  
5. Unit test  
   • Insert 1 000 random doubles, extract in order, assert descending.  
   • Compare accuracy vs. Phase 1 implementation.

Checklist  
☐ add O(log n) ☐ peek O(1) ☐ extract O(log n)  
☐ No "off-by-one" on percolate loops  
☐ Dynamic resize keeps amortized complexity O(log n)

---

Starter Code & Implementation Guide
```java
public final class StockHeap {
    private double[] heap = new double[16];
    private int size = 0;

    public void add(double price) {
        // Step 1: Ensure capacity - grow array if needed
        if (size == heap.length) grow();
        
        // Step 2: Place new element at end of heap
        heap[size] = price;
        
        // Step 3: Restore heap property by moving up
        percolateUp(size);
        
        // Step 4: Update size
        size++;
    }

    private void percolateUp(int i) {
        // TODO: Implement the "bubble up" process
        // HINT: Keep comparing with parent and swapping if needed
        // Parent of index i is at (i-1)/2
        // Continue while: parent exists AND parent < current
        // Use a helper method swap(int i, int j) for clarity
    }

    public double extractMax() {
        if (size == 0) throw new RuntimeException("Empty heap");
        
        // Step 1: Save the maximum (root) to return
        double max = heap[0];
        
        // Step 2: Move last element to root, decrease size
        heap[0] = heap[--size];
        
        // Step 3: Restore heap property by moving down
        percolateDown(0);
        
        return max;
    }

    private void percolateDown(int i) {
        // TODO: Implement the "bubble down" process
        // HINT: Keep comparing with children and swapping with larger child
        // Left child of i is at 2*i+1, right child at 2*i+2
        // Continue while: has children AND heap property violated
        // Always swap with the LARGER child (max-heap property)
    }

    private void swap(int i, int j) {
        // TODO: Swap elements at indices i and j
        // This helper method will clean up your percolate methods
    }

    public double peekMax() { 
        if (size == 0) throw new RuntimeException("Empty heap");
        return heap[0]; 
    }
    
    public int size() { return size; }
    
    private void grow() { 
        // TODO: Create new array with double capacity
        // Copy all elements from old array to new array
        // Update the heap reference
        // HINT: Use System.arraycopy or Arrays.copyOf
    }
}
```

Sample Execution
```java
public static void main(String[] args) {
    StockHeap h = new StockHeap();
    for (double p : new double[]{42.7, 18.9, 35.1, 50.3}) {
        h.add(p);
        System.out.println("Added " + p + ", max is now " + h.peekMax());
    }
    
    System.out.print("Extracting in order: ");
    while (h.size() > 0) {
        System.out.print(h.extractMax() + " ");
    }
}
```

Expected Output
```
Added 42.7, max is now 42.7
Added 18.9, max is now 42.7
Added 35.1, max is now 42.7
Added 50.3, max is now 50.3
Extracting in order: 50.3 42.7 35.1 18.9 
```

---

## 2. Calendar Event Scheduler – Self-Balancing Binary Search Tree (Red-Black Tree)

**What You're Building**  
A calendar system that maintains events in chronological order while guaranteeing fast insertions, lookups, and range queries. Your tree will support:
• Scheduling new events in O(log n) time
• Finding the next/previous event relative to any time
• Querying all events in a time range
• Maintaining balance automatically (no manual rebalancing)

**Real-World Context**  
Google Calendar handles millions of events. If implemented as a simple list, finding your next meeting would be O(n). With an unbalanced BST, a busy person's schedule could degrade to O(n) in worst case. Red-black trees guarantee O(log n) for ALL operations.

Step-by-step Guide  

Phase 1 – Library prototype (TreeMap)  
a. Flesh out `Event` (id, title, start, end).  
b. Implement all four warm-up methods; instrument with `System.nanoTime()` for timing.  

Phase 2 – RB-Tree scaffolding  
1. Copy the `Node` inner class from starter.  
2. Code `RBTree.put` WITHOUT balancing first; verify it acts like an ordinary BST.  
3. Add `leftRotate` / `rightRotate` (write separate JUnit tests that build a 3-node "zig" and "zag").  
4. Implement `fixAfterInsert` using lecture case table (comment each case in code).  
5. Re-run step-2 JUnit tests; tree must now be balanced.  

Phase 3 – Range queries  
• `floorKey` / `ceilingKey` by walking down while tracking last ≤ / ≥ candidate.  
• `subSet(a,b)` (optional) – in-order traversal with pruning.

Debug tips  
• Print a dot-graph (`dot` file) after every insert; visualize with GraphViz.  
• If height > 2 log₂(n+1) for n ≥ 100, invariants are broken.

---

Starter Code & Implementation Guide
```java
public final class RBTree<K extends Comparable<K>, V> {
    private static final boolean RED = true, BLACK = false;

    private static final class Node<K,V> {
        K key; V value;
        Node<K,V> left, right, parent;
        boolean color = RED;
        
        Node(K k, V v, Node<K,V> p) { 
            key = k; value = v; parent = p; 
        }
    }

    private Node<K,V> root;

    public V put(K key, V val) {
        // Step 1: Handle empty tree case
        if (root == null) {
            root = new Node<>(key, val, null);
            root.color = BLACK;  // root is always black
            return null;
        }
        
        // Step 2: Find insertion point using standard BST logic
        Node<K,V> current = root, parent = null;
        int cmp = 0;
        
        // TODO: Navigate to insertion point
        // HINT: while (current != null) { compare keys, go left or right }
        // Remember to update parent as you go
        // If you find existing key, update value and return old value
        
        // Step 3: Create new red node and link to parent
        // TODO: Create newNode = new Node<>(key, val, parent)
        // TODO: Link parent to newNode (left or right based on final cmp)
        
        // Step 4: Fix any red-black violations
        // TODO: Call fixAfterInsert(newNode)
        
        return null;
    }

    private void fixAfterInsert(Node<K,V> x) {
        // Red-black repair algorithm - implements the 3 cases from lecture
        while (x != root && x.parent.color == RED) {
            // TODO: Determine if parent is left or right child of grandparent
            // TODO: Find uncle node
            // TODO: Implement Case 1: Uncle is red (recolor and continue)
            // TODO: Implement Case 2: Uncle is black, zig-zag (rotate to case 3)
            // TODO: Implement Case 3: Uncle is black, zig-zig (rotate and recolor)
            
            // HINT: You'll need symmetric cases for parent being left vs right child
            // HINT: Use helper methods isRed(node) to handle null nodes safely
        }
        root.color = BLACK;  // Root is always black
    }

    private void leftRotate(Node<K,V> x) {
        // TODO: Standard left rotation
        // HINT: Save x.right as y, update x.right and y.left
        // HINT: Don't forget to update parent pointers!
        // HINT: Handle case where x is the root
    }

    private void rightRotate(Node<K,V> y) {
        // TODO: Standard right rotation (symmetric to leftRotate)
    }

    // Helper method to safely check color (null nodes are black)
    private static <K,V> boolean isRed(Node<K,V> node) {
        return node != null && node.color == RED;
    }

    public V get(K key) {
        Node<K,V> n = root;
        while (n != null) {
            int cmp = key.compareTo(n.key);
            if (cmp == 0) return n.value;
            n = (cmp < 0) ? n.left : n.right;
        }
        return null;
    }

    public K ceilingKey(K key) {
        // TODO: Find smallest key >= target
        // HINT: Walk down tree, keep track of best candidate so far
        // HINT: When going left, current node becomes new candidate
        // HINT: When going right, candidate doesn't change
        return null;
    }
}
```

Sample Execution
```java
public static void main(String[] args) {
    RBTree<LocalDateTime, String> cal = new RBTree<>();
    
    LocalDateTime[] times = {
        LocalDateTime.parse("2025-06-03T09:00"),
        LocalDateTime.parse("2025-06-03T13:00"), 
        LocalDateTime.parse("2025-06-03T15:00"),
        LocalDateTime.parse("2025-06-03T11:30")
    };
    String[] events = {"Breakfast", "Exam", "Coffee", "Meeting"};
    
    for (int i = 0; i < times.length; i++) {
        cal.put(times[i], events[i]);
        System.out.println("Added: " + events[i]);
    }
    
    LocalDateTime query = LocalDateTime.parse("2025-06-03T10:00");
    System.out.println("Next event after 10 AM: " + 
        cal.get(cal.ceilingKey(query)));
}
```

Expected Output
```
Added: Breakfast
Added: Exam
Added: Coffee
Added: Meeting
Next event after 10 AM: Meeting
```

---

## 3. Web-Browser "Back/Forward" – Stack & Deque

**What You're Building**  
A browser history manager that tracks page visits and enables back/forward navigation. Your implementation will support:
• Visiting new pages (clearing forward history)
• Going back through visited pages
• Going forward through previously visited pages
• Handling edge cases (empty history, repeated visits)

**Real-World Context**  
Every browser implements this with two stacks. Your phone's "undo" feature, IDE's navigation history, and game save states all use similar stack-based approaches. Understanding this pattern is crucial for UI development.

Tasks → Detailed Steps  

Phase A – ArrayDeque prototype (10 min)  
• Drop-in replacement to understand API.  

Phase B – `CustomStack<T>`  
1. `Object[] data = new Object[8]; int top = 0;`  
2. Grow when `top == data.length`.  
3. Throw `EmptyStackException` on pop/peek when empty.  
4. Achieve 100 % branch coverage in JUnit.  

Phase C – `HistoryManager`  
• `visit(u)` clears forward-stack, pushes onto back.  
• `back()` pops from back ➜ push onto forward, return url.  
• `forward()` inverse.  
• Edge cases: empty stacks, repeated visits.

---

Starter Code & Implementation Guide
```java
public final class CustomStack<T> {
    private Object[] data = new Object[8];
    private int top = 0;  // Points to next empty slot

    public void push(T item) {
        // Step 1: Check if we need more space
        if (top == data.length) grow();
        
        // Step 2: Add item and increment top
        data[top++] = item;
    }

    @SuppressWarnings("unchecked")
    public T pop() {
        // TODO: Check if stack is empty (throw EmptyStackException)
        // TODO: Decrement top, get item, clear array slot (prevent memory leak)
        // TODO: Return the item
        // HINT: Use --top to decrement then access
    }

    @SuppressWarnings("unchecked") 
    public T peek() {
        // TODO: Check if stack is empty (throw EmptyStackException)
        // TODO: Return top item without removing it
        // HINT: top-1 is the index of the last added item
    }

    public boolean isEmpty() { return top == 0; }
    public int size() { return top; }

    public void clear() {
        // TODO: Reset top to 0, clear all array slots to prevent memory leaks
        // HINT: Use a for loop to set each slot to null
    }

    private void grow() {
        // TODO: Create new array with double capacity
        // TODO: Copy all elements from old array
        // TODO: Update data reference
        // HINT: Use System.arraycopy(data, 0, newData, 0, top)
    }
}

public final class HistoryManager {
    private final CustomStack<String> backStack = new CustomStack<>();
    private final CustomStack<String> forwardStack = new CustomStack<>();
    private String currentPage = null;

    public void visit(String url) {
        // TODO: If we have a current page, push it to back stack
        // TODO: Clear the forward stack (visiting new page kills forward history)
        // TODO: Set current page to the new URL
        // HINT: Think about what happens when you visit a new page in a real browser
    }

    public String back() {
        // TODO: Check if back stack is empty - if so, stay on current page
        // TODO: Push current page to forward stack
        // TODO: Pop from back stack and make it current
        // TODO: Return the new current page
    }

    public String forward() {
        // TODO: Similar to back() but using forward stack
        // TODO: This is the symmetric operation to back()
    }

    public String getCurrentPage() { return currentPage; }
    
    // Useful for debugging
    public String toString() {
        return "Current: " + currentPage + 
               ", Back: " + backStack.size() + 
               ", Forward: " + forwardStack.size();
    }
}
```

Sample Execution
```java
public static void main(String[] args) {
    HistoryManager browser = new HistoryManager();
    
    browser.visit("google.com");
    browser.visit("github.com");
    browser.visit("stackoverflow.com");
    
    System.out.println("Current: " + browser.getCurrentPage());
    System.out.println("Back to: " + browser.back());
    System.out.println("Back to: " + browser.back());
    System.out.println("Forward to: " + browser.forward());
    System.out.println("Current: " + browser.getCurrentPage());
}
```

Expected Output
```
Current: stackoverflow.com
Back to: github.com
Back to: google.com
Forward to: github.com
Current: github.com
```

---

## 4. URL Shortener – Custom HashMap

**What You're Building**  
A URL shortening service (like bit.ly) that maps short codes to full URLs. Your hash table will support:
• Storing short-code → full-URL mappings
• Fast lookups (O(1) average case)
• Handling hash collisions with separate chaining
• Dynamic resizing to maintain performance
• Deleting shortened URLs

**Real-World Context**  
Services like bit.ly, tinyurl.com, and YouTube's video IDs all rely on fast hash table lookups. A poorly implemented hash table could mean slow page loads and lost ad revenue. Your implementation must handle millions of URLs efficiently.

Tasks – Guided  

Phase 1 – Warm-up  
• Benchmark `HashMap` put/get for 1 M entries (use `System.currentTimeMillis`).  

Phase 2 – Core data structure  
1. `static class Entry<K,V> { final K key; V val; Entry<K,V> next; }`  
2. Hash function: `int idx = (key.hashCode() & 0x7fffffff) % buckets.length;`  
3. Rehash when `size / buckets.length > 0.75`  
4. Provide `Iterator<K>` via outer class implementing `Iterable<K>`.  

Testing  
• Load 10 000 random strings, verify collisions handled.  
• Delete half the keys, call `containsKey` on the rest.

---

Starter Code & Implementation Guide
```java
public final class CustomHashMap<K,V> {
    private static final int DEFAULT_CAPACITY = 16;
    private static final double LOAD_FACTOR = 0.75;

    // Each bucket is a linked list of entries (separate chaining)
    private static class Entry<K,V> {
        final K key;      // Keys are immutable once stored
        V value;          // Values can be updated
        Entry<K,V> next;  // Next entry in the chain
        
        Entry(K k, V v) { key = k; value = v; }
    }

    private Entry<K,V>[] buckets;
    private int size = 0;

    @SuppressWarnings("unchecked")
    public CustomHashMap() {
        buckets = new Entry[DEFAULT_CAPACITY];
    }

    public V put(K key, V value) {
        // Step 1: Check if we need to resize
        if (size >= buckets.length * LOAD_FACTOR) {
            resize();
        }
        
        // Step 2: Find the correct bucket
        int index = hash(key);
        Entry<K,V> entry = buckets[index];
        
        // Step 3: Search the chain for existing key
        while (entry != null) {
            if (entry.key.equals(key)) {
                V oldValue = entry.value;
                entry.value = value;  // Update existing entry
                return oldValue;
            }
            entry = entry.next;
        }
        
        // Step 4: Key not found, add new entry at head of chain
        Entry<K,V> newEntry = new Entry<>(key, value);
        newEntry.next = buckets[index];  // Link to old head
        buckets[index] = newEntry;       // Update head
        size++;
        return null;
    }

    public V get(K key) {
        // TODO: Hash the key to find bucket
        // TODO: Walk the chain looking for matching key
        // TODO: Return value if found, null otherwise
        // HINT: Very similar to the search loop in put()
    }

    public boolean containsKey(K key) {
        // TODO: Use get() method or implement similar search
        // TODO: Return true if key exists, false otherwise
    }

    public V remove(K key) {
        // TODO: Hash key to find bucket
        // TODO: Handle special case: removing head of chain
        // TODO: Walk chain to find key, update links to skip removed entry
        // TODO: Decrement size and return old value
        // HINT: You'll need to track previous entry to update links
    }

    private int hash(K key) {
        // Use built-in hashCode but ensure positive result
        return (key.hashCode() & 0x7fffffff) % buckets.length;
    }

    private void resize() {
        // TODO: Save old bucket array
        // TODO: Create new array with double capacity
        // TODO: Reset size to 0
        // TODO: Re-insert all entries from old array (they may hash to different buckets!)
        // HINT: Walk through each old bucket, then each entry in the chain
    }

    public int size() { return size; }
    
    // Useful for debugging - see how many collisions you have
    public void printStatistics() {
        int nonEmptyBuckets = 0;
        int maxChainLength = 0;
        for (Entry<K,V> head : buckets) {
            if (head != null) {
                nonEmptyBuckets++;
                int chainLength = 0;
                Entry<K,V> current = head;
                while (current != null) {
                    chainLength++;
                    current = current.next;
                }
                maxChainLength = Math.max(maxChainLength, chainLength);
            }
        }
        System.out.printf("Buckets: %d/%d used, max chain: %d, load factor: %.2f%n",
                nonEmptyBuckets, buckets.length, maxChainLength, (double)size/buckets.length);
    }
}
```

Sample Execution
```java
public static void main(String[] args) {
    CustomHashMap<String, String> shortener = new CustomHashMap<>();
    
    // Add some URLs
    shortener.put("abc123", "https://www.google.com");
    shortener.put("xyz789", "https://www.github.com");
    shortener.put("def456", "https://www.stackoverflow.com");
    
    System.out.println("Short URL abc123 -> " + shortener.get("abc123"));
    System.out.println("Contains xyz789? " + shortener.containsKey("xyz789"));
    
    System.out.println("Removing def456: " + shortener.remove("def456"));
    System.out.println("Contains def456? " + shortener.containsKey("def456"));
    
    System.out.println("Final size: " + shortener.size());
    shortener.printStatistics();
}
```

Expected Output
```
Short URL abc123 -> https://www.google.com
Contains xyz789? true
Removing def456: https://www.stackoverflow.com
Contains def456? false
Final size: 2
Buckets: 2/16 used, max chain: 1, load factor: 0.12
```

---

## 5. Hospital Triage – Doubly Linked List Queue

**What You're Building**  
A hospital emergency room queue that allows patients to be added at either end and removed from any position. Your list will support:
• Adding critical patients to the front of the line
• Adding regular patients to the back of the line  
• Removing specific patients by reference (O(1) when you have the node)
• Iterating through all patients in order
• Maintaining patient priorities dynamically

**Real-World Context**  
ERs need to quickly reprioritize patients as conditions change. Unlike arrays, linked lists allow O(1) insertion/deletion at any position if you have a direct reference to the node. This pattern appears in LRU caches, process schedulers, and any system requiring fast arbitrary updates.

Tasks  

Phase 1 – LinkedList prototype  
• Model `Patient` with id, name, triageLevel (1–5).  

Phase 2 – Doubly linked list  
1. Maintain both `head` & `tail`.  
2. `addFirst` / `addLast` updates both neighbours in O(1).  
3. `remove(node)` takes a **node reference** (not id) – constant time.  
4. Implement `Iterator` that starts at `head`.  
5. Add `toString` printing `[id:level]` list for debugging.

Study questions  
• Why does storing a direct node pointer allow O(1) removal?  
• What extra memory does the list pay vs. an array-based queue?

---

Starter Code & Implementation Guide
```java
public final class Patient {
    final int id;
    final String name;
    final int triageLevel;  // 1=critical, 5=minor
    
    public Patient(int id, String name, int level) {
        this.id = id; this.name = name; this.triageLevel = level;
    }
    
    public String toString() { return "[" + id + ":" + triageLevel + "]"; }
}

public final class PatientNode {
    Patient data;
    PatientNode next, prev;  // Doubly linked
    
    PatientNode(Patient p) { data = p; }
}

public final class TriageList implements Iterable<Patient> {
    private PatientNode head, tail;
    private int size = 0;

    public PatientNode addFirst(Patient patient) {
        PatientNode newNode = new PatientNode(patient);
        
        if (head == null) {
            // Empty list case
            head = tail = newNode;
        } else {
            // Non-empty list: link new node before current head
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
        return newNode;  // Return node reference for O(1) removal later
    }

    public PatientNode addLast(Patient patient) {
        // TODO: Create new node
        // TODO: Handle empty list case (similar to addFirst)
        // TODO: Handle non-empty list case (link after current tail)
        // TODO: Update tail reference and size
        // TODO: Return node reference
        // HINT: This is symmetric to addFirst but works with tail
    }

    public void remove(PatientNode node) {
        if (node == null) return;
        
        // TODO: Update the previous node's next pointer
        // TODO: Update the next node's prev pointer  
        // TODO: Handle special cases: removing head, removing tail
        // TODO: Decrement size
        
        // HINT: Four cases to consider:
        // 1. Only node in list (head == tail == node)
        // 2. Removing head (node.prev == null)
        // 3. Removing tail (node.next == null) 
        // 4. Removing middle node
    }

    public Iterator<Patient> iterator() {
        return new Iterator<Patient>() {
            private PatientNode current = head;
            
            public boolean hasNext() { return current != null; }
            
            public Patient next() {
                if (!hasNext()) throw new NoSuchElementException();
                Patient data = current.data;
                current = current.next;
                return data;
            }
        };
    }

    public String toString() {
        StringBuilder sb = new StringBuilder();
        for (Patient p : this) {
            sb.append(p).append(" ");
        }
        return sb.toString().trim();
    }

    public int size() { return size; }
    public boolean isEmpty() { return size == 0; }
    
    // Useful for debugging - verify list integrity
    public boolean checkInvariants() {
        if (size == 0) return head == null && tail == null;
        if (size == 1) return head == tail && head.prev == null && head.next == null;
        
        // Check forward links
        int forwardCount = 0;
        PatientNode current = head;
        while (current != null) {
            forwardCount++;
            current = current.next;
        }
        
        // Check backward links  
        int backwardCount = 0;
        current = tail;
        while (current != null) {
            backwardCount++;
            current = current.prev;
        }
        
        return forwardCount == size && backwardCount == size;
    }
}
```

Sample Execution
```java
public static void main(String[] args) {
    TriageList triage = new TriageList();
    
    // Add patients
    PatientNode ann = triage.addLast(new Patient(1, "Ann", 3));    // Moderate
    PatientNode bob = triage.addFirst(new Patient(2, "Bob", 1));   // Critical - jumps to front
    PatientNode carl = triage.addLast(new Patient(3, "Carl", 4));  // Minor - goes to back
    
    System.out.println("Initial queue: " + triage);
    
    // Ann gets transferred to another hospital
    triage.remove(ann);
    System.out.println("After removing Ann: " + triage);
    
    // Emergency patient arrives
    triage.addFirst(new Patient(4, "Dana", 1));
    System.out.println("After adding Dana (critical): " + triage);
    
    // Verify list is still valid
    System.out.println("List valid? " + triage.checkInvariants());
}
```

Expected Output
```
Initial queue: [2:1] [1:3] [3:4]
After removing Ann: [2:1] [3:4] 
After adding Dana (critical): [4:1] [2:1] [3:4]
List valid? true
```

---

## 6. Filesystem Explorer – Breadth-First Traversal & Custom Queue

**What You're Building**  
A file system explorer that visits directories level-by-level (like the `find` command or file manager tree view). Your queue will support:
• Breadth-first traversal of directory trees
• Tracking depth for proper indentation
• Handling circular-array wraparound efficiently
• Dynamic resizing without losing queue order

**Real-World Context**  
File indexing (Spotlight, Windows Search), backup software, and malware scanners all use BFS to systematically explore directory trees. Your queue implementation mirrors the circular buffers used in network routers and audio processing.

Tasks  

Phase A – BFS with `LinkedList`  
• Track depth using a `null` level marker or pair `<File,int depth>`.

Phase B – Circular-array queue  
1. `File[] buf = new File[32]; int front = 0, rear = 0, size = 0;`  
2. Modular arithmetic for wrap-around.  
3. Grow by doubling when `size == buf.length`.  
4. Ensure `offer`, `poll`, `isEmpty`, `size`.  
5. Re-run BFS; output order must match Phase A.

---

Starter Code & Implementation Guide
```java
public final class CustomQueue<T> {
    private Object[] buffer = new Object[32];
    private int front = 0;  // Index of first element
    private int rear = 0;   // Index of next empty slot  
    private int size = 0;   // Number of elements

    public void offer(T item) {
        // Step 1: Check if we need more space
        if (size == buffer.length) grow();
        
        // Step 2: Add item at rear position
        buffer[rear] = item;
        
        // Step 3: Advance rear with wraparound
        rear = (rear + 1) % buffer.length;
        
        // Step 4: Update size
        size++;
    }

    @SuppressWarnings("unchecked")
    public T poll() {
        if (isEmpty()) return null;
        
        // Step 1: Get item from front
        T item = (T) buffer[front];
        
        // Step 2: Clear slot to prevent memory leak
        buffer[front] = null;
        
        // Step 3: Advance front with wraparound
        front = (front + 1) % buffer.length;
        
        // Step 4: Update size
        size--;
        
        return item;
    }

    public boolean isEmpty() { return size == 0; }
    public int size() { return size; }

    private void grow() {
        // TODO: Create new buffer with double capacity
        // TODO: Copy elements in correct order (front to rear)
        // TODO: Reset front=0, rear=size for simplicity
        // HINT: Elements from front to end of array, then from 0 to rear
        // HINT: Use two System.arraycopy calls or a loop
        
        // Example approach:
        // 1. int newCapacity = buffer.length * 2;
        // 2. Object[] newBuffer = new Object[newCapacity];  
        // 3. Copy buffer[front..buffer.length-1] to newBuffer[0..]
        // 4. Copy buffer[0..rear-1] to newBuffer[buffer.length-front..]
        // 5. Update buffer, front=0, rear=size
    }
    
    // Useful for debugging
    public String toString() {
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < size; i++) {
            int index = (front + i) % buffer.length;
            sb.append(buffer[index]);
            if (i < size - 1) sb.append(", ");
        }
        return sb.append("]").toString();
    }
}

public final class DirectoryBFS {
    public static void traverse(File root) {
        CustomQueue<File> queue = new CustomQueue<>();
        CustomQueue<Integer> depths = new CustomQueue<>();  // Parallel queue for depth tracking
        
        // Start traversal
        queue.offer(root);
        depths.offer(0);
        
        while (!queue.isEmpty()) {
            File current = queue.poll();
            int depth = depths.poll();
            
            // Print with indentation (2 spaces per level)
            for (int i = 0; i < depth; i++) {
                System.out.print("  ");
            }
            System.out.println(current.getPath());
            
            // Add children to queue if current is a directory
            if (current.isDirectory()) {
                File[] children = current.listFiles();
                if (children != null) {
                    // TODO: Sort children for consistent output (optional)
                    // Arrays.sort(children);
                    
                    for (File child : children) {
                        queue.offer(child);
                        depths.offer(depth + 1);
                    }
                }
            }
        }
    }
    
    // Alternative approach using a wrapper class instead of parallel queues
    private static class FileWithDepth {
        final File file;
        final int depth;
        FileWithDepth(File f, int d) { file = f; depth = d; }
    }
    
    public static void traverseAlternative(File root) {
        CustomQueue<FileWithDepth> queue = new CustomQueue<>();
        queue.offer(new FileWithDepth(root, 0));
        
        while (!queue.isEmpty()) {
            FileWithDepth item = queue.poll();
            
            // Print with indentation
            for (int i = 0; i < item.depth; i++) {
                System.out.print("  ");
            }
            System.out.println(item.file.getPath());
            
            // Add children
            if (item.file.isDirectory()) {
                File[] children = item.file.listFiles();
                if (children != null) {
                    for (File child : children) {
                        queue.offer(new FileWithDepth(child, item.depth + 1));
                    }
                }
            }
        }
    }
}
```

Sample Execution (assuming directory structure)
```java
public static void main(String[] args) {
    // Create test directory structure:
    // /tmp/demo/
    //   a.txt
    //   sub/
    //     b.txt
    //     c.txt
    
    System.out.println("=== BFS Traversal ===");
    DirectoryBFS.traverse(new File("/tmp/demo"));
    
    System.out.println("\n=== Testing queue operations ===");
    CustomQueue<String> q = new CustomQueue<>();
    q.offer("first");
    q.offer("second"); 
    System.out.println("Queue: " + q);
    System.out.println("Poll: " + q.poll());
    System.out.println("Queue: " + q);
}
```

Expected Output
```
=== BFS Traversal ===
/tmp/demo
  /tmp/demo/a.txt
  /tmp/demo/sub
    /tmp/demo/sub/b.txt
    /tmp/demo/sub/c.txt

=== Testing queue operations ===
Queue: [first, second]
Poll: first
Queue: [second]
```

---

## 7. Autocomplete – Trie (Prefix Tree)

**What You're Building**  
An autocomplete system (like search suggestions or IDE code completion) that efficiently finds all words with a given prefix. Your trie will support:
• Inserting words in O(word length) time
• Finding all words with a prefix in O(prefix length + results) time
• Limiting results to prevent overwhelming users
• Comparing performance against tree-based alternatives

**Real-World Context**  
Google Search, VS Code intellisense, phone keyboards, and DNS resolution all use tries for fast prefix matching. A naive approach checking every word individually would be too slow for real-time suggestions.

Tasks  

Phase 1 – Core trie operations  
1. `insert(word)` for lowercase 'a'–'z' only.  
2. Mark `node.word = true` at terminal node.  

Phase 2 – `startsWith(prefix)`  
• Navigate prefix; DFS down collecting up to 10 words.  

Phase 3 – Performance comparison  
1. Load 50 k English words into both `Trie` and `TreeSet`.  
2. Time 1 000 random prefix queries length 3–5.  
3. Explain observed difference vs. O-analysis.

Edge cases  
• Duplicate insertions.  
• Prefix longer than any stored word.

---

Starter Code & Implementation Guide
```java
public final class Trie {
    private static final int ALPHABET_SIZE = 26;  // 'a' to 'z'
    
    private static class TrieNode {
        TrieNode[] children = new TrieNode[ALPHABET_SIZE];
        boolean isWord = false;  // True if this node ends a valid word
        
        // Optional: store the character for debugging
        char character;  
        TrieNode(char c) { character = c; }
        TrieNode() { character = '\0'; }  // Root node
    }
    
    private final TrieNode root = new TrieNode();

    public void insert(String word) {
        // TODO: Validate input (lowercase letters only)
        if (word == null || word.isEmpty()) return;
        
        TrieNode current = root;
        
        // TODO: For each character in word:
        // 1. Convert char to index (c - 'a')
        // 2. If child doesn't exist, create new TrieNode
        // 3. Move current to child
        // TODO: Mark final node as word ending
        
        // HINT: Use a for-each loop over word.toCharArray()
    }

    public java.util.List<String> startsWith(String prefix) {
        java.util.List<String> results = new java.util.ArrayList<>();
        
        // Step 1: Navigate to end of prefix
        TrieNode prefixNode = findPrefixNode(prefix);
        
        if (prefixNode != null) {
            // Step 2: Collect all words from this subtree
            collectWords(prefixNode, new StringBuilder(prefix), results, 10);
        }
        
        return results;
    }

    private TrieNode findPrefixNode(String prefix) {
        TrieNode current = root;
        
        // TODO: Navigate through prefix characters
        // TODO: Return null if prefix path doesn't exist
        // TODO: Return final node if prefix path exists
        
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            if (current.children[index] == null) {
                return null;  // Prefix not found
            }
            current = current.children[index];
        }
        return current;
    }

    private void collectWords(TrieNode node, StringBuilder prefix, 
                             java.util.List<String> results, int limit) {
        // Stop if we've found enough words
        if (results.size() >= limit) return;
        
        // If current node ends a word, add it to results
        if (node.isWord) {
            results.add(prefix.toString());
        }
        
        // TODO: Recursively explore all children
        // TODO: For each non-null child:
        //   1. Append character to prefix
        //   2. Recursively collect from child
        //   3. Remove character from prefix (backtrack)
        
        // HINT: Loop through all 26 possible children
        // HINT: Convert index back to character: (char)('a' + i)
        // HINT: Use prefix.append(c) and prefix.deleteCharAt(prefix.length()-1)
    }
    
    // Optional: useful for debugging
    public boolean contains(String word) {
        TrieNode node = findPrefixNode(word);
        return node != null && node.isWord;
    }
    
    // Optional: display tree structure
    public void printTrie() {
        printNode(root, "", true);
    }
    
    private void printNode(TrieNode node, String prefix, boolean isLast) {
        System.out.println(prefix + (isLast ? "└── " : "├── ") + 
                          (node.character == '\0' ? "ROOT" : node.character) +
                          (node.isWord ? " *" : ""));
        
        java.util.List<TrieNode> children = new java.util.ArrayList<>();
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            if (node.children[i] != null) {
                children.add(node.children[i]);
            }
        }
        
        for (int i = 0; i < children.size(); i++) {
            printNode(children.get(i), 
                     prefix + (isLast ? "    " : "│   "), 
                     i == children.size() - 1);
        }
    }
}
```

Sample Execution
```java
public static void main(String[] args) {
    Trie trie = new Trie();
    
    // Insert words
    String[] words = {"apple", "app", "application", "apply", "ape", "april"};
    for (String word : words) {
        trie.insert(word);
        System.out.println("Inserted: " + word);
    }
    
    // Test prefix searches
    System.out.println("\nPrefix 'app': " + trie.startsWith("app"));
    System.out.println("Prefix 'ap': " + trie.startsWith("ap"));
    System.out.println("Prefix 'xyz': " + trie.startsWith("xyz"));
    
    // Test edge cases
    System.out.println("Contains 'app': " + trie.contains("app"));
    System.out.println("Contains 'appl': " + trie.contains("appl"));
    
    // Demonstrate tree structure (optional)
    System.out.println("\nTrie structure:");
    trie.printTrie();
}
```

Expected Output
```
Inserted: apple
Inserted: app
Inserted: application
Inserted: apply
Inserted: ape
Inserted: april

Prefix 'app': [app, apple, application, apply]
Prefix 'ap': [app, ape, apple, application, apply, april]
Prefix 'xyz': []
Contains 'app': true
Contains 'appl': false

Trie structure:
└── ROOT
    └── a
        └── p
            ├── e *
            └── p *
                ├── l
                │   ├── e *
                │   └── i
                │       └── c
                │           └── a
                │               └── t
                │                   └── i
                │                       └── o
                │                           └── n *
                └── r
                    └── i
                        └── l *
```

---

### General Tips

• Unit-test each structure separately before integrating into the scenario.  
• Follow O-notation targets discussed in lectures.  
• For "from-scratch" parts, **only** `java.lang` is allowed.
• Use `assert` statements to verify invariants during development.
• Consider edge cases: empty inputs, null values, boundary conditions.
• Profile your implementations - theoretical complexity should match observed performance.