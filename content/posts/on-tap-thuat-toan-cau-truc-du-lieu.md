---
title: "Thuật toán và Cấu trúc dữ liệu trong Java và JavaScript"
date: 2025-10-17
draft: false
tags: ["java", "javascript", "thuật-toán", "cấu-trúc-dữ-liệu", "phỏng-vấn", "lập-trình"]
categories: ["học-tập"]
summary: "Hướng dẫn ôn tập thuật toán và cấu trúc dữ liệu với Java và JavaScript, từ cơ bản đến nâng cao."
---

# Thuật toán và Cấu trúc dữ liệu trong Java và JavaScript

Chào các bạn! Hôm nay mình sẽ chia sẻ về cách ôn tập thuật toán và cấu trúc dữ liệu hiệu quả với Java và JavaScript. Đây là kiến thức nền tảng quan trọng cho mọi lập trình viên! 😊

## 🎯 Tại sao cần học thuật toán và cấu trúc dữ liệu?

### Lợi ích thực tế
- **Tối ưu hiệu suất**: Code chạy nhanh hơn, tiết kiệm tài nguyên
- **Giải quyết vấn đề**: Tư duy có cấu trúc, logic rõ ràng
- **Phỏng vấn**: Các công ty lớn đều test thuật toán
- **Career growth**: Thăng tiến nhanh, lương cao hơn
- **Hiểu sâu**: Nắm được cách hoạt động của các thư viện

### Ví dụ thực tế
```java
// Không tối ưu - O(n²)
public boolean hasDuplicate(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        for (int j = i + 1; j < arr.length; j++) {
            if (arr[i] == arr[j]) return true;
        }
    }
    return false;
}

// Tối ưu - O(n)
public boolean hasDuplicate(int[] arr) {
    Set<Integer> seen = new HashSet<>();
    for (int num : arr) {
        if (seen.contains(num)) return true;
        seen.add(num);
    }
    return false;
}
```

## 📊 Cấu trúc dữ liệu cơ bản

### 1. Array (Mảng)

#### Java
```java
// Khai báo và khởi tạo
int[] numbers = {1, 2, 3, 4, 5};
int[] dynamicArray = new int[10];

// Thao tác cơ bản
numbers[0] = 10;                    // O(1)
int length = numbers.length;        // O(1)
int element = numbers[2];           // O(1)

// Duyệt mảng
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}

// Enhanced for loop
for (int num : numbers) {
    System.out.println(num);
}
```

#### JavaScript
```javascript
// Khai báo và khởi tạo
const numbers = [1, 2, 3, 4, 5];
const dynamicArray = new Array(10);

// Thao tác cơ bản
numbers[0] = 10;                    // O(1)
const length = numbers.length;      // O(1)
const element = numbers[2];         // O(1)

// Duyệt mảng
for (let i = 0; i < numbers.length; i++) {
    console.log(numbers[i]);
}

// Modern methods
numbers.forEach(num => console.log(num));
numbers.map(num => num * 2);
numbers.filter(num => num > 2);
```

### 2. Linked List (Danh sách liên kết)

#### Java Implementation
```java
class ListNode {
    int val;
    ListNode next;
    
    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}

class LinkedList {
    private ListNode head;
    
    // Thêm vào đầu - O(1)
    public void addFirst(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        head = newNode;
    }
    
    // Thêm vào cuối - O(n)
    public void addLast(int val) {
        ListNode newNode = new ListNode(val);
        if (head == null) {
            head = newNode;
            return;
        }
        
        ListNode current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
    }
    
    // Tìm kiếm - O(n)
    public boolean contains(int val) {
        ListNode current = head;
        while (current != null) {
            if (current.val == val) return true;
            current = current.next;
        }
        return false;
    }
    
    // Xóa - O(n)
    public boolean remove(int val) {
        if (head == null) return false;
        if (head.val == val) {
            head = head.next;
            return true;
        }
        
        ListNode current = head;
        while (current.next != null) {
            if (current.next.val == val) {
                current.next = current.next.next;
                return true;
            }
            current = current.next;
        }
        return false;
    }
}
```

