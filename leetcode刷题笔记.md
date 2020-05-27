[toc]
##### A+B问题
[简单](https://www.lintcode.com/problem/a-b-problem/description)

**描述**

> 给出两个整数 aa 和 bb , 求他们的和。（不使用+运算符）

**思路**

> 异或运算(^)，相当于忽略进位的加法
>
> 与运算再左移一位，相当于求两个二进制数相加，是否出现进位

```c
int aplusb(int a, int b) 
{
    if(a==0)  
        return b; 
    if(b==0)  
        return a; 
    int x1 = a^b; 
    int x2 = (a&b)<<1; 
    return aplusb(x1,x2); 
}
```



##### 149. 买卖股票的最佳时机

[中等](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock/description)

**描述**
> 假设有一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。

**样例**
> 输入: [3, 2, 3, 1, 2]  输出: 1
>
> 说明：你可以在第三天买入，第四天卖出，利润是 2 - 1 = 1
>
> 输入: [1, 2, 3, 4, 5] 输出: 4
>
> 说明：你可以在第0天买入，第四天卖出，利润是 5 - 1 = 4

> 输入: [5, 4, 3, 2, 1] 输出: 0
>
> 说明：你可以不进行任何操作然后也得不到任何利润

**思路**
> 定义 buy 和 sell ，buy 为当天买和之前买的最小值，sell 为当天卖和之前卖的最大值，遍历完即可得到最大收益。

```c
int maxProfit(vector<int> &prices) {
    int buy = prices[0];
    int sell = 0;
    
    for(int i=1; i<prices.size(); i++) {
        buy = buy<prices[i]? buy : prices[i];
        sell = sell>(prices[i]-buy)? sell : (prices[i]-buy);
    }
    return sell;
}
```


##### 150. 买卖股票的最佳时机 II
[中等](https://www.lintcode.com/problem/best-time-to-buy-and-sell-stock-ii/description)

**描述**
> 给定一个数组 prices 表示一支股票每天的价格。
你可以完成任意次数的交易, 不过你不能同时参与多个交易 (也就是说, 如果你已经持有这支股票, 在再次购买之前, 你必须先卖掉它)。
> 设计一个算法求出最大的利润.

**样例**
> 输入: [2, 1, 2, 0, 1] 输出: 2
>
> 解释: 
    在第 2 天以 1 的价格买入，然后在第 3 天以 2 的价格卖出，利润 1；在第 4 天以 0 的价格买入, 然后在第 5 天以 1 的价格卖出，利润 1 ； 总利润 2。

**思路**
> 可以用贪心法，只要当天价格高于前一天，就加入到收益之中去

```c
int maxProfit(vector<int> &prices) {
    // write your code here
    int profit = 0;
    int i, diff;
    for(i = 1 ; i < prices.size(); i++)
    {
        diff = prices[i] - prices[i-1];
        if( diff > 0)
            profit += diff;
    }
    return profit;
}
```


##### 1199. 完美的数
[简单](https://www.lintcode.com/problem/perfect-number/description)

描述
> 我们定义完美数是一个正整数，它等于除其自身之外的所有正约数的总和。现在，给定一个整数n，写一个函数，当它是一个完美的数字时返回true，而当它不是时，返回false。

样例
> 输入：28，输出：true；解释：28 = 1 + 2 + 4 + 7 + 14

```c
bool checkPerfectNumber(int num)
{
    if(num <= 1) 
        return false;
    int sum = 1;
    for(int i=2; i*i<=num; ++i)
    {
        if(num%i==0) {
            sum += i;
            if(num / i != i)
                sum += num/i;
        }
    }
    return sum == num;
}
```


##### 1224. 计数重复个数
[困难](https://www.lintcode.com/problem/count-the-repetitions/description)

**复杂度较高**

**描述**
> 定义`S = [s, n]`为 n 个字符串 s 组成的字符串 S ，例如`["abc", 3] = "abcabcabc"`.
>
> 另外，如果字符串 s2 在去掉一些字符后变成了字符串  s1，我们就说s1可以从 s2 中“得到”s1。例如`"abc"`可以从`"abdbec"`中得到，但不能从`"acbbe"`中得到.
>
> 给你两个非空字符串 s1、s2（最多100个字符）以及两个整数 n1、n2，根据 `S1=[s1, n1]` , `S2=[s2, n2]` 得到字符串S1、S2，计算最大的整数 M ，使得 `[S2,M]` 可以从 S1 中得到。

**样例**
> Example 1:
>
> Input：s1="acb", n1=4, s2="ab", n2=2
>
> Output：2
>
> Explanation：get "abababab"  form "acbacbacbacb".

> Example 2:
>
> Input：s1="aaa", n1=3, s2="aa", n2=1
>
> Output：4
>
> Explanation：get "aaaaaaaa" from "aaaaaaaaa".

```c
class Solution {
public:
    /**
     * @param s1: the first string
     * @param n1: the repeat times of the first string
     * @param s2: the second string
     * @param n2: the repeat times of the second string
     * @return: the maximum integer
     */
    int getMaxRepetitions(string &s1, int n1, string &s2, int n2) {
        // Write your code here
        if(n1 == n2)
            return count_num( s1, s2 );
        else
        {
            s1 = get_str(s1, n1);
            s2 = get_str(s2, n2);
            return count_num( s1, s2 );
        }
    }
    
    string get_str(string s, int n)
    {
        string tmp = s;
        for(int i=0; i<n-1; i++)
            s += tmp;
        return s;
    }
    
    int count_num(string s1, string s2)
    {
        int i = 0, j = 0;
        int count = 0;
        while( i < s1.size() )
        {
            if(s1[i] == s2[j])
            {
                i++;  j++;
            }
            else
                i++;
            
            if(j == s2.size())
            {
                count++;
                j=0;
            }
        }
        return count;
    }
};
```


##### 判断回文子串
> 判断题目给出的字符串是不是回文，仅考虑字符串中的字母字符和数字字符，并且忽略大小写
例如："A man, a plan, a canal: Panama"是回文
"race a car"不是回文

重点：`isalnum()` `tolower()`
```c
bool isPalindrome(string s) {
    if(s.size() == 0)
        return true;
    int low=0, high=s.size()-1;
    while(low < high)
    {
        if(!isalnum(s[low]))
        {
            low++;
            continue;
        }
        if(!isalnum(s[high]))
        {
            high--;
            continue;
        }
        if(tolower(s[low]) == tolower(s[high]))
        {
            low++;
            high--;
            continue;
        }
        else
            return false;
    }
    return true;
}
```




