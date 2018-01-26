# Bitwise operators
##### How do they work in general?
In computers, numbers are represented with bits, which are series of ones and zeros. In a base two system, each place in a number can hold one of two values, 0 or 1. Bitwise operators do operations on these bits.

Most programming languages let you write binary numbers indicated by the `0b` notation (e.g. `0b1100`, or `0x` for hex). In JavaScript you need to use `parseInt("00000011", 2)` inputting a string representation of the binary number, and the base you're converting from.

Built-in conversion in Ruby:
```ruby
# Binary string to decimal integer
"111".to_i(2) # => 7
0b00000111 # => 7

# Decimal integer to string in base 2 / binary
7.to_s(2) # => "111"
```
In JavaScript:
```JavaScript
// Binary string to decimal integer
parseInt("00000111", 2) // => 7

// Decimal integer to binary string
7..toString(2) // => "111"
```

Very basic introduction: [Codecademy bitwise operator exercises](https://www.codecademy.com/courses/introduction-to-bitwise-operators)

## & AND
Bitwise `AND` compares two binary numbers and returns another binary number with the corresponding bit place set to 1 if both bit places of the two compare numbers are 1, otherwise the corresponding bit place is 0. Inputs are decimal numbers, and output is also a decimal number.

E.g. 12 & 1 returns 0:
```
  1100  12
& 0001  1
------
= 0000  0
```

3 & 1 returns 1:  
```
  0011  3
& 0001  1
------
= 0001  1
```

3 & 3 returns 3:  
```
  0011  3
& 0011  3
------
= 0011  3
```

Using the AND operator can only result in a number less tan or equal to the larger of the two inputs.

#### How is this useful?
To access certain bits! AND can be used with a bit mask to selectively pick out bits. For example, use `a & mask` with `mask = 7` (or `0b111`), to retain only the last 3 bits, or with `mask = 1` to only access the last bit.

In the **Bit Count example** below, we use `count += num & 1` to increment the count when we see a 1 bit in the one's place (instead of a 0 bit, which increments the count by zero). The `& 1` acts as a mask to catch only the last bit. We then right shift by one `num >>= 1` to shift the next bit into the one's place to look at. We do this until the number is 0, i.e. no 1 bits are left due to shifting.

```ruby
def bit_count(num)
  count = 0
  while num > 0 do
    count += num & 1
    count >>= 1
  end
  count
end
```

## | OR
Bitwise 'OR' returns 1 if either bit is 1, otherwise it returns 0.
The bitwise OR (a|b) operator compares two numbers on a bit level and returns a number where the bits of that number are turned on if either of the corresponding bits of a and b are 1.

E.g. 9 | 4 returns 13:  
```
  1001  9
| 0100  4
------
= 1101  13
```

Using OR can only create results that are greater than or equal to the larger of the two integer inputs.

#### How is it useful?
To turn on certain bits!
`80 | 7 = 87` or `0b1010000 | 0b111 = 1010111`

## ^ XOR
The XOR (a^b) or Exclusive Or operator compares two numbers on a bit level and returns a number where the bits of that number are turned on if either of the corresponding bits of a and b are 1 but not both.

E.g. 5 ^ 4 returns :  
```
  0101  5
| 0100  4
------
= 0001  1
```

If a bit is 0 in both it stays 0 in the result. Note that XOR-ing a number with itself will always result in a 0.

#### How is this useful?
XOR can be used to flip bits in combination with an appropriate mask. E.g. `0b1010 ^ 0b1111 = 0101`

In the **parity computation example** below, we use XOR to keep track of the 1 bits seen in a binary number. The goal is to know if it's an even or odd number od 1 bits in the number. Starting with result = 0, for each 1 bit we XOR with, the result flips from 0 to 1 or vice versa. We flip it once or any odd number of times, it ends up 1 (odd). We flip it two or any even number of times, it ends up 0 (even).

```ruby
# Compute the parity of a binary word.
# Return 0 if number of 1s in the binary equivalent of the number is odd, returns 0 if they're even
# num = 12, or 0b1100 => 0
# num = 7, or 0b111 => 1

def parity(num)
  result = 0
  while num > 0 do
    result ^= num & 1
    num >>= 1
  end
  result
end
```

## ~ NOT
~ NOT flips all of the bits in a single number. Mathematically this is equivalent to adding one to the number and making it negative.

```
~ 1  = -2
~ 2  = -3
~ 42 = -43
```

## >> Right Shift, << Left Shift
The shift operators work by shifting bits over by a designated number of places. The shift number is written as a decimal integer, and bitwise shifts only work on integers, not flaots or strings. Every shift place essentially floor divides or multiplies the decimal number by 2.

**Left bit shift:**
```
00000001 << 2 = 00000100
       1 << 2 = 4
// This is equivalent to 1 * (2^2)
```

```
00010001 << 3 = 10001000  
      17 << 3 = 136  
// This is equivalent to 17 * (2^3)
```

Generalized: `a << b = a * (2^b)`  
- Left shifting by 1, for example, is like multiplying by `2^1 = 2`.  
- Left shifting by 2 is like multiplying by `2^2 = 4`.  
- Left shifting by 3 is like multiplying by `2^3 = 8`.

**Right bit shift:**  
Right bit shifting works the same, but reduces the number a divided by 2^b.
Generalized: `a >> b = a / (2^b)`

#### How is this useful?
The `>>` and `<<` operators with a mask can help us in getting a value in a particular part of a number. We shift it some places, usually so that the places we want to access are at the right end, and the access their values by & masking them.
