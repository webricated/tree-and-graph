## 🧮 Problem: Range Sum Queries with Point Updates

You are given an integer array `arr` of length `n`. Your task is to efficiently perform the following two types of operations multiple times:

1. **Query** the sum of elements in a given range `[l, r]` (inclusive).
2. **Update** the value at a specific index `i` to a new value `val`.

To handle multiple such operations efficiently, you need to implement a **Segment Tree** that supports:

* **Building** the segment tree in `O(n)` time.
* **Querying** range sums in `O(log n)` time.
* **Updating** a value at a position in `O(log n)` time.

---

### 📥 Input Format

* An initial array `arr[]` of `n` integers.
* A series of operations of the form:

  * `query(l, r)` – Return the sum of elements in the range `[l, r]`.
  * `update(i, val)` – Update the value at index `i` to `val`.

---

### 📤 Output Format

* For each `query(l, r)` operation, output the resulting sum.

---

### ✅ Example

**Input:**

```text
Initial array: [1, 2, 3, 4, 5]
Operations:
query(1, 3)
update(2, 10)
query(1, 3)
```

**Output:**

```text
9
16
```

**Explanation:**

* First query: `arr[1] + arr[2] + arr[3] = 2 + 3 + 4 = 9`
* Update: `arr[2] = 10`, so array becomes `[1, 2, 10, 4, 5]`
* Second query: `arr[1] + arr[2] + arr[3] = 2 + 10 + 4 = 16`

---

### ⏱ Constraints

* `1 ≤ n ≤ 10⁵`
* `0 ≤ arr[i], val ≤ 10⁹`
* `0 ≤ l ≤ r < n`
* `0 ≤ i < n`
* Number of operations: up to 10⁵

---

### 🚀 Your Task

Implement a class `Main` (or `SegmentTree`) with:

* A constructor to initialize the segment tree with the given array.
* A method `update(int i, int val)` to update the array.
* A method `query(int l, int r)` to return the sum in range `[l, r]`.


## Code:

```Java
//class SegmentTreeCP {
class Main {
    int[] tree;
    int n;

    Main(int[] arr) {
        n = arr.length;
        tree = new int[2 * n];
        build(arr);
    }

    void build(int[] arr) {
        // Need to fill the leaves
        for (int i = 0; i < n; i++) tree[n + i] = arr[i];
        // Building Tree
        for (int i = n - 1; i > 0; i--)
            tree[i] = tree[i << 1] + tree[i << 1 | 1]; // Use sum/min/max
    }

    void update(int pos, int val) {
        pos += n;
        tree[pos] = val;
        for (pos >>= 1; pos > 0; pos >>= 1)
            tree[pos] = tree[pos << 1] + tree[pos << 1 | 1]; // Use sum/min/max
    }

    int query(int l, int r) {
        l += n; r += n + 1;
        int res = 0; // or Integer.MAX_VALUE / -∞ for min/max
        while (l < r) {
            if ((l & 1) == 1) res += tree[l++]; // Use sum/min/max
            if ((r & 1) == 1) res += tree[--r]; // Use sum/min/max
            l >>= 1; r >>= 1;
        }
        return res;
    }
    
    
    public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4, 5};
    //SegmentTreeCP st = new SegmentTreeCP(arr);
    Main st = new Main(arr);
    System.out.println(st.query(1, 3)); // Sum from 1 to 3 → 2+3+4 = 9

    st.update(2, 10); // Set index 2 to 10
    System.out.println(st.query(1, 3)); // Now 2+10+4 = 16
    }

}

```
https://en.algorithmica.org/hpc/data-structures/segment-trees/