#### JavaScript Implementation
```javascript
class ListNode {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
    }
    
    // Thêm vào đầu - O(1)
    addFirst(val) {
        const newNode = new ListNode(val);
        newNode.next = this.head;
        this.head = newNode;
    }
    
    // Thêm vào cuối - O(n)
    addLast(val) {
        const newNode = new ListNode(val);
        if (!this.head) {
            this.head = newNode;
            return;
        }
        
        let current = this.head;
        while (current.next) {
            current = current.next;
        }
        current.next = newNode;
    }
    
    // Tìm kiếm - O(n)
    contains(val) {
        let current = this.head;
        while (current) {
            if (current.val === val) return true;
            current = current.next;
        }
        return false;
    }
}
```

### 3. Stack (Ngăn xếp)

#### Java Implementation
```java
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        
        // Push - O(1)
        stack.push(1);
        stack.push(2);
        stack.push(3);
        
        // Pop - O(1)
        int top = stack.pop(); // 3
        
        // Peek - O(1)
        int peek = stack.peek(); // 2
        
        // Check empty
        boolean isEmpty = stack.isEmpty();
        
        // Size
        int size = stack.size();
    }
}

// Custom Stack Implementation
class CustomStack<T> {
    private Object[] data;
    private int top;
    private int capacity;
    
    public CustomStack(int capacity) {
        this.capacity = capacity;
        this.data = new Object[capacity];
        this.top = -1;
    }
    
    public void push(T item) {
        if (top == capacity - 1) {
            throw new RuntimeException("Stack overflow");
        }
        data[++top] = item;
    }
    
    public T pop() {
        if (top == -1) {
            throw new RuntimeException("Stack underflow");
        }
        return (T) data[top--];
    }
    
    public T peek() {
        if (top == -1) {
            throw new RuntimeException("Stack is empty");
        }
        return (T) data[top];
    }
    
    public boolean isEmpty() {
        return top == -1;
    }
}
```

#### JavaScript Implementation
```javascript
class Stack {
    constructor() {
        this.items = [];
    }
    
    // Push - O(1)
    push(item) {
        this.items.push(item);
    }
    
    // Pop - O(1)
    pop() {
        if (this.isEmpty()) {
            throw new Error("Stack underflow");
        }
        return this.items.pop();
    }
    
    // Peek - O(1)
    peek() {
        if (this.isEmpty()) {
            throw new Error("Stack is empty");
        }
        return this.items[this.items.length - 1];
    }
    
    // Check empty
    isEmpty() {
        return this.items.length === 0;
    }
    
    // Size
    size() {
        return this.items.length;
    }
}

// Usage
const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
console.log(stack.pop()); // 3
console.log(stack.peek()); // 2
```

### 4. Queue (Hàng đợi)

#### Java Implementation
```java
import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();
        
        // Enqueue - O(1)
        queue.offer(1);
        queue.offer(2);
        queue.offer(3);
        
        // Dequeue - O(1)
        int front = queue.poll(); // 1
        
        // Peek - O(1)
        int peek = queue.peek(); // 2
        
        // Check empty
        boolean isEmpty = queue.isEmpty();
    }
}

// Custom Queue Implementation
class CustomQueue<T> {
    private Object[] data;
    private int front;
    private int rear;
    private int size;
    private int capacity;
    
    public CustomQueue(int capacity) {
        this.capacity = capacity;
        this.data = new Object[capacity];
        this.front = 0;
        this.rear = -1;
        this.size = 0;
    }
    
    public void enqueue(T item) {
        if (size == capacity) {
            throw new RuntimeException("Queue overflow");
        }
        rear = (rear + 1) % capacity;
        data[rear] = item;
        size++;
    }
    
    public T dequeue() {
        if (size == 0) {
            throw new RuntimeException("Queue underflow");
        }
        T item = (T) data[front];
        front = (front + 1) % capacity;
        size--;
        return item;
    }
    
    public T peek() {
        if (size == 0) {
            throw new RuntimeException("Queue is empty");
        }
        return (T) data[front];
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}
```

