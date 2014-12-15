Fibonacci数列相信大家都很熟悉，比较简单的方法是使用递推或递归，递推由于用到了动态规划的思想，时间复杂度为O(n)，比递归快很多。这里就不再细说了。

今天要介绍的是用矩阵来求解Fibonacci数列。设A = [1 1]（第一列）[1 0]（第二列）,则：

1. [f(n) f(n-1)]（第一列） = A^(n-2) * [f(2) f(1)]（第一列）
2. [f(n+1) f(n)]（第一列）[f(n) f(n-1)]（第二列） = A^n

接下来要解决的问题是如何计算矩阵的n次方，有两种方法：

1. 二分法。设i=n/2，若n%2==1,A^n = A^i*A^i*A，若n%2==0,A^n=A^i*A^i。
2. 矩阵快速幂。将n拆分为二进制数。

这里上一些关键代码（矩阵A为上述矩阵，矩阵F初始为单位矩阵）：

矩阵结构体：

    struct Matrix 
	{
	    int row;
	    int col;
	    int m_map[2][2];
	};

矩阵乘法：

    Matrix operator*(const Matrix& M1,const Matrix& M2)
	{
	    Matrix M;
	    M.row = M.col = 2;
	    for(int i = 0; i < M1.row; i++) {
	        for(int j = 0; j < M2.col; j++) {
	            M.m_map[i][j] = 0;
	            for(int k = 0; k < M1.col; k++) {
	                M.m_map[i][j] += M1.m_map[i][k] * M2.m_map[k][j];
	            }
	            M.m_map[i][j] %= 10007;
	        }
	    }
	    return M;
	}

二分递归求矩阵次方：

    Matrix pow(int n) {
	    if(n == 1) {
	        return A;
	    }
	    int i = n / 2;
	    if(n % 2 == 0) {
	        return pow(i) * pow(i);
	    } else {
	        return pow(i) * pow(i) * A;
	    }
	}

矩阵快速幂求矩阵次方：
	
	#define Bit(n) 1<<n
    int pow(int n) {
		Matrix temp = A;
	    for(int i = 0; Bit(i) <= n; i++) {
	        if(Bit(i) & n) F = F * temp;
	        temp = temp * temp;
	    }
	    return F.m_map[0][1] % 10007;
	}

经过测试，发现矩阵快速幂求解比二分求解快很多。