今天介绍一个经典的组合数学问题：错排。

考虑一个有n个元素的排列，若一个排列中所有的元素都不在自己原来的位置上，那么这样的排列就称为原排列的一个错排。首先，错排是相对于一个固定的排列而言的，其次，错排的个数却只与这个固定排列的长度有关，而与这个固定排列的顺序无关。

错排的递推公式：Dn=(n-1)(Dn-1+Dn-2)。有了这个递推公式，直接用最简单的动态规划实现。时间复杂度为O(n)。

    #include <iostream>
	using namespace std;
	int d[10005];
	int main()
	{
	    d[1] = 0;
	    d[2] = 1;
	    int n;
	    cin>>n;
	    for(int i = 3; i <= n; i++) {
	        d[i] = (i - 1) * (d[i-1] + d[i-2]);
	    }
	    cout<<d[n];
	
	}