# BIT Manipulation

BIT Manipulation, also known as Bitwise Manipulation, is a technique used in computer programming to perform operations at the bit level. It involves manipulating individual bits of binary numbers to achieve desired results. This technique is commonly used in various areas such as data compression, cryptography, and low-level programming.


## Binary Representation of Decimal Numbers 
Binary representation is a way of expressing numbers using only two digits: 0 and 1. It is a base-2 numeral system, unlike the decimal system which is base-10. In binary, each digit represents a power of 2, starting from the rightmost digit. The rightmost digit represents 2^0 (1), the next digit represents 2^1 (2), the next digit represents 2^2 (4), and so on.

To convert a decimal number to binary in C++, you can use the following code:

```c++
#include <iostream>
#include <vector>

std::vector<int> decimalToBinary(int decimal) {
    std::vector<int> binary;
    
    while (decimal > 0) {
        binary.push_back(decimal % 2);
        decimal /= 2;
    }
    
    // Reverse the binary vector to get the correct binary representation
    std::reverse(binary.begin(), binary.end());
    
    return binary;
}

int main() {
    int decimalNumber;
    std::cout << "Enter a decimal number: ";
    std::cin >> decimalNumber;
    
    std::vector<int> binaryNumber = decimalToBinary(decimalNumber);
    
    std::cout << "Binary representation: ";
    for (int digit : binaryNumber) {
        std::cout << digit;
    }
    
    return 0;
}
```
In this code, the decimalToBinary function takes an integer decimal as input and returns a vector of integers representing the binary representation of the decimal number. It uses a while loop to repeatedly divide the decimal number by 2 and store the remainder (0 or 1) in the binary vector. Finally, the vector is reversed to obtain the correct binary representation.

## Bitwise Operators

Bitwise operators are used to perform bitwise operations on binary numbers. The following are the commonly used bitwise operators:

- `&` (AND): Performs a bitwise AND operation between two numbers, resulting in a number where each bit is set if and only if the corresponding bits of both numbers are set.
- `|` (OR): Performs a bitwise OR operation between two numbers, resulting in a number where each bit is set if at least one of the corresponding bits of the two numbers is set.
- `^` (XOR): Performs a bitwise XOR (exclusive OR) operation between two numbers, resulting in a number where each bit is set if and only if exactly one of the corresponding bits of the two numbers is set.
- `~` (NOT): Performs a bitwise NOT operation on a number, resulting in a number where each bit is inverted (0 becomes 1 and 1 becomes 0).
- `<<` (Left Shift): Shifts the bits of a number to the left by a specified number of positions, effectively multiplying the number by 2 raised to the power of the shift amount.
- `>>` (Right Shift): Shifts the bits of a number to the right by a specified number of positions, effectively dividing the number by 2 raised to the power of the shift amount.

### Usage Example:

```cpp
int a = 5; // Binary: 0101
int b = 3; // Binary: 0011

cout << "a & b: " << (a & b) << endl; // Binary: 0001, Decimal: 1
cout << "a | b: " << (a | b) << endl; // Binary: 0111, Decimal: 7
cout << "a ^ b: " << (a ^ b) << endl; // Binary: 0110, Decimal: 6
```

## 3. Number of Set Bits in a Number and Usage of `__builtin_popcount`

The number of set bits (bits with value 1) in a binary representation of a number is often needed in various algorithms.

### Code Example:

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 9; // Binary: 1001
    int count = __builtin_popcount(number);
    cout << "Number of set bits in " << number << " is: " << count << endl;
    return 0;
}
```

### 4. Position of Rightmost and Leftmost Set Bit

#### Rightmost Set Bit

The rightmost set bit (or the least significant set bit) of a number can be found by isolating it using the expression `n & -n`. Here, `-n` is the two's complement of `n`, which in binary is the negation of `n` plus one. This operation zeroes out all bits except for the least significant set bit.

**Why the code works:**

- The expression `n & -n` isolates the rightmost set bit of `n`. This works because in two's complement, `-n` is obtained by inverting all the bits of `n` and adding 1. Thus, in `n & -n`, all bits to the left of the rightmost set bit in `n` become 0 when ANDed with their inverted selves in `-n`, and the rightmost 1 remains because it ANDs with 1 (due to the +1 in two's complement).
- `log2(n & -n) + 1` calculates the position of this isolated bit by finding its logarithm base 2 (which gives the position of the highest bit set, or in this case, the only bit set) and adds 1 because bit positions start from 1, not 0.

```cpp
int rightmostSetBit(int n) {
    return log2(n & -n) + 1;
}
```

#### Leftmost Set Bit

To find the leftmost set bit (or the most significant set bit) of a number, we can keep shifting the number to the right until it becomes 0. The number of shifts needed gives us the position of the leftmost set bit.

**Why the code works:**

- By continuously shifting `n` to the right (`n = n >> 1`), we essentially move all bits towards the least significant bit position.
- The loop continues until `n` becomes 0, meaning all bits have been shifted out. The number of iterations equals the position of the leftmost set bit because that's how many shifts are required to move the leftmost set bit to the least significant bit position and then out of the number.

```cpp
int leftmostSetBit(int n) {
    int pos = 0;
    while (n) {
        pos++;
        n = n >> 1;
    }
    return pos;
}
```

### 5. Set and Unset kth Bit

#### Setting the kth Bit

Setting the kth bit means changing the kth bit to 1, regardless of its current state.

**Why the code works:**

- `1 << k` creates a number with only the kth bit set (bits are zero-indexed, so the 1st bit is at position 0).
- Using the OR operator `|` with `n` and this number will set the kth bit of `n` to 1. This is because ORing any bit with 1 results in 1, so the kth bit becomes 1, while all other bits of `n` remain unchanged due to ORing with 0s.

```cpp
int setKthBit(int n, int k) {
    return n | (1 << k);
}
```

#### Unsetting the kth Bit

Unsetting the kth bit means changing the kth bit to 0, regardless of its current state.

**Why the code works:**

- `1 << k` again creates a number with only the kth bit set.
- `~(1 << k)` inverts this number, so all bits are set except the kth bit.
- Using the AND operator `&` with `n` and this number will unset the kth bit of `n` to 0. This is because ANDing any bit with 0 results in 0, effectively unsetting the kth bit, while all other bits remain unchanged due to ANDing with 1s.

```cpp
int unsetKthBit(int n, int k) {
    return n & ~(1 << k);
}
```

These methods leverage the basic principles of binary arithmetic and bitwise operations to manipulate specific bits directly, demonstrating the power and efficiency of bitwise operations in low-level programming.


## 6. Subsets Using Bitmask

Bitmasking can be used to generate all possible subsets of a given set efficiently.

### Code Example:

```cpp
#include <iostream>
#include <vector>
using namespace

 std;

void printSubsets(vector<int>& set) {
    int n = set.size();
    for (int i = 0; i < (1 << n); i++) {
        cout << "{ ";
        for (int j = 0; j < n; j++) {
            if (i & (1 << j)) {
                cout << set[j] << " ";
            }
        }
        cout << "}" << endl;
    }
}

int main() {
    vector<int> set = {1, 2, 3};
    printSubsets(set);
    return 0;
}
```

This code iterates through all possible combinations of elements in the set, using a bitmask to determine which elements are included in each subset.

---

Bit manipulation is a powerful technique in computer science for optimizing algorithms and solving problems efficiently. By understanding and leveraging bitwise operators and techniques like bit masking, programmers can write more efficient and effective code for a variety of applications.