#### JavaScript Implementation
```javascript
class Queue {
    constructor() {
        this.items = [];
    }
    
    // Enqueue - O(1)
    enqueue(item) {
        this.items.push(item);
    }
    
    // Dequeue - O(1)
    dequeue() {
        if (this.isEmpty()) {
            throw new Error("Queue underflow");
        }
        return this.items.shift();
    }
    
    // Peek - O(1)
    front() {
        if (this.isEmpty()) {
            throw new Error("Queue is empty");
        }
        return this.items[0];
    }
    
    // Check empty
    isEmpty() {
        return this.items.length === 0;
    }
    
    // Size
    size() {
        return this.items.length;
    }
}
```

## 🔍 Thuật toán tìm kiếm

### 1. Linear Search (Tìm kiếm tuyến tính)

#### Java
```java
public class SearchAlgorithms {
    // Linear Search - O(n)
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1; // Not found
    }
    
    // Linear Search with enhanced for loop
    public static int linearSearchEnhanced(int[] arr, int target) {
        int index = 0;
        for (int num : arr) {
            if (num == target) {
                return index;
            }
            index++;
        }
        return -1;
    }
}
```

#### JavaScript
```javascript
// Linear Search - O(n)
function linearSearch(arr, target) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) {
            return i;
        }
    }
    return -1; // Not found
}

// Linear Search with forEach
function linearSearchForEach(arr, target) {
    let index = 0;
    for (const num of arr) {
        if (num === target) {
            return index;
        }
        index++;
    }
    return -1;
}
```

### 2. Binary Search (Tìm kiếm nhị phân)

#### Java
```java
// Binary Search - O(log n)
public static int binarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1; // Not found
}

// Recursive Binary Search
public static int binarySearchRecursive(int[] arr, int target, int left, int right) {
    if (left > right) {
        return -1;
    }
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, right);
    } else {
        return binarySearchRecursive(arr, target, left, mid - 1);
    }
}
```

#### JavaScript
```javascript
// Binary Search - O(log n)
function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    
    while (left <= right) {
        const mid = Math.floor(left + (right - left) / 2);
        
        if (arr[mid] === target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1; // Not found
}

// Recursive Binary Search
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
    if (left > right) {
        return -1;
    }
    
    const mid = Math.floor(left + (right - left) / 2);
    
    if (arr[mid] === target) {
        return mid;
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, right);
    } else {
        return binarySearchRecursive(arr, target, left, mid - 1);
    }
}
```

## 🔄 Thuật toán sắp xếp

### 1. Bubble Sort

#### Java
```java
public class SortingAlgorithms {
    // Bubble Sort - O(n²)
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            // If no swapping occurred, array is sorted
            if (!swapped) break;
        }
    }
}
```

#### JavaScript
```javascript
// Bubble Sort - O(n²)
function bubbleSort(arr) {
    const n = arr.length;
    for (let i = 0; i < n - 1; i++) {
        let swapped = false;
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                swapped = true;
            }
        }
        // If no swapping occurred, array is sorted
        if (!swapped) break;
    }
}
```

### 2. Quick Sort

#### Java
```java
// Quick Sort - O(n log n) average, O(n²) worst
public static void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pivotIndex = partition(arr, low, high);
        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
}

private static int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    
    swap(arr, i + 1, high);
    return i + 1;
}

private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

#### JavaScript
```javascript
// Quick Sort - O(n log n) average, O(n²) worst
function quickSort(arr, low = 0, high = arr.length - 1) {
    if (low < high) {
        const pivotIndex = partition(arr, low, high);
        quickSort(arr, low, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, high);
    }
}

