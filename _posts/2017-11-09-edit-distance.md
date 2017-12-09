---
layout: post
title: Edit Distance
categories: Algorithm
---

<h1>Problem</h1>
<p>Given two strings x and y, What is the cheapest possible way to convert x into y where following operations are allowed to perform &#8211;</p>
<ol>
<li>Substitute a character c of x with c&#8217;.</li>
<li>Insert a character in x.</li>
<li>Delete a character from x.</li>
</ol>
<p>Each of these operations may have a cost associated with them. For example we may have a 26*26 matrix which can have a cost of substituting one character with another character. In a similar manner we may have a 26*2 matrix which can have cost of inserting and deleting a character.</p>
<p><span id="more-1729"></span></p>
<p>For the sake of simplicity we would consider the cost associated with all the operations to be one. However one can easily implement the more general case with just small tweaks in the code presented here.</p>
<p>Link to problem on SPOJ &#8211; Edit Distance</p>
<h1>Motivation</h1>
<p>Before delving right into the problem let&#8217;s see some practical application of edit distance problem &#8211;</p>
<ol>
<li>In phone book whenever we search for a contact with name then it suggests a list of possible contacts. This is done by calculating the minimum cost of the entered string with all the contacts present in phone book and the ones having lowest minimum cost are presented to user ( Beginners often think of using trie for solving this problem but that is wrong !!! ).</li>
<li>In DNA the mutation of C -&gt; G is more likely than the mutation of C -&gt; A. So we give the mutation of C -&gt; G low-cost and mutation of C -&gt; A high-cost. Then we calculate the minimum cost to transform one DNA strand to other using edit distance. This gives us an idea how much the two DNA strands are related evolutionarily.</li>
<li>Apart from the above two the most common application of edit distance is in spelling correction where we calculate the minimum number of edits with respect to all the valid dictionary words and the ones with the least number of edits are suggested to user.</li>
</ol>
<p>Now let&#8217;s see the implementation details.</p>
<h1>Problem Analysis</h1>
<p>Let&#8217;s first figure out the sub problem for this problem.</p>
<h2>Sub Problem</h2>
<p>Suppose we have a suffix string of string x starting from index i denoted by x[i:] and suffix string of string y starting from index j denoted by y[j:]. Then &#8211;</p>
<p>We will compare the characters x[i] and y[j] if both the character are same then answer for this sub problem will be same as answer for sub problem with suffixes x[i + 1:] and y[j + 1:].</p>
<p>Else we will perform these three operations &#8211;</p>
<ul>
<li>Insert the character y[j] into the string x and then cost will be cost of insertion plus the cost to solve the sub problem with suffixes x[i:] and y[j + 1:].</li>
<li>Delete x[i] from string x and then cost will be cost of deletion plus the cost to solve the sub problem with suffixes x[i + 1:] and y[j:].</li>
<li>Substitute x[i] with y[j] and then cost will be cost of substitution plus the cost to solve the sub problem with suffixes x[i + 1:] and y[j + 1:].</li>
</ul>
<p>The sub problem formulation will be more clear with this image &#8211; <img src="https://i0.wp.com/raw.githubusercontent.com/Abhey/Article-Stuff/master/Edit%20Distance(1).png?ssl=1" alt="Sub problem formulation" data-recalc-dims="1" /></p>
<h1>Implementation</h1>
<p>Now let&#8217;s see implementation detail of this problem.</p>
<h2>Naive Implementation</h2>
<p>In naive implementation we will recursively solve each and every sub problem following the recurrence relation that we defined in the previous section.</p>
<h3>Code</h3>
<p>The code for naive implementation is pretty straight forward.</p>
{% highlight cpp %}
#include <bits/stdc++.h>
using namespace std;

int len1,len2;

int solveEditDistance(string str1, string str2, int i, int j){

	// If str1 == "" then all we can do is insertion .........
	if(i == len1)
		return len2 - j;

	// If str2 == "" then all we can do is deletion .........
	if(j == len2)
		return len1 - i;

	// Two end characters are same hence, there is no need for substitution, insertion or deletion.
	if(str1[i] == str2[j])
		return solveEditDistance(str1,str2,i + 1,j + 1);

	else{

		// Time we choose between substitution, insertion and deletion ..........
		int substitutionCost = 1 + solveEditDistance(str1,str2,i + 1,j + 1);
		int insertionCost = 1 + solveEditDistance(str1,str2,i,j + 1);
		int deletionCost = 1 + solveEditDistance(str1,str2,i + 1,j);

		return min(substitutionCost,min(insertionCost,deletionCost));

	}

}

