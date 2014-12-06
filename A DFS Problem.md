在蓝桥杯校选拔赛（都不好意思说出口）上遇到一个水题，当时是纯用手算的，后来发现其实就是一个简单的深搜，和数独的思路差不多（但我当时还不会呢，原谅我刚开始学算法，都大三了），我只能通过这种闲暇的机会学一些简单的算法，惭愧

题目大概是这样的，两组1-7数字进行排列，要求相同数字之间数的个数等于该数字，已知前两个数字是7、4，求符合条件的排列。

话不多说，直接上代码

    #include <iostream>
	#include <string.h>
	
	using namespace std;

	int a[14];
	//判断某个数字是否使用过
	int f[8];
	//结束标志
	bool sign = false;
	//深度为i的位置
	void DFS(int i);
	int main()
	{
	    memset(a, 0, sizeof(a));
	    memset(f, 0, sizeof(f));
	    a[0] = a[8] = 7;
	    a[1] = a[6] = 4;
	    f[4] = f[7] = 1;
	    DFS(2);
	    //输出
	    for(int i = 0; i < 14; i++) {
	        cout<<a[i];
	    }
	    return 0;
	}
	void DFS(int i) {
	    //如果i等于14，将sign置为true
	    if(i == 14) {
	        sign = true;
	        return ;
	    }
	    //如果深度为i的位置有数字，直接跳过,其实这里的if else很重要
	    if(a[i] != 0) {
	        DFS(i + 1);
	    }
	    else {
	        for(int j = 1; j < 8 ; j++) {
	            //判断j是否有被使用过
	            if(f[j] == 0) {
	                //判断深度为i的位置填j是否可行，(i+j+1) < 14这个条件容易忽略，虽然有时不写也行
	                if((i+j+1) < 14 && a[i+j+1] == 0) {
	                    a[i] = a[i+j+1] = j;
	                    f[j] = 1;
	                    //继续搜下一层
	                    DFS(i+1);
	                    //如果已结束，直接返回
	                    if(sign == true) return ;
	                    //否则，将原先的状态复原
	                    a[i] = a[i+j+1] = 0;
	                    f[j] = 0;
	                }
	            }
	        }
	    }
	}