function partition(arr, low, high) {
    const pivot = arr[high];
    let i = low - 1;
    
    for (let j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            [arr[i], arr[j]] = [arr[j], arr[i]];
        }
    }
    
    [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]];
    return i + 1;
}
```

## 🌳 Cấu trúc dữ liệu nâng cao

### 1. Binary Search Tree

#### Java Implementation
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    TreeNode(int val) {
        this.val = val;
    }
}

class BinarySearchTree {
    private TreeNode root;
    
    // Insert - O(log n) average, O(n) worst
    public void insert(int val) {
        root = insertRecursive(root, val);
    }
    
    private TreeNode insertRecursive(TreeNode node, int val) {
        if (node == null) {
            return new TreeNode(val);
        }
        
        if (val < node.val) {
            node.left = insertRecursive(node.left, val);
        } else if (val > node.val) {
            node.right = insertRecursive(node.right, val);
        }
        
        return node;
    }
    
    // Search - O(log n) average, O(n) worst
    public boolean search(int val) {
        return searchRecursive(root, val);
    }
    
    private boolean searchRecursive(TreeNode node, int val) {
        if (node == null) return false;
        if (node.val == val) return true;
        
        if (val < node.val) {
            return searchRecursive(node.left, val);
        } else {
            return searchRecursive(node.right, val);
        }
    }
    
    // Inorder Traversal - O(n)
    public void inorderTraversal() {
        inorderRecursive(root);
    }
    
    private void inorderRecursive(TreeNode node) {
        if (node != null) {
            inorderRecursive(node.left);
            System.out.print(node.val + " ");
            inorderRecursive(node.right);
        }
    }
}
```

#### JavaScript Implementation
```javascript
class TreeNode {
    constructor(val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }
    
    // Insert - O(log n) average, O(n) worst
    insert(val) {
        this.root = this.insertRecursive(this.root, val);
    }
    
    insertRecursive(node, val) {
        if (!node) {
            return new TreeNode(val);
        }
        
        if (val < node.val) {
            node.left = this.insertRecursive(node.left, val);
        } else if (val > node.val) {
            node.right = this.insertRecursive(node.right, val);
        }
        
        return node;
    }
    
    // Search - O(log n) average, O(n) worst
    search(val) {
        return this.searchRecursive(this.root, val);
    }
    
    searchRecursive(node, val) {
        if (!node) return false;
        if (node.val === val) return true;
        
        if (val < node.val) {
            return this.searchRecursive(node.left, val);
        } else {
            return this.searchRecursive(node.right, val);
        }
    }
    
    // Inorder Traversal - O(n)
    inorderTraversal() {
        const result = [];
        this.inorderRecursive(this.root, result);
        return result;
    }
    
    inorderRecursive(node, result) {
        if (node) {
            this.inorderRecursive(node.left, result);
            result.push(node.val);
            this.inorderRecursive(node.right, result);
        }
    }
}
```

### 2. Hash Table

#### Java Implementation
```java
import java.util.*;

public class HashTable<K, V> {
    private static final int INITIAL_CAPACITY = 16;
    private static final double LOAD_FACTOR = 0.75;
    
    private Entry<K, V>[] table;
    private int size;
    private int capacity;
    
    @SuppressWarnings("unchecked")
    public HashTable() {
        this.capacity = INITIAL_CAPACITY;
        this.table = new Entry[capacity];
        this.size = 0;
    }
    
    // Put - O(1) average, O(n) worst
    public void put(K key, V value) {
        if (size >= capacity * LOAD_FACTOR) {
            resize();
        }
        
        int index = hash(key);
        Entry<K, V> entry = table[index];
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                entry.value = value;
                return;
            }
            entry = entry.next;
        }
        
        table[index] = new Entry<>(key, value, table[index]);
        size++;
    }
    
    // Get - O(1) average, O(n) worst
    public V get(K key) {
        int index = hash(key);
        Entry<K, V> entry = table[index];
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
            entry = entry.next;
        }
        
        return null;
    }
    
    // Remove - O(1) average, O(n) worst
    public V remove(K key) {
        int index = hash(key);
        Entry<K, V> entry = table[index];
        Entry<K, V> prev = null;
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                if (prev == null) {
                    table[index] = entry.next;
                } else {
                    prev.next = entry.next;
                }
                size--;
                return entry.value;
            }
            prev = entry;
            entry = entry.next;
        }
        
        return null;
    }
    
    private int hash(K key) {
        return Math.abs(key.hashCode()) % capacity;
    }
    
    @SuppressWarnings("unchecked")
    private void resize() {
        Entry<K, V>[] oldTable = table;
        capacity *= 2;
        table = new Entry[capacity];
        size = 0;
        
        for (Entry<K, V> entry : oldTable) {
            while (entry != null) {
                put(entry.key, entry.value);
                entry = entry.next;
            }
        }
    }
    
    private static class Entry<K, V> {
        K key;
        V value;
        Entry<K, V> next;
        
        Entry(K key, V value, Entry<K, V> next) {
            this.key = key;
            this.value = value;
            this.next = next;
        }
    }
}
```