int main(){

	string str1, str2;
	cin >> str1 >> str2;

	len1 = str1.size();
	len2 = str2.size();

	cout << solveEditDistance(str1,str2,0,0) << endl;

}
{% endhighlight %}
<h3>Complexity</h3>
<p>In this naive approach we are solving 3 sub problems for each sub problem irrespective of the fact that we have solved that sub problem previously. So in the worst case we may end up doing O(3^n) operations which is exponential complexity. This worst case occurs when there is no matching character in both the strings. So from here we get an idea that if we save results of each computation then we may end up with a more efficient solution. This is exactly what the dynamic programming solution does.</p>
<h2>Dynamic Programming Implementation</h2>
<p>The recursive dynamic programming implementation of this problem is almost similar to the naive solution, all we have to do is to memoize the result of each sub problem solved before returning it and we are done.</p>
<p>Let&#8217;s talk about iterative implementation. In iterative version we will first create a dp matrix of size (|x| + 1)*(|y| + 1), Where |x| and |y| represents size of string x and y. Now each row will represent one character of string x and the last row will represent empty character. Similarly each column will represent one character of string y and the last character will represent empty character.At any point dp[i][j] will represent the minimum cost to transform the string suffix string of x starting from i into suffix string of y starting from j.</p>
<p>Now we will start iterating from last column of last row. If we are in last row then the cost will be cost of insertion (Because there is no character in x) and if we are in last column then the cost will be cost of deletion (Because there is no character in y). Otherwise we will find the cost according to the model that we formulated earlier.</p>
<p>The figure below shows how the iterative implementation will fill dp matrix for string FOOD &amp; MONEY &#8211;<br />
<img src="https://i0.wp.com/raw.githubusercontent.com/Abhey/Article-Stuff/master/Edit%20Distance(2).png?ssl=1" alt="Iterative implementation" data-recalc-dims="1" /></p>
<h3>Code</h3>
<p>Although the logic might seem difficult at first sight, but the code is pretty easy to understand.</p>
{% highlight cpp %}
#include <bits/stdc++.h>
using namespace std;

int solveEditDistance(string str1, string str2){

	int len1 = str1.size();
	int len2 = str2.size();


	int dp[len1 + 1][len2 + 1];

	for(int i = len1; i >= 0 ; i --){

		for(int j = len2; j>= 0 ; j --){

			if(i == len1){

				// If str1 == "" then all we can do is insertion .........
				dp[i][j] = len2 - j;

			}

			else if(j == len2){

				// If str2 == "" then all we can do is delete .........
				dp[i][j] = len1 - i;

			}

			else if(str1[i] == str2[j]){

				// Two end characters are same hence, there is no need for substitution, insertion or deletion.
				dp[i][j] = dp[i + 1][j + 1];

			}

			else{

				// Time we choose between substitution, insertion and deletion ..........
				// We directly jump to dp[i + 1][j + 1] in case of substitution.
				// We jump to dp[i + 1][j] in case of deletion.
				// We jump to dp[i][j + 1] in case of insertion.
				dp[i][j] = 1 + min(dp[i + 1][j + 1],min(dp[i + 1][j],dp[i][j + 1]));

			}

		}

	}

	return dp[0][0];

}

int main(){

	string str1, str2;
	cin >> str1 >> str2;

	cout << solveEditDistance(str1,str2) << endl;

}
{% endhighlight %}
<h3>Complexity</h3>
<p>Total number of sub problems = (total number of suffixes of string x) * (total number of suffixes in string y)<br />
Total number of suffixes in string x = |x| (Size of string x), Similarly for string y total number of suffixes = |y| (Size of string y)<br />
So, total number of sub problems = |x|*|y|<br />
Also the time required to solve one sub problem given we know solutions to rest other sub problem = O(1)<br />
So, overall complexity = |x|*|y|*O(1) = O(|x|*|y|)</p>
<h1>Further Reading</h1>
<p>In this article we have discussed the suffix based sub problem implementation, the problem can also be solved via prefix based sub problem implementation. You may refer to this article for prefix based sub problem implementation.</p>
<p>Another problem that can be solved via slight modification to this problem is that of Longest Common Subsequence. For further reading on LCS refer <a title="Printing Longest Common Subsequence" href="http://www.gohired.in/2014/04/25/printing-longest-common-subsequence/" target="_blank" rel="nofollow noopener">this article</a>.</p>