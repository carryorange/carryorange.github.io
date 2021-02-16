# Miscellany

## String algorithms
### [KMP algorithm](https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/) for pattern searching
- Context: Given a string `s` of length `m` and a pattern `p` of length `n` (assuming `m` > `n`), return the starting indices of substrings in `s` that matches `p`.
  * Naive pattern search doesn't work well in cases where we see many matching characters followed by a mismatching one. KMP addresses this.
    * e.g. s="AAAAAAAAAAAAAAAAAAAAAAAAB", p="AAAAB"
- Complexity
  * Naive matching: worst case O(mn)
  * KMP: worst case O(m+n)
    * KMP achieves this with O(n) preprocessing
- Algorithm
  * Preprocessing
    * Construct longest **proper prefix** which is also suffix array (`lps`)
      * Proper prefix: prefix with whole string **not** allowed. (e.g. "", "A" in "AB")
    * `lps[i]` definition:
      * `lps[i]` is the largest integer smaller than i s.t `p[0..lps[i]]` is a suffix of `p[0..i]`