#### JavaScript Implementation
```javascript
class HashTable {
    constructor(initialCapacity = 16, loadFactor = 0.75) {
        this.capacity = initialCapacity;
        this.loadFactor = loadFactor;
        this.table = new Array(this.capacity);
        this.size = 0;
    }
    
    // Put - O(1) average, O(n) worst
    put(key, value) {
        if (this.size >= this.capacity * this.loadFactor) {
            this.resize();
        }
        
        const index = this.hash(key);
        let entry = this.table[index];
        
        while (entry) {
            if (entry.key === key) {
                entry.value = value;
                return;
            }
            entry = entry.next;
        }
        
        this.table[index] = { key, value, next: this.table[index] };
        this.size++;
    }
    
    // Get - O(1) average, O(n) worst
    get(key) {
        const index = this.hash(key);
        let entry = this.table[index];
        
        while (entry) {
            if (entry.key === key) {
                return entry.value;
            }
            entry = entry.next;
        }
        
        return undefined;
    }
    
    // Remove - O(1) average, O(n) worst
    remove(key) {
        const index = this.hash(key);
        let entry = this.table[index];
        let prev = null;
        
        while (entry) {
            if (entry.key === key) {
                if (prev === null) {
                    this.table[index] = entry.next;
                } else {
                    prev.next = entry.next;
                }
                this.size--;
                return entry.value;
            }
            prev = entry;
            entry = entry.next;
        }
        
        return undefined;
    }
    
    hash(key) {
        let hash = 0;
        const str = String(key);
        for (let i = 0; i < str.length; i++) {
            hash = (hash << 5) - hash + str.charCodeAt(i);
            hash = hash & hash; // Convert to 32-bit integer
        }
        return Math.abs(hash) % this.capacity;
    }
    
    resize() {
        const oldTable = this.table;
        this.capacity *= 2;
        this.table = new Array(this.capacity);
        this.size = 0;
        
        for (let entry of oldTable) {
            while (entry) {
                this.put(entry.key, entry.value);
                entry = entry.next;
            }
        }
    }
}
```

## 🎯 Thuật toán phổ biến

### 1. Two Pointers Technique

#### Java
```java
// Two Sum - O(n)
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{};
}

// Remove Duplicates from Sorted Array - O(n)
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    
    int slow = 0;
    for (int fast = 1; fast < nums.length; fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    return slow + 1;
}
```

#### JavaScript
```javascript
// Two Sum - O(n)
function twoSum(nums, target) {
    const map = new Map();
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        map.set(nums[i], i);
    }
    return [];
}

// Remove Duplicates from Sorted Array - O(n)
function removeDuplicates(nums) {
    if (nums.length === 0) return 0;
    
    let slow = 0;
    for (let fast = 1; fast < nums.length; fast++) {
        if (nums[fast] !== nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    return slow + 1;
}
```

### 2. Sliding Window Technique

