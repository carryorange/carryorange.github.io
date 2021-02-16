# A compilation of stock buy/sell questions
## [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
### Overview
Only allow at most one trade throughout the given period. Get the max profit.

### Algorithm
Find two indices `left < right`, where `price[right] - price[left]` is maximized.

So need to keep track of min price seen so far, and update max profit using min price as a reference.


## [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii)
### Overview
Allow any number of trades throughout the given period. Get the max profit.

### Algorithm
1. Only need to compare one day with previous day, and if it's a gain, collect it.
2. Accumulate the gains between two days.


## [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
### Overview
Allow _at most_ two transactions.

### Algorithm
__Dynamic Programming__


## [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
### Overview
Allow _at most_ `k` transactions.

### Algorithm
__Dynamic Programming__