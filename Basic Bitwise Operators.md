# Bitwise-operators-complete-guide-
Introduction
Let's learn bitwise operations that are useful in Competitive Programming. Prerequisite is knowing the binary system. For example, the following must be clear for you already.

13=1⋅8+1⋅4+0⋅2+1⋅1=1101(2)=00001101(2)
Keep in mind that we can pad a number with leading zeros to get the length equal to the size of our type size. For example, char has 8 bits and int has 32.

Bitwise AND, OR, XOR
You likely already know basic logical operations like AND and OR. Using if(condition1 && condition2) checks if both conditions are true, while OR (c1 || c2) requires at least one condition to be true.

Same can be done bit-per-bit with whole numbers, and it's called bitwise operations. You must know bitwise AND, OR and XOR, typed respectively as & | ^, each with just a single character. XOR of two bits is 1 when exactly one of those two bits is 1 (so, XOR corresponds to != operator on bits). There's also NOT but you won't use it often. Everything is explained in Wikipedia but here's an example for bitwise AND. It shows that 53 & 28 is equal to 20.

53 = 110101
28 = 11100

  110101
&  11100  // imagine padding a shorter number with leading zeros to get the same length
 -------
  010100  =  20
  
  _________________**************************************************________________________________________________________________________
C++ code for experimenting
#include <bits/stdc++.h>
using namespace std;

string to_binary(int x) {
	string s;
	while(x > 0) {
		s += (x % 2 ? '1' : '0');
		x /= 2;
	}
	reverse(s.begin(), s.end());
	return s;
}

int main() {
	cout << "13 = " << to_binary(13) << endl; // 1101
	
	int x = 53;
	int y = 28;
	cout << "x = " << to_binary(x) << ", y = " << to_binary(y) << endl;
	
	cout << "AND, OR, XOR:" << endl;
	cout << to_binary(x & y) << " " << to_binary(x | y) << " " << to_binary(x ^ y) << endl;
}
_______________________________________*****************************************************_______________________________________________________

In comments, Rezwan.Arefin01 suggested a simpler way to print a number in binary format. cout << bitset<8>(x); prints a number after converting it into a bitset, which can be printed. There will be more info about bitsets in part 2.

Shifts
There are also bitwise shifts << and >>, not having anything to do with operators used with cin and cout.

As the arrows suggest, the left shift << shifts bits to the left, increasing the value of the number. Here's what happens with 13 << 2 — a number 13 shifted by 2 to the left.

    LEFT SHIFT                             RIGHT SHIFT
       13 =     1101                          13 =   1101
(13 << 2) =   110100                   (13 >> 2) =     11   
If there is no overflow, an expression x << b is equal to x⋅2b, like here we had (13 << 2) = 52.

Similarly, the right shift >> shifts bits to the right and some bits might disappear this way, like bits 01 in the example above. An expression x >> b is equal to the floor of x2b. It's more complicated for negative numbers but we won't discuss it.

So what can we do?
2k is just 1 << k or 1LL << k if you need long longs. Such a number has binary representation like 10000 and its AND with any number x can have at most one bit on (one bit equal to 1). This way we can check if some bit is on in number x. The following code finds ones in the binary representation of x, assuming that x∈[0,109]:

for(int i = 0; i < 30; i++) if((x & (1 << i)) != 0) cout << i << " ";

(we don't have to check i=30 because 230>x)

And let's do that slightly better, stopping for too big bits, and using the fact that if(value) checks if value is non-zero in C++.

for(int i = 0; (1 << i) <= x; i++) if(x & (1 << i)) cout << i << " ";

Consider this problem: You are given N≤20 numbers, each up to 109. Is there a subset with sum equal to given goal S?

It can be solved with recursion but there's a very elegant iterative approach that iterates over every number x from 0 to 2n−1 and considers x to be a binary number of length n, where bit 1 means taking a number and bit 0 is not taking. Understanding this is crucial to solve any harder problems with bitwise operations. Analyze the following code and then try to write it yourself from scratch without looking at mine.

solution code
___________________________________________*****************************************************____________________________________
for(int mask = 0; mask < (1 << n); mask++) {
	long long sum_of_this_subset = 0;
	for(int i = 0; i < n; i++) {
		if(mask & (1 << i)) {
			sum_of_this_subset += a[i];
		}
	}
	if(sum_of_this_subset == S) {
		puts("YES");
		return 0;
	}
}
puts("NO");

______________________________________*****************************************************_____________________________________________________
Two easy problems where you can practice iterating over all 2N possibilities:
- https://codeforces.com/problemset/problem/1097/B
- https://codeforces.com/problemset/problem/550/B

Speed
Time complexity of every bitwise operation is O(1). These operations are very very fast (well, popcount is just fast) and doing 109 of them might fit in 1 second. You will later learn about bitsets which often produce complexity like O(n232), good enough to pass constraints n≤105.