#### Java
```java
// Maximum Sum of Subarray of Size K - O(n)
public int maxSumSubarray(int[] arr, int k) {
    int maxSum = 0;
    int windowSum = 0;
    
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;
    
    // Slide the window
    for (int i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}

// Longest Substring Without Repeating Characters - O(n)
public int lengthOfLongestSubstring(String s) {
    Set<Character> set = new HashSet<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        while (set.contains(s.charAt(right))) {
            set.remove(s.charAt(left));
            left++;
        }
        set.add(s.charAt(right));
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

#### JavaScript
```javascript
// Maximum Sum of Subarray of Size K - O(n)
function maxSumSubarray(arr, k) {
    let maxSum = 0;
    let windowSum = 0;
    
    // Calculate sum of first window
    for (let i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;
    
    // Slide the window
    for (let i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}

// Longest Substring Without Repeating Characters - O(n)
function lengthOfLongestSubstring(s) {
    const set = new Set();
    let left = 0;
    let maxLength = 0;
    
    for (let right = 0; right < s.length; right++) {
        while (set.has(s[right])) {
            set.delete(s[left]);
            left++;
        }
        set.add(s[right]);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

## 📚 Lộ trình ôn tập

### Tuần 1-2: Cấu trúc dữ liệu cơ bản
- Array, LinkedList
- Stack, Queue
- Hash Table
- **Bài tập**: 5-10 bài mỗi ngày

### Tuần 3-4: Thuật toán tìm kiếm và sắp xếp
- Linear Search, Binary Search
- Bubble Sort, Quick Sort, Merge Sort
- **Bài tập**: 5-10 bài mỗi ngày

### Tuần 5-6: Cấu trúc dữ liệu nâng cao
- Binary Search Tree
- Heap, Priority Queue
- Graph (BFS, DFS)
- **Bài tập**: 5-10 bài mỗi ngày

### Tuần 7-8: Thuật toán nâng cao
- Dynamic Programming
- Greedy Algorithms
- Backtracking
- **Bài tập**: 5-10 bài mỗi ngày

## 🎯 Tips học hiệu quả

### 1. Học bằng cách làm
- **Code mỗi ngày**: Dù chỉ 30 phút
- **Làm bài tập**: LeetCode, HackerRank
- **Debug nhiều**: Học từ lỗi

### 2. Hiểu Big O Notation
```java
// O(1) - Constant time
int getFirst(int[] arr) {
    return arr[0];
}

// O(n) - Linear time
int findMax(int[] arr) {
    int max = arr[0];
    for (int num : arr) {
        max = Math.max(max, num);
    }
    return max;
}

// O(n²) - Quadratic time
void bubbleSort(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr, j, j + 1);
            }
        }
    }
}
```

### 3. Thực hành với project thực tế
- **Caching**: Sử dụng Hash Table
- **Search**: Binary Search cho sorted data
- **Sorting**: Quick Sort cho large datasets
- **Tree**: BST cho hierarchical data

## 📖 Tài liệu tham khảo

### Java
- [Java Collections Framework](https://docs.oracle.com/javase/tutorial/collections/)
- [Algorithms in Java](https://algs4.cs.princeton.edu/)
- [LeetCode Java Solutions](https://leetcode.com/)

### JavaScript
- [JavaScript Algorithms](https://github.com/trekhleb/javascript-algorithms)
- [MDN Web Docs](https://developer.mozilla.org/)
- [LeetCode JavaScript Solutions](https://leetcode.com/)

### General
- [LeetCode](https://leetcode.com/)
- [HackerRank](https://www.hackerrank.com/)
- [CodeSignal](https://codesignal.com/)
- [Cracking the Coding Interview](https://www.crackingthecodinginterview.com/)

## 🎯 Kết luận

Thuật toán và cấu trúc dữ liệu là nền tảng quan trọng:

1. **Học từng bước**: Từ cơ bản đến nâng cao
2. **Thực hành thường xuyên**: Code mỗi ngày
3. **Hiểu Big O**: Tối ưu hiệu suất
4. **Áp dụng thực tế**: Sử dụng trong project

**Lời khuyên:**
- Bắt đầu với cấu trúc dữ liệu cơ bản
- Luyện tập thuật toán mỗi ngày
- Tham gia coding contests
- Đọc code của người khác

Chúc các bạn học tập thành công! Nếu có câu hỏi gì, hãy comment bên dưới nhé! 😊

---

*Bài viết được viết dựa trên kinh nghiệm học tập thực tế của tác giả. Thuật toán cần thời gian để thành thạo, hãy kiên trì luyện tập!*