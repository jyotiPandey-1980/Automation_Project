## Least Number of Perfect Squares (numSquares)

### Problem
Given an integer `n`, return the least number of perfect square numbers that sum to `n`.

### Java Implementation
```java
public class Solution {
    public int numSquares(int n) {
        if (n <= 0) return 0;

        // Remove factors of 4
        while ((n & 3) == 0) n >>= 2;

        // If n ≡ 7 (mod 8), answer is 4
        if ((n & 7) == 7) return 4;

        // If n is a perfect square, answer is 1
        if (isSquare(n)) return 1;

        // Check if sum of two squares => answer is 2
        for (int i = 1; i * i <= n; i++) {
            if (isSquare(n - i * i)) return 2;
        }

        // Otherwise, answer is 3
        return 3;
    }

    private boolean isSquare(int x) {
        int r = (int) Math.sqrt(x);
        return r * r == x;
    }
}
```

### Approach Overview
- **Reduce by powers of 4**: If `n` is divisible by 4, repeatedly divide by 4 without changing the answer.
- **8k + 7 check**: If the reduced `n` satisfies `n ≡ 7 (mod 8)`, by Legendre’s three-square theorem the answer is **4**.
- **Perfect square check**: If `n` is a perfect square, answer is **1**.
- **Sum of two squares**: Check if `n = a^2 + b^2` for some `a, b`. If yes, answer is **2**.
- **Otherwise**: By the theorems above, answer is **3**.

### Why This Works
- **Lagrange’s Four-Square Theorem**: Every positive integer is the sum of four or fewer squares.
- **Legendre’s Three-Square Theorem**: A number is a sum of three squares iff it’s not of the form `4^a(8b + 7)`.
  - After removing all factors of 4, if the remainder is `7 mod 8`, we need **4** squares.
  - If not, and it’s not 1 or 2 squares, it must be **3**.

### Complexity
- Removing powers of 4 and modular checks are O(1).
- Two-square check loops over `i ≤ sqrt(n)` and runs `isSquare` in O(1). 
- **Time**: O(sqrt(n))
- **Space**: O(1)

### Usage
```java
public class Main {
    public static void main(String[] args) {
        Solution s = new Solution();
        System.out.println(s.numSquares(12)); // 3 -> 4 + 4 + 4
        System.out.println(s.numSquares(13)); // 2 -> 4 + 9
        System.out.println(s.numSquares(1));  // 1 -> 1
        System.out.println(s.numSquares(28)); // 4 (8k+7 form after reduction)
    }
}
```

### Edge Cases
- **n ≤ 0**: Returns 0 by convention in this implementation.
- **Large n**: Works efficiently up to standard 32-bit integer limits.

### Alternatives
- **Dynamic programming**: O(n·sqrt(n)) time, O(n) space; simpler but slower for large `n`.
- **BFS on remainders**: Also O(n·sqrt(n)), can be practical but heavier than the number-theory approach.

### Notes
- The bitwise checks `(n & 3) == 0` and `(n & 7) == 7` implement `n % 4 == 0` and `n % 8 == 7` efficiently.
- `isSquare` uses integer comparison to avoid floating-point inaccuracies.