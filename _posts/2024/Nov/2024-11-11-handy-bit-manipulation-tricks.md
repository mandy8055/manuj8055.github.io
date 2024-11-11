---
title: Handy Bit Manipulation Tricks
tags: [Algorithms, Bit manipulation]
style: fill
color: warning
description: Common bit manipulation tricks that are worth keeping in your toolbox.
comments: true
---

<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

---

As a developer, understanding bit manipulation can be extremely powerful when solving certain types of problems. Here are some common bit manipulation tricks that are worth keeping in your toolbox.

## Basic Bit Operations

Let's start with the fundamental bit manipulation operations:

### Setting a Bit

To set the bit at position `pos` in the number `n`, we use the bitwise OR operator (`|`) with the left-shifted 1 (i.e., `1 << pos`):

```ts
function setBit(n: number, pos: number): number {
  return n | (1 << pos);
}
```

For example, setting the bit at position 2 in the binary number `0b1010` results in `0b1110`:

```
   0b1010
 | 0b0100
-----------
   0b1110
```

### Clearing a Bit

To clear the bit at position `pos` in the number `n`, we use the bitwise AND (`&`) operator with the inverse of the left-shifted 1 (i.e., `~(1 << pos)`):

```ts
function clearBit(n: number, pos: number): number {
  return n & ~(1 << pos);
}
```

For example, clearing the bit at position 1 in the binary number `0b1110` results in `0b1010`:

```
   0b1110
 & 0b1101
-----------
   0b1010
```

### Toggling a Bit

To toggle the bit at position `pos` in the number `n`, we use the bitwise XOR (`^`) operator with the left-shifted 1 (i.e., `1 << pos`):

```ts
function toggleBit(n: number, pos: number): number {
  return n ^ (1 << pos);
}
```

For example, toggling the bit at position 2 in the binary number `0b1010` results in `0b1110`:

```
   0b1010
 ^ 0b0100
-----------
   0b1110
```

### Checking a Bit

To check if the bit at position `pos` in the number `n` is set, we use the bitwise AND (`&`) operator with 1 after right-shifting `n` by `pos` bits:

```ts
function checkBit(n: number, pos: number): boolean {
  return (n >> pos) & 1;
}
```

For example, checking the bit at position 1 in the binary number `0b1110` results in `true`:

```
   0b1110
 >>     1
-----------
   0b0001
 & 0b0001
-----------
   0b0001 (true)
```

## Bit Manipulation Tricks

Now, let's dive into some more advanced bit manipulation tricks:

### Checking if a Number is a Power of 2

To check if a number `n` is a power of 2, we can use the following trick:

```ts
function isPowerOfTwo(n: number): boolean {
  return n > 0 && (n & (n - 1)) == 0;
}
```

The idea is that a number is a power of 2 if and only if it has exactly one bit set. Subtracting 1 from the number will clear the rightmost set bit, and the result of `n & (n - 1)` will be 0 if and only if `n` was a power of 2.

Here's a visual representation:

```
n = 8 (0b1000)
n - 1 = 7 (0b0111)
n & (n - 1) = 0b1000 & 0b0111 = 0b0000
```

### Counting the Number of Set Bits (Brian Kernighan's Algorithm)

To count the number of set bits in the binary representation of a number `n`, we can use the following algorithm:

```ts
function countSetBits(n: number): number {
  let count = 0;

  while (n) {
    n = n & (n - 1);

    count++;
  }

  return count;
}
```

The idea is to repeatedly clear the rightmost set bit of the number until the number becomes 0. Each time we clear a bit, we increment the count.

Here's a visual representation:

```
n = 0b1011
n - 1 = 0b1010
n & (n - 1) = 0b1011 & 0b1010 = 0b1010
count = 1

n = 0b1010
n - 1 = 0b1001
n & (n - 1) = 0b1010 & 0b1001 = 0b1000
count = 2

n = 0b1000
n - 1 = 0b0111
n & (n - 1) = 0b1000 & 0b0111 = 0b0000
count = 3
```

### Finding the Rightmost Set Bit

To find the rightmost set bit in a number `n`, we can use the following trick:

```ts
function rightmostSetBit(n: number): number {
  return n & -n;
}
```

The idea is that `-n` is the two's complement of `n`, which has all the bits set except the rightmost set bit in `n`. By performing the bitwise AND (`&`) operation, we get the rightmost set bit.

Here's a visual representation:

```
n = 0b1010
-n = 0b0110
n & -n = 0b1010 & 0b0110 = 0b0010

```

### Finding the Rightmost Different Bit of Two Numbers

To find the rightmost different bit between two numbers `m` and `n`, we can use the following trick:

```ts
function rightmostDifferentBit(m: number, n: number): number {
  return rightmostSetBit(m ^ n);
}
```

The idea is to first find the bitwise XOR (`^`) of `m` and `n`, which will set the bits that are different between the two numbers. Then, we can use the "Finding the Rightmost Set Bit" trick to get the rightmost different bit.

Here's a visual representation:

```
m = 0b1010
n = 0b1100
m ^ n = 0b0110
rightmostSetBit(0b0110) = 0b0010
```
