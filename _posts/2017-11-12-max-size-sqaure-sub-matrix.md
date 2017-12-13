---
layout: post
title:  Maximum size of square sub matrix with all 1’s in a binary matrix
categories: Algorithm
---

<h1>Problem</h1>
<p>Consider a binary matrix as shown in the figure below. You need to find the maximum size of square sub matrix with all 1&#8217;s.</p>
<p align="center"><img src="https://i0.wp.com/raw.githubusercontent.com/Abhey/Article-Stuff/master/MaxSizeSubMatrix(1).png?ssl=1" alt="Sample problem" data-recalc-dims="1" /></p>
<p><span id="more-1762"></span></p>
<h1>Naive Implementation</h1>
<p>The naive solution for this problem will be to consider each and every sub matrix and then check if it is max one or not.</p>
<h2>Code</h2>
<p>The code for naive solution is pretty straight forward.</p>
{% highlight cpp %}
#include <bits/stdc++.h>
using namespace std;

int findMaxOnesMatrixSize(vector<vector<int> > &matrix){

	int n = matrix.size();
	int m = matrix[0].size();

	int maxOnesMatrixSize = 0;

	for(int a = 1 ; a <= max(m,n) ; a ++){

		for(int i = 0 ; i < n ; i ++)
			for(int j = 0 ; j < m ; j ++){

				int temp = 1;

				if(i + a > n || j + a > m)
					continue;

				for(int x = i ; x < i + a; x ++)
					for(int y = j ; y < j + a ; y ++)
						if(matrix[x][y] == 0){
							temp = 0;
							break;
						}

				if(temp)
					maxOnesMatrixSize = max(maxOnesMatrixSize,a);

			}

	}

	return maxOnesMatrixSize;

}

// Driver function .............
int main(){

	int test;
	cin >> test;

	while(test --){

		int n,m;
		cin >> n >> m;

		vector<vector<int> > matrix(n,vector<int> (m));

		for(int i = 0 ; i < n ; i ++)
			for(int j = 0 ; j < m ; j ++)
				cin >> matrix[i][j];

		cout << findMaxOnesMatrixSize(matrix) << endl;
	
	}

}
{% endhighlight %}
<h2>Complexity</h2>
<p>For a matrix of order m * n, Let c = min(m,n). Then the overall complexity of naive solution will be O(c<sup>3</sup>mn).</p>
<h1>Dynamic Programming Implementation</h1>
<p>Let&#8217;s make some observations about this problem before delving right into implementation details. Consider a square sub matrix of order n. Then we can consider this sub matrix as a combination of three square sub matrices of order n &#8211; 1 and a single cell, As shown in figure below &#8211;</p>
<p><img src="https://i2.wp.com/raw.githubusercontent.com/Abhey/Article-Stuff/master/MaxSizeSubMatrix(2).png?ssl=1" alt="Breakdown of sub matrix into smaller sub matrices" data-recalc-dims="1" /></p>
<p>Now if all these three sub matrices of order n &#8211; 1 and the single cell are filled with 1&#8217;s, Then the bigger square sub matrix of order n will also be filled with 1&#8217;s. Let&#8217;s consider that dp[i][j] represents the maximum size of sub matrix with all 1&#8217;s ending at cell (i,j). Then from the above discussion it becomes pretty much intuitive that if, the (i,j) cell contains 1 then,</p>
<pre>dp[i][j] = 1 + min(dp[i - 1][j],d[i][j - 1],dp[i - 1][j - 1])</pre>
<p>So our overall recurrence relation looks somewhat like this &#8211;</p>
<pre class="brush: cpp; title: ; notranslate" title="">
            0                                                       matrix[i][j] = 0
dp[i][j] =  
            1 + min(dp[i - 1][j],d[i][j - 1],dp[i - 1][j - 1])      matrix[i][j] = 1
</pre>
<p>Now, with the recurrence relation in our hand all we need to do is to create a dp matrix of size m * n (Same as the order of given matrix). We will then start iterating from first column of first row and will fill the dp matrix according to the recurrence relation established above. Finally we will again iterate over the dp matrix to find the max value present in the matrix. A much more efficient solution will be to compute the max value right at the time of filling dp matrix.</p>
<h2>Code</h2>
<p>The above discussion becomes more clear with the code below &#8211;</p>
{% highlight cpp %}
#include <bits/stdc++.h>
using namespace std;

int findMaxOnesMatrixSize(vector<vector<int> > &matrix){

	int n = matrix.size();
	int m = matrix[0].size();

	// Initializing dp matrix with 0 ........
	vector<vector<int> > dp(n, vector<int>(m,0));
	int maxOnesMatrixSize = 0;

	for(int i = 0 ; i < n ; i ++){

		for(int j = 0 ; j < m ; j ++){

			if(matrix[i][j] == 0){

				// If the current block has zero no need to do any computation ...........
				continue;

			}

			int upperMatrixSize = 0, diagonalMatrixSize = 0, sideMatrixSize = 0;

			if(i > 0){

				// Updating upper matrix size ............
				upperMatrixSize = dp[i - 1][j];

			}

			if(j > 0){

				// Updating side matrix size ............
				sideMatrixSize = dp[i][j - 1];

			}

			if(i > 0 && j > 0){

				// Updating diagonal matrix size ...........
				diagonalMatrixSize = dp[i - 1][j - 1];

			}

			dp[i][j] = 1 + min(diagonalMatrixSize,min(upperMatrixSize,sideMatrixSize));
			maxOnesMatrixSize = max(maxOnesMatrixSize,dp[i][j]);

		}

	}

	return maxOnesMatrixSize;

}

// Driver function ...........
int main(){

	int n,m;
	cin >> n >> m;

	vector<vector<int> > matrix(n,vector<int> (m));

	for(int i = 0 ; i < n ; i ++)
		for(int j = 0 ; j < m ; j ++)
			cin >> matrix[i][j];

	cout << findMaxOnesMatrixSize(matrix) << endl;

}
{% endhighlight %}
<h2>Complexity</h2>
<p>In the dynamic programming implementation we iterate m * n times to fill dp matrix. Hence the overall complexity of this solution is O(m*n).</p>