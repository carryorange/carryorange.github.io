# Bit Manipulation Tricks

## Bit manipulations
* xor
  * Basic: 1^1=0, 1^0=1, 0^1=1, 0^0=0
  * p^q = (p|q)&(!(p&q))
  * a^a = a&(!a) = 0
  * a^0 = a
* First least significant one bit in `i`: (i & (-i))