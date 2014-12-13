道听途说了一道Google面试题，a[m]数组和b[n]数组笛卡尔乘积（即a[i]*b[j],0<=i<m,o<=j<n)，求第k大数（即排完序之后的第k个数）。

分析：数组笛卡尔乘积的Range为a[0]*b[0] ~ a[m-1]*b[n-1], 然后进行二分查找。第k大数满足：

1. 小于等于它的个数大于等于k。
2. 它在满足条件1的所有数中最小。

此算法的时间复杂度为：O((m+n)*log(Range))。

代码：

    #include <iostream>
	#include <algorithm>
	using namespace std;
	int m,n,k;
	//求小于v的数的个数
	int getLessNum(int m, int *a, int *b);
	int main()
	{
	    cin>>m>>n>>k;
	    int *a = new int[m];
	    int *b = new int[n];
	    for(int i = 0; i < m; i++) {
	        cin>>a[i];
	    }
	    for(int i = 0; i < n; i++) {
	        cin>>b[i];
	    }
	    sort(a, a + m);
	    sort(b, a + n);
	    //计算Range
	    int mMin = a[0] * b[0];
	    int mMax = a[m - 1] * b[n -1];
	    int mMid = (mMin + mMax) / 2;
	    while(mMin < mMax - 1) {
	        if(getLessNum(mMid, a, b) >= k) {
	            mMax = mMid;
	        } else {
	            mMin = mMid;
	        }
	        mMid = (mMin + mMax) / 2;
	    }
	    if(getLessNum(mMid, a, b) >= k) {
	        cout<<mMin;
	    } else {
	        cout<<mMax;
	    }
	    return 0;
	
	}
	//求小于等于v的个数的最坏时间复杂度是O(m+n),注意这里的小技巧
	//一个细节是从左到右、从上到下是分别递增的
	int getLessNum(int v, int *a, int *b) {
	    int num = 0;
	    //将行初始化为0，列初始化为n - 1;
	    int i = 0;
	    int j = n - 1;
	    for( ; i < m; i++) {
	        while(a[i] * b[j] > v) {
	            j--;
	            //如果j < 0,则往下不可能再有小于等于v的数了，直接返回num
	            if(j < 0) return num;
	        }
	        num += j + 1;
	    }
	    return num;
	
	}

