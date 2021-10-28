#### 300 最长递增子序列

```
解题思路
【简单思路】
首先，通过递归的方法，写一个L函数，功能是寻找从i位置开始的最长子序列的长度
先令记录最长长度的变量值为1
然后从i的下一个位置开始，用游标j遍历剩余位置
如果*j>*i，就再比较一次大小，如果j位置的长度+1大于最大长度，把j位置的长度+1记为最大长度
最后返回最大长度
然后再写一个函数，遍历所给数组，寻找从每一个位置开始的最长子序列，找出最大的那个值并返回
【空间换时间】
如果不优化，则不能通过
因为我们的L函数中有很多重复计算
比如某一个位置开始的最长子序列长度，我们可能计算了很多次
用一个数组把它记录下来，如果需要再次计算时，我们直接从数组中取值就好了
这种方法也叫动态规划
其实可以理解为带备忘录的递归
【语法】
用int?类型的数组，初始化时就均为空
为数组开辟空间：int?[] memo=new int?[10000];
比大小用Math.Max
```

```
public class Solution {
    int?[] memo=new int?[10000];
    public int L(int[] nums,int i)
    {
        if (memo[i]!=null)
            return (int)memo[i];

        if(i==nums.Length-1)
            return 1;

        int max_len=1;

        for(int j=i+1;j<nums.Length;j++)
        {
            if(nums[j]>nums[i])
                max_len=Math.Max(max_len,L(nums,j)+1);
        }
        memo[i] = max_len;
        return max_len;
    }

    public int LengthOfLIS(int[] nums)
    {
        int max=1;
        int temp=0;
        for(int i=0;i<nums.Length;i++)
        {
            temp=L(nums, i);
            if (temp>max)
                max=temp;
        }
        return max;
    }
}
```

#### 58. 最后一个单词的长度

```
【语法】
切片函数Split
s.Split(' ');
Length是数组的属性，不是函数，不用括号
a.Length;
foreach语句
foreach(string a in list)
【思路】
切片
从前往后，找到最后一个非空字符串，打印它的长度
```

```
public class Solution {
    public int LengthOfLastWord(string s) {
        string[] list=s.Split(' ');
        int len=0;
        foreach(string a in list)
        {
            if(a.Length>0)
                len=a.Length;
        }
        return len;
    }
}
```

#### 70. 爬楼梯

```
### 解题思路1
【思路】
走到第n阶，只可能通过第n-1阶、第n-2阶到达。
n阶的走法数=n-1阶的走法数+n-2阶的走法数之和。
为什么呢？
反正规律是这样子的，当时写出来时，想了好久都没明白。
具体原因在方法2中解释。
用一个递归函数，求n阶有几种走法。
【语法】
//记忆式搜索
int?[] memory=new int?[10000];
if(memory[n]!=null)
return (int)memory[n];
memory[n]=ClimbStairs(n-1)+ClimbStairs(n-2);
```

代码 方法一 记忆式搜索

```
public class Solution {
    //记忆式搜索
    int?[] memory=new int?[10000];
    //求走到第n阶，有几种走法
    public int ClimbStairs(int n) {
        if(memory[n]!=null)
            return (int)memory[n];
        if(n==1)
            return 1;
        if(n==2)
            return 2;
        if(n>2)
            memory[n]=ClimbStairs(n-1)+ClimbStairs(n-2);
            return (int)memory[n];
        return 0;
    }
}
```

解题思路2

```
【排列组合知识】
完成一件事有两类不同的方案
在第一类方案中有m种不同的方法，在第二类方案中有n种不同的方法
那么完成这件事共有N=m+n种不同的方法，这是分类加法计数原理；
完成一件事需要两个步骤，做第一步有m种不同的方法，做第二步有n种不同的方法．
那么完成这件事共有N=m×n种不同的方法，这就是分步乘法计数原理．
【思路】
注意：是走法，不是步数
设第1阶走到第n-1阶是x种走法，第1阶走到第n-2阶是y种走法
第n-1阶走到第n阶是1种走法，第n-2阶走到第n阶是1种走法
那么第1阶走到第n阶的走法数=x*1+y*1=x+y
```

代码 方法二 动态规划

```
public class Solution {
    //动态规划:存储到达当前位置的走法
    int?[] dp=new int?[10000];
    //求走到第n阶，有几种走法
    public int ClimbStairs(int n) {
        //设置初始状态
        //第一阶：一种
        //第二阶：两种
        dp[1]=1;
        dp[2]=2;
        //从第三阶开始，到第n阶，求第i阶的走法 
        //第n阶的【走法】等于第n-1阶的走法+第n-2阶的走法
        for(int i=3;i<=n;i++)
        {
            dp[i]=dp[i-1]+dp[i-2];
        }
        return (int)dp[n];
    }
}
```

#### 746. 使用最小花费爬楼梯

解题思路

【题目】
You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.
You can either start from the step with index 0, or the step with index 1.
Return the minimum cost to reach the top of the floor.

【模型】
![屏幕截图 2021年9月22日00时44分33秒.jpg](1632242685-iaauMG-屏幕截图 2021年9月22日00时44分33秒.jpg)

【思路】
要求写一个函数，求走到阶梯顶部的最小花费。

【问题1】怎么才能到达阶梯顶部呢？如何将过程拆解？
阶梯顶部，设其为第n阶。
按照规定，一次跨越1阶或2阶。那么只能通过离开第n-1阶或第n-2阶才能到达第n阶。

【问题2】怎么求走到阶梯顶部的最小花费呢？如何拆解？
走到第n阶的最小花费，有可能是走到第n-1阶的最小花费+离开第n-1阶的花费cost[n-1],也可能是走到第n-2的最小花费+离开第n-2阶花费cost[n-2]，取这两者之中的最小值。

【注释】
阶梯顶部的下标应为数组长度cost.Length。
比如Example1：阶梯顶部i=3。

【解题方法】
我们用一个数组dp存储走到第i阶的最小花费。
可以选择第0阶、第1阶为起点，故这两个位置最小花费均为0。
然后依次求：走到下一个位置的最小花费、走到第i个位置的最小花费、走到阶梯顶部的最小花费。
后面的计算，需要用到前面的计算结果。这就是动态规划。

代码

```
public class Solution {
    public int MinCostClimbingStairs(int[] cost) 
    {
        //动态规划记忆数组：存储走到第i阶的最小花费
        int?[] dp=new int?[10000];
        
        //可以选择第0阶、第1阶为起点，故这两个位置最小花费均为0
        dp[0]=0;
        dp[1]=0;

        for(int i=2;i<=cost.Length;i++) 
        {
            //走到第i阶的最小花费,可能是走到第i-1阶的最小花费+离开第i-1阶所需要的花费cost[n-1]
            //也可能是走到第i-2阶的最小花费+离开第i-2阶的花费cost[i-2]，取它们两个中的最小值
            dp[i]=Math.Min((int)dp[i-1]+cost[i-1],(int)dp[i-2]+cost[i-2]); 
        }
        return (int)dp[cost.Length];
    }
}
```

#### 剑指 Offer 30. 包含min函数的栈

```
解题思路

【思路】
用一个栈作为普通的栈，实现基本的功能
再用一个栈，作为辅助，存储递减的元素

普通栈每有一个元素入栈时，进行一次判断
如果辅助栈为空，进辅助栈
如果入栈元素小于辅助栈栈顶元素，进辅助栈

普通栈每有一个栈出栈时，进行一次判断
比较两个栈栈顶元素，如果相同，则一同出栈
如果不同，普通栈出栈

【语法】
声明并初始化一个栈
Stack<int> stack = new Stack<int>();
进栈
stack.Push(x);
出栈
stack.Pop();
查看栈顶
stack.Peek();
栈的判空
stack.Count==0
```

代码

```
public class MinStack
{

    /** initialize your data structure here. */
    Stack<int> stack = new Stack<int>();
    Stack<int> stackformin = new Stack<int>();

    public void Push(int x)
    {
        if(stackformin.Count==0)
            stackformin.Push(x);
        else if(x<=stackformin.Peek())
        {
            stackformin.Push(x);
        }
        stack.Push(x);
    }

    public void Pop()
    {
        if (stack.Peek() == stackformin.Peek())
            stackformin.Pop();
        stack.Pop();
    }

    public int Top()
    {
        return stack.Peek();
    }

    public int Min()
    {
        return stackformin.Peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.Push(x);
 * obj.Pop();
 * int param_3 = obj.Top();
 * int param_4 = obj.Min();
 */
```

#### 剑指 Offer 06. 从尾到头打印链表

```
解题思路

【思路】
先遍历一次链表，获得链表的长度
再逆序遍历一个和链表等长的数组，同时遍历链表，把链表元素的值依次填入
最后返回这个数组

【语法】
游标
ListNode p=head;
判断游标是否到达链表末尾
while(p!=null)
游标后移
p=p.next;
取值
p.val;
声明数组
int[] res=new int[length];
```

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int[] ReversePrint(ListNode head) {
        ListNode p=head;
        int length=0;
        while(p!=null)
        {
            length++;
            p=p.next;
        }
        int[] res=new int[length];
        p=head;
        for(int i=length-1;i>=0;i--)
        {
            res[i]=p.val;
            p=p.next;
        }
        return res;
    }
}
```

#### 剑指 Offer 24. 反转链表

解题思路

将链表元素逆序存储进一个临时数组
然后遍历临时数组，将元素存储进链表

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode p=head;
        int length=0;
        while(p!=null)
        {
            length++;
            p=p.next;
        }
        int[] res=new int[length];
        p=head;
        for(int i=length-1;i>=0;i--)
        {
            res[i]=p.val;
            p=p.next;
        }
        p=head;
        for(int i=0;i<=length-1;i++)
        {
            p.val=res[i];
            p=p.next;
        }
        return head;
    }
}
```

#### 剑指 Offer 05. 替换空格

解题思路

```
用Split函数将字符串以空格为间隔切片，存储到数组中
然后遍历数组，将单词粘贴到字符串末尾，并加上%20
最后一个单词不加%20
```

代码

```
public class Solution {
    public string ReplaceSpace(string s) 
    {
        string[] word;
        string res="";
        word=s.Split(" ");
        for(int i=0;i<word.Length;i++)
        {
            if(i!=word.Length-1)
                res=res+word[i]+"%20";
            else if(i==word.Length-1)
                res=res+word[i];
        }
        return res;
    }
}
```

#### 剑指 Offer 58 - II. 左旋转字符串

解题思路

```
【思路】
截取字符串，截取前面n位的字符和后面的字符
然后倒过来拼接在一起即可
【语法】
从索引处截取之后的全部字符
public String Substring(int startIndex);
从索引处截取指定长度的字符
public String Substring(int startIndex, int length);
```

代码

```
public class Solution {
    public string ReverseLeftWords(string s, int n) {
        string a=s.Substring(0,n);
        string b=s.Substring(n);
        return b+a;
    }
}
```

#### 1. 两数之和

解题思路

```
【思路】
for循环中有两个游标，一个前指针，一个后指针，后指针始终保持在前指针之前
【语法】
for循环比foreach麻烦，但是也更强大
for(int i=0;i<nums.Length;i++)
for(int j=i+1;j<nums.Length;j++)
以数组的形式返回
return new int[]{i,j};
```

代码

```
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        for(int i=0;i<nums.Length;i++)
            for(int j=i+1;j<nums.Length;j++)
            {
                if(nums[i]+nums[j]==target)
                    return new int[]{i,j};
            }
        return new int[]{0,0};
    }
}
```

#### 198. 打家劫舍

解题思路

```
【解法】
本题适合使用动态规划解法
【动态规划适用题型】
可以使用动态规划的问题一般都有一些特点可以遵循，题目的问法一般是三种方式：
1.求最大值/最小值
2.求可不可行
3.求方案总数
如果碰到一个问题，是问这三个问题之一的，就有90%概率是使用动态规划来求解
重点说明的是，如果一个问题是让求出所有的方案和结果，则肯定不是使用动态规划
【动态规划解题步骤】
①定义子问题
原问题是“从全部房子中能偷到的最大收益”，子问题就是“走到第i间房子时能偷到的最大收益（不一定偷第i间）”
②写出子问题的递推关系
i=0，只偷一间房，最大收益为该房收益
i=1，在两间房之中选一间偷，最大收益为钱多的那间房的收益
i=2，在三间房之中选一间或两间偷，最大收益为偷中间那家时的收益，或偷第三间房+偷前面那家的收益，取它们之中的最大值
i=k，偷到第k间房，可以选择偷第k间房 不偷第k-1间，也可以选择偷k-1间 不偷第k间。
此刻的最大收益为 f(k) = max(f(k-1),f(k-2)+nums[i])
③确定 DP 数组的计算顺序
动态规划有两种计算顺序
一种是自顶向下的、使用备忘录的递归方法
一种是自底向上的、使用 dp 数组的循环方法
不过在普通的动态规划题目中，99% 的情况我们都不需要用到备忘录方法，所以我们最好坚持用自底向上的 dp 数组。
④空间优化（可选）
很多时候我们并不需要始终持有全部的 DP 数组，只用两个变量保存两个子问题的结果，就可以依次计算出所有的子问题。
【思路】
用一个dp数组记录当前的状态，所谓状态就是遍历到此位置时，这里是指最多偷多少
dp[i]是所有情况中的的最值，包括偷这间房和不偷这间房的情况，是它们之中的最大收益
而非选择偷这间房的最大收益
那么通过动态规划的遍历计算之后，动规数组的最后一个元素dp[nums.Length-1]就是最大值
【动规方程】
偷到一个位置的最大收益=偷到前一个位置的最大收益（不偷当前位置） 或 偷到前前位置的最大收益+偷当前位置的收益（偷当前位置）
dp[i]=Math.Max(dp[i-2]+nums[i],dp[i-1]);
【语法】
int[] dp=new int[nums.Length+1];
```

代码

```
public class Solution {
    public int Rob(int[] nums) {
        //动态规划数组：记录从第0间或第1间开始，一直偷到当前位置，可能偷到最多的钱的数值
        int[] dp=new int[nums.Length+1];

        //动态规划数组的初始化
        dp[0]=nums[0];
        //只有一间房的情况
        if(nums.Length==1)
            return dp[0];
        dp[1]=Math.Max(nums[0],nums[1]);

        //偷第三间房之后的情况
        for(int i=2;i<nums.Length;i++)
        {
            dp[i]=Math.Max(dp[i-2]+nums[i],dp[i-1]);
        }
        return dp[nums.Length-1];
    }
}
```

针对空间复杂度优化后的代码

```
循环开始时，cur 表示 dp[i-1]，pre 表示 dp[i-2]
循环结束时，cur 表示 dp[i]，pre 表示 dp[i-1]
```

```
public class Solution {
    public int Rob(int[] nums) {
        int pre=0;
        int cur=0;
        foreach(int i in nums)
        {
            int temp = Math.Max(cur, pre + i);
            pre = cur;
            cur = temp;
        }
        return cur;
    }
}
```

#### 725. 分隔链表

解题思路

```
【伪代码】
遍历链表

情况1
如果小划分的长度为0，那么大划分的长度为1
先处理大划分
由于每一个大划分只有一个元素
遍历到大划分时，保存当前节点，将其next域设为空
小划分没有元素，无需处理

情况2
如果小划分长度不为0，那么大划分长度是小划分长度+1
如果有大划分，先处理大划分
遍历到大划分的开头时，保存其划分的起始节点，将上一个划分的末尾元素置为空
遍历到大划分的末尾时，剩余大划分数量自减
处理任意一个划分后，初始化计数器
处理小划分
遍历到小划分的开头时，保存其划分的起始节点
【总结】
逻辑过于复杂，写出来的代码难以调试且不可维护
```

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution
{
    public ListNode[] SplitListToParts(ListNode head, int k)
    {
        ListNode p = head;

        int length = 0;
        while (p != null)
        {
            length++;
            p = p.next;
        }

        int smalldividelength = length / k;
        int bigdividelength = smalldividelength + 1;
        int bigdividecount = length % k;
        ListNode[] listarray = new ListNode[k];

        int i = 0;
        int j = 0;
        p = head;
        ListNode q = head;

        while (p != null)
        {
            if (smalldividelength == 0)
            {
                listarray[j] = p;
                q = p;
                p = p.next;
                q.next = null;
                j++;
            }
            else if (bigdividecount > 0)
            {
                if (i % bigdividelength == 0)
                {
                    listarray[j] = p;
                    if (j != 0)
                    {
                        q.next = null;
                    }
                    j++;
                }
                if (i == bigdividelength - 1)
                {
                    bigdividecount--;
                    i = -1;
                }
                q = p;
                p = p.next;
            }
            else if (bigdividecount == 0)
            {
                if (i % smalldividelength == 0)
                {
                    if (j != 0)
                    {
                        q.next = null;
                    }
                    listarray[j] = p;
                    if (j < listarray.Length - 1)
                        j++;
                }
                q = p;
                p = p.next;
            }
            i++;
        }
        return listarray;
    }
}
```

#### 326. 3的幂

```
解题思路

【暴力法】
int最大为:2147483647
从3的0次方比到19次方即可
【语法】
幂运算 3的i次方
Math.Pow(3,i)
【调试】
Console.Write();
```

代码

```
public class Solution {
    public bool IsPowerOfThree(int n) 
    {
        for(int i=0;i<=19;i++)
        {
            if(n==Math.Pow(3,i))
                return true;
        }
        return false;
    }
}
```

未通过代码

```
public class Solution {
    public bool IsPowerOfThree(int n) 
    {
        int temp=1;
        for(int i=0;i<=n;i++)
        {
            if(n==temp)
                return true;
            else
                temp=temp*3;
        }
        return false;
    }
}
```

【注释】将for循环条件改为i<=19即可

#### 566. 重塑矩阵

解题思路

```
【思路】
先遍历原交错数组，遍历到某一个元素时，计算出它在一维数组中的下标ixc+j，并存放于一维数组
再遍历新的交错数组，计算出当前下标在一维数组中的下标，将对应的元素填入
【语法】
foreach是只读的，不可用于赋值
更多：[C# 交错数组](https://haiyue.blog.csdn.net/article/details/120433302)
【调试】
Console.Write();
【注释】
交错数组中存放的整数个数不等于arr.Length
```

代码

```
public class Solution {
    public int[][] MatrixReshape(int[][] mat, int r, int c) {
        //原交错数组中的元素个数
        int count=0;
        foreach(int[] i in mat)
        {
            foreach(int j in i)
            {
                count++;
            }
        }
        //如果不可重塑，返回原矩阵
        if(r*c!=count)
            return mat;
        else
        {
            //一维辅助数组
            int[] temp=new int[r*c];
            //交错数组：此处仅仅初始化了一维，还不能直接对二维进行操作
            int[][] res=new int[r][];
            //遍历原交错数组
            for(int i=0;i<mat.Length;i++)
                for(int j=0;j<mat[i].Length;j++)
                {
                    temp[i*mat[0].Length+j]=mat[i][j];
                }
            //遍历新交错数组
            for(int i=0;i<res.Length;i++)
            {
                //交错数组中的数组必须先初始化再使用
                res[i]=new int[c];
                for(int j=0;j<c;j++)
                {
                    res[i][j]=temp[i*c+j];
                }
            }
            return res;
        }
    }
}
```

更优解法

```
public class Solution
{
    public int[][] MatrixReshape(int[][] mat, int r, int c)
    {
        //原行数
        int originrow = mat.Length;
        //原列数
        int origincol = mat[0].Length;
        //判断是否不可转化
        if (r * c != originrow * origincol) return mat;
        //创建交错数组
        var res = new int[r][];
        //为交错数组中的数组开辟空间
        for (int i = 0; i < r; i++)
            res[i] = new int[c];
        //为交错数组赋值
        //将原矩阵行列与现矩阵行列对应
        //一维转二维
        //行数=一维下标/列数
        //列数=一维下标%列数
        for (int i = 0; i < r * c; i++)
        {
            res[i / c][i % c] = mat[i / origincol][i % origincol];
        }
        return res;
    }
}
```

#### 118. 杨辉三角

解题思路

```
【思路】
首尾元素为1
非首末元素=上一行同位置元素+上一行前一位置元素
【语法】
【IList】
The IList<T> generic interface is a descendant of the ICollection<T> generic interface and is the base interface of all generic lists.
【IList<IList<int>>】
交错数组继承于IList<IList<>>接口
故返回值可以使用int[][]类型的交错数组
```

代码

```
public class Solution {
    public IList<IList<int>> Generate(int numRows) {
        //声明交错数组
        var res = new int[numRows][];
        //遍历交错数组第一维
        for (int i = 0; i < numRows; i++)
        {
            //为内含的每一个数组开辟空间
            res[i] = new int[i + 1];
            //首尾元素为1
            res[i][0] = 1;
            res[i][i] = 1;
            //为内含的每一个数组的中间元素赋值
            for (int j = 1; j < i; j++)
            {
                //非首末元素=上一行同位置元素+上一行前一位置元素
                res[i][j] = res[i - 1][j - 1] + res[i - 1][j];
            }
        }
        return res;
        //return res.ToArray();
    }
}
```

#### 344. 反转字符串

解题思路

```
【方法】
双指针
【思路】
首尾指针一同向中间移动，交换元素
```

代码

```
public class Solution {
    public void ReverseString(char[] s) {
        char head;
        char tail;
        for(int i=0;i<s.Length/2;i++)
        {
            head=s[i];
            tail=s[s.Length-1-i];
            s[s.Length-1-i]=head;
            s[i]=tail;
        }
    }
}
```

#### 557. 反转字符串中的单词 III

解题思路

```
【思路】
用Split以空格切割字符串，保存到字符串数组中
将字符串数组转化为字符交错数组 StringArrayToCharJaggedArray
遍历字符交错数组，对内部的字符数组进行逆序 ReverseString
依次写入到字符串中并加入空格，然后返回
【语法】
[String与Char相关类型的转换](https://haiyue.blog.csdn.net/article/details/120439931)
```

代码

```
public class Solution {
    public string ReverseWords(string s) 
    {
        string[] words=s.Split(" ");
        char[][] word=StringArrayToCharJaggedArray(words);
        string res="";
        for(int i=0;i<word.Length;i++)
        {
            ReverseString(word[i]);
            res=res+new string(word[i]);
            //res=res+CharArrayToString(word[i]);
            if(i!=word.Length-1)
                res=res+" ";
        }
        return res;
    }

    public void ReverseString(char[] s) 
    {
        char head;
        char tail;
        for(int i=0;i<s.Length/2;i++)
        {
            head=s[i];
            tail=s[s.Length-1-i];
            s[s.Length-1-i]=head;
            s[i]=tail;
        }
    }

    string CharArrayToString(char[] s)
    {
        string str = "";
        foreach (char c in s)
        {
            char temp = c;
            str = str + temp.ToString();
        }
        return str;
    }

    char[][] StringArrayToCharJaggedArray(string[] words)
    {
        char[][] word=new char[words.Length][];
        for(int i=0;i<words.Length;i++)
        {
            word[i]=new char[words[i].Length];
            word[i]=words[i].ToCharArray();
        }
        return word;
    }
}
```

【更优解法】

Linq

```
public class Solution {
    public string ReverseWords(string s) 
    {
		if (s == null || s.Length == 1) 
            return s;
		return string.Join(" ", s.Split(' ').Select(x => new string(x.Reverse().ToArray())));
    }
}
```

StringBuilder

```
public class Solution {
    public string ReverseWords(string s) 
    {
        string[] stringArray = s.Split(' ');
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < stringArray.Length; i++)
        {
            for (int j = stringArray[i].Length - 1; j >= 0; j--)
            {
                stringBuilder.Append(stringArray[i][j]);                 
            }
            stringBuilder.Append(" ");
        }
        stringBuilder.Remove(s.Length, 1);
        return stringBuilder.ToString();
    }
}
```

#### 4. 寻找两个正序数组的中位数

解题思路

```
【思路】
将两个数组拼接起来，用一个数组存放
排序
如果是偶数个，求中间两个数的平均数
如果是奇数个，输出中位数
【语法】
排序 默认升序
Array.Sort(arr);
【注意】
需要强制转换为double类型，再进行除法运算，不然会得到整数的结果
```

代码

```
public class Solution {
    public double FindMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] nums3=new int[nums1.Length+nums2.Length];
        for(int i=0;i<nums3.Length;i++)
        {
            if(i<nums1.Length)
                nums3[i]=nums1[i];
            else
                nums3[i]=nums2[i-nums1.Length];
        }
        Array.Sort(nums3);
        foreach(var i in nums3)
        {
            Console.Write(i);
        }
        double res=0;
        if(nums3.Length%2==0)
            res=((double)nums3[nums3.Length/2-1]+nums3[nums3.Length/2])/2;
        else if(nums3.Length%2==1)
            res=nums3[nums3.Length/2];
        return res;
    }
}
```

#### 剑指 Offer 03. 数组中重复的数字

解题思路

```
【思路】
【双重循环】
遍历数组，一个指针指向开头，一个指针指向前一个指针的下一位
如果两个指针所指向元素相等，说明当前元素有重复，则直接返回
注：该算法无法满足时间复杂度的要求
其实用双重循环是多余的，用一个循环就足够了

【哈希表】
遍历数组，如果哈希表中不存在某一元素，将其存入哈希表
如果已存在某一元素，将其直接返回

【字典】
遍历数组，如果字典中不存在某一元素，将其存入字典
如果已存在某一元素，将其直接返回

【数组】
用一个数组保存当前遍历到的数字的数量
如果个数大于1，将其直接返回
```

双重循环（RUNTIME ERROR）

```
public class Solution {
    public int FindRepeatNumber(int[] nums) 
    {
        int?[] repeat=new int?[nums.Length];
        for(int slow=0;slow<nums.Length;slow++)
            for(int fast=slow+1;fast<nums.Length;fast++)
            {
                if(repeat[nums[fast]]==1)
                    continue;
                if(nums[slow]==nums[fast])
                    return nums[fast];
            }
        return 0;
    }
}
```

哈希表

```
public class Solution {
    public int FindRepeatNumber(int[] nums) 
    {
        Hashtable ht = new Hashtable();
        for(int i=0;i<nums.Length;i++)
        {
            if(ht.Contains(nums[i]))
                return nums[i];
            else
                ht.Add(nums[i],1);
        }
        return 0;
    }
}
```

字典

```
public class Solution {
    public int FindRepeatNumber(int[] nums) 
    {
        Dictionary<int, int> dict = new Dictionary<int, int>();
        for(int i=0;i<nums.Length;i++)
        {
            if(dict.ContainsKey(nums[i]))
                return nums[i];
            else
                dict.Add(nums[i],1);
        }
        return 0;
    }
}
```

数组

```
public class Solution {
    public int FindRepeatNumber(int[] nums) {
        int[] count=new int[nums.Length];
        for(int i=0;i<nums.Length;i++)
        {
            count[nums[i]]++;
            if(count[nums[i]]>1)
                return nums[i];
        }
        return 0;
    }
}
```

#### 剑指 Offer 50. 第一个只出现一次的字符

解题思路

```
【思路】
遍历字符串，用字典存放出现的字符和次数
遍历字典，输出出现次数为1的数
【语法】
想要返回一个字符，可以直接 return ' ';
如果是字符串，可以直接 return "hello world";
以下属性均需要大写，不然报错:'KeyValuePair<char, int>.key' is inaccessible due to its protection level
字典键 d.Key
字典值 d.Value
[哈希表与字典](https://haiyue.blog.csdn.net/article/details/120445477)
【注】
因为字典的顺序就是插入的顺序，所以直接遍历字典，输出字典中第一个出现次数为1的数就好了
```

代码

```
public class Solution {
    public char FirstUniqChar(string s) 
    {
        Dictionary<char,int> dict=new Dictionary<char,int>();
        foreach(var c in s)
        {
            if(dict.ContainsKey(c))
                dict[c]=dict[c]+1;
            else
                dict.Add(c,1);
        }
        foreach(var d in dict)
        {
            Console.Write(d);
            Console.Write(d.Key);
            if(d.Value==1)
                return d.Key;
        }
        return ' ';
    }
}
```

#### 剑指 Offer 04. 二维数组中的查找

解题思路

```
【方法】
二分查找

【二分查找】
[二分查找](https://haiyue.blog.csdn.net/article/details/120452285)
首先，待查找的数组必须是有序的
用两个游标low、high分别指向数组的开头和末尾，再用一个游标mid指向这两个游标的中间位置
如果target等于mid，查找成功
如果target小于mid，说明target在数组的前半部分，那么就把high游标前移一位
如果target大于mid，说明target在数组的后半部分，那么就把high游标后移一位
每循环一次，更新mid游标的位置，让其始终处于两个游标的中间位置
当low>high时，说明这一半数组中没有这个元素，已知另一半也没有，所以查找失败，结束循环

【思路1】
将交错数组存储进一维数组
然后对一维数组进行排序
最后使用二分查找

【思路2】
对交错数组中的每一个数组进行二分查找
```

思路1

```
public class Solution {
    public bool FindNumberIn2DArray(int[][] matrix, int target) 
    {
        int[] array=JaggedArrayTo1DArray(matrix);
        Array.Sort(array);
        return BinarySearch(array,target);
    }

    public int[] JaggedArrayTo1DArray(int[][] matrix) 
    {
        int count=0;
        foreach(int[] i in matrix)
        {
            foreach(int j in i)
            {
                count++;
            }
        }
        int[] array=new int[count];
        for(int i=0;i<matrix.Length;i++)
        {
            for(int j=0;j<matrix[i].Length;j++)
            {
                array[i*matrix[0].Length+j]=matrix[i][j];
            }
        }
        return array;
    }

    public bool BinarySearch(int[] array,int target)
    {
        int low=0;
        int high=array.Length-1;
        int mid;
        while(low<=high)
        {
            mid=(low+high)/2;
            if(target==array[mid])
                return true;
            else if(target<array[mid])
                high--;
            else if(target>array[mid])
                low++;
        }
        return false;
    }
}
```

思路2

```
public class Solution {
    public bool FindNumberIn2DArray(int[][] matrix, int target) {
        foreach(var i in matrix)
        {
            bool res;
            if(res=BinarySearch(i,target))
                return res;
        }
        return false;
    }

    public bool BinarySearch(int[] array,int target)
    {
        int low=0;
        int high=array.Length-1;
        int mid;
        while(low<=high)
        {
            mid=(low+high)/2;
            if(target==array[mid])
                return true;
            else if(target<array[mid])
                high--;
            else if(target>array[mid])
                low++;
        }
        return false;
    }
}
```

#### 剑指 Offer 11. 旋转数组的最小数字

解题思路

```
【语法】
使用数组自带的函数
array.Min();
此外，还有array.Max()、array.Average()
```

代码

```
public class Solution {
    public int MinArray(int[] array) {
        return array.Min();
    }
}
```

#### 217. 存在重复元素

解题思路
用哈希表作为存放数组元素，如果表中已存在，返回true
存放完还没有发生重复，则返回false

代码

```
public class Solution {
    public bool ContainsDuplicate(int[] nums) {
        Hashtable ht = new Hashtable();
        foreach(var i in nums)
        {
            if(ht.Contains(i))
                return true;
            else
                ht.Add(i,1);
        }
        return false;
    }
}
```

#### 53. 最大子序和

解题思路

```
【解法】
动态规划
【理解】
动态规划，就是
将问题分解，缩小问题规模
找到状态转移方程
借助数组保存上一次计算的状态
通过空间换时间
减少不必要的计算
将时间复杂度由O（n2）降低到O（n）
【最佳思路】
创建一个dp数组，保存以当前位置元素为结尾的连续子数组的最大和
对dp数组的第0个元素初始化，使之等于所给数组的第0个元素
然后从1开始，遍历所给数组，计算dp[i]
有两个选择：
一是当前元素把当前元素加入子数组
二是舍弃之前的子数组，只以当前元素作为子数组
由此可得状态转移方程：
dp[i]=Math.Max(dp[i-1]+nums[i],nums[i]);
【注意】
这一题，答案并不是dp数组的最后一个元素
而是dp数组中的最大值
【状态转移方程】
状态转移方程是什么？怎么找到状态转移方程？
其实，状态转移方程可以理解为高中数学中等差数列、等比数列的递推公式
通过前一个状态的值来计算出当前的状态值
【语法】
return dp.Max();
```

代码

```
public class Solution
{
    public int MaxSubArray(int[] nums)
    {
        int[] dp=new int[nums.Length];
        dp[0]=nums[0];
        for(int i=1;i<nums.Length;i++)
        {
            dp[i]=Math.Max(dp[i-1]+nums[i],nums[i]);
        }
        return dp.Max();
    }
}
```

错误示例一

```
【思路】
dp数组保存以当前元素为结尾的最大的子数组的和
只有两种可能
一是从某一位置到此的子数组，和最大，二是仅此元素为应该子数组，和最大
要计算从某一位置到此的最大和，只需要倒着数，数到头即可
【错误原因】
本代码的计算结果并没有问题，但是时间复杂度过高，为O（n2），超出时间限制
看起来用了动态规划，其实没有
这里实际上是用了一个数组，记录了以当前元素为结尾的连续子数组的最大和
但是在计算这个最大和的时候，并没有借助到dp数组降低时间复杂度
而是用了双重循环嵌套，一个又一个地重新算了一遍
其实是没有理解动态规划的精髓，即借助dp数组和状态转移方程，来降低时间复杂度
没有理解dp数组和状态转移方程的作用
只是形似神不似，本质上是暴力法
```

代码

```
public class Solution {
    public int MaxSubArray(int[] nums) {
        int[] dp=new int[nums.Length];
        dp[0]=nums[0];
        for(int i=1;i<nums.Length;i++)
        {
            int max=-9999999;
            int count=0;
            for(int j=i;j>=0;j--)
            {
                count=count+nums[j];
                max=Math.Max(max,count);    
            }
            dp[i]=max;
        }
        return dp.Max();
    }
}
```

错误示例二

```
【思路】
dp数组保存从开始位置到当前位置中某一段的连续子数组的最大值
如果当前数比之前的子数组之和大，并且之前子数组之和小于0，重新设置一个子数组
如果当前数小于0，最大子数组之和不变
如果当前数大于0，并且最大子数组之和大于0
计算Start到i的和:dp[i-1] + 从end+1到i的和【这一步有问题：可以从i加到start，记录当中的最大值】
比较三者的值：Start到i的和、当前i值、dp[i-1]
哪个最大，执行哪个的操作
\#1
dp[i]=Start到i的和
End=i
\#2
dp[i]=nums[i]
Start=i
End=i
\#3
dp[i]=dp[i-1]
【错误原因】
本思路的dp数组，保存从开始位置到当前位置中某一段的连续子数组的最大值
这是行不通的，因为你没办法确定dp数组保存的连续子数组的最大和，到底是从哪个位置开始，从哪个位置结束
这样保存下来的数据，就不能对未来的计算产生帮助了
每一次计算dp[i]，都得重新算，这样就根本不是动态规划，只是披着动态规划的外衣的暴力法
解答的时候，思路过于混乱，并没有找到简单的规律
只是把手算的过程强行转化为了编码
并没有对思路进行抽象和优化
本应该把三种情况合并为一个状态转移方程
写得太复杂了，这就是屎山代码，不可维护，就算通过了，也是不可取的
```

代码

```
public class Solution {
    public int MaxSubArray(int[] nums) {
        int[] dp=new int[nums.Length];
        dp[0]=nums[0];
        int start=0;
        int end=0;
        for(int i=1;i<nums.Length;i++)
        {
            //Console.Write(dp[i-1]+"\n"+start+","+end+"\n");
            if(dp[i-1]<0 && nums[i]>dp[i-1])
            {
                dp[i]=nums[i];
                start=i;
                end=i;
            }
            else if(nums[i]<0)
            {
                dp[i]=dp[i-1];
            }
            else if(nums[i]>0 && dp[i-1]>0)
            {
                //int startToi=nums.Where(x => x>=start && x<=i).Sum();
                int startToi=0;
                for(int k=start;k<=i;k++)
                {
                    startToi=startToi+nums[k];
                }
                //Console.Write("*"+i+":"+startToi+"*");
                int max;
                max=Math.Max(startToi,nums[i]);
                max=Math.Max(max,dp[i-1]);
                dp[i]=max;

                if(max==startToi)
                {
                    end=i;
                }
                else if(max==nums[i])
                {
                    start=i;
                    end=i;
                }
                else if(max==dp[i-1])
                {
                    
                }
            }
        }
        return dp[nums.Length-1];
    }
}
```

#### 55. 跳跃游戏

解题思路

```
【解法】
动态规划
【思路】
一、问题规模分解
当前位置可以到达的最远位置，要么等于当前位置+可跳跃长度 ，要么等于前一位置可以到达的最远位置，这取决于谁更大
二、状态转移方程/递推方程
dp[i]=Math.Max(dp[i-1],nums[i]+i);
三、出现符合条件的情况，即刻返回
当可达最远距离大于等于最后一个元素的下标时
if(dp[i]>=nums.Length-1)
return true;
【特殊情况】
数组长度为1的情况需要特别处理
【语法】
在stdout中输出换行
Console.Write("\n");
```

代码

```
public class Solution {
    public bool CanJump(int[] nums) 
    {
        int[] dp=new int[nums.Length];
        dp[0]=nums[0];
        if(nums.Length==1)
            return true;
        for(int i=1;i<=dp[i-1];i++)
        {
            dp[i]=Math.Max(dp[i-1],nums[i]+i);
            if(dp[i]>=nums.Length-1)
                return true;
        }
        return false;
    }
}
```

贪心法

```
public bool CanJump(int[] nums) 
{
    int canReach = 0;
    for(int i=0;i<=canReach&&i<nums.Length;i++)
    {
        if (i+nums[i]>canReach)
        {
            canReach=i+nums[i];
            if (canReach>=nums.Length-1)
                return true;
        }
    }
    return canReach >= nums.Length-1;
}
```

#### 45. 跳跃游戏 II

解题思路

```
【解法】
动态规划

【思路】
遍历所给数组，更新当前位置可达范围内所有位置的 到达该位置的最少跳跃次数

【注意】
循环内用于调试的Console.Write()记得删除或注释掉
不然可能因此超时，导致不能通过
```

代码

```
public class Solution {
    public int Jump(int[] nums) 
    {
        int len=nums.Length;
        int[] dp=new int[len];
        dp[0]=0;
        for(int i=1;i<len;i++)
        {
            dp[i]=int.MaxValue;
        }
        for(int i=0;i<len-1;i++)
        {
            for(int j=i+1;j<=i+nums[i];j++)
            {
                if(j<=dp.Length-1)
                {
                    if(dp[i]+1<dp[j])
                        dp[j]=dp[i]+1;
                    //dp[j]=Math.Min(dp[i]+1,dp[j]);
                }
            }
        }
        return dp[len-1];
    }
}
```

贪心法

在这次跳跃可达的范围内，选择下一次可以跳得最远的那一个位置
也就是说，这一次可以不跳最远，是为了下一次的跳得最远

#### 242. 有效的字母异位词

```
解题思路
利用两个哈希表，分别存储两个单词的字符频次
然后判断两个哈希表是否相等
```

【判断两个哈希表相等】

```
bool isSame=ht1.Cast<DictionaryEntry>().Union(ht2.Cast<DictionaryEntry>()).Count() == ht1.Count && ht1.Count==ht2.Count;
return isSame;
```

```
【语法】
不可以使用 ht1.Equals(ht2)
这仅仅是判断是否是同一个引用（内存空间）
遍历哈希表时，不能用var，只能用DictionaryEntry
foreach (DictionaryEntry c in ht1)
哈希表的键值对进行自加操作，需要将Object转换为int
ht1[c]=(int)ht1[c]+1;

【自评】
方法过于暴力
其实可以只用一个哈希表
先存储一个单词的频次，每次加1
再遍历另一个单词，每次减1
如果最后哈希表的键的值有非零的数存在，说明两个单词的字符频次不等
```

### 代码

```
public class Solution {
    public bool IsAnagram(string s, string t) 
    {
        Hashtable ht1=new Hashtable();
        Hashtable ht2=new Hashtable();
        foreach(var c in s)
        {
            if(!ht1.Contains(c))
                ht1.Add(c,1);
            else
                ht1[c]=(int)ht1[c]+1;
        }
        foreach(var c in t)
        {
            if(!ht2.Contains(c))
                ht2.Add(c,1);
            else
                ht2[c]=(int)ht2[c]+1;
        }
        bool isSame=ht1.Cast<DictionaryEntry>().Union(ht2.Cast<DictionaryEntry>()).Count() == ht1.Count && ht1.Count==ht2.Count;
        return isSame;
    }
}
```

#### 73. 矩阵置零

代码 O(mn)

```
public class Solution {
    public void SetZeroes(int[][] matrix) 
    {
        //深拷贝
        int[][] res=JaggedArrayDeepCopy(matrix);

        //不能在原数组上进行修改，因为这会影响后续计算的结果
        //根据原数组的值对新数组进行修改
        //行
        for(int i=0;i<=matrix.Length-1;i++)
        {
            //列
            for(int j=0;j<=matrix[i].Length-1;j++)
            {
                if(matrix[i][j]==0)
                {
                    Set(res,i,j);
                }
            }
        }
        //让原数组与新数组的值保持一致
        //行
        for(int i=0;i<=matrix.Length-1;i++)
        {
            //列
            for(int j=0;j<=matrix[i].Length-1;j++)
            {
                matrix[i][j]=res[i][j];
            }
        }
    }

    //将一个位置的同行或者同列元素全部置为0
    void Set(int[][] res,int r,int c) 
    {
        //把某一行的所有元素置为0
        for(int i=0;i<res[0].Length;i++)
        {
            res[r][i]=0;
        }
        //把某一列的所有元素置为0
        for(int i=0;i<res.Length;i++)
        {
            res[i][c]=0;
        } 
    }

    //对交错数组进行深拷贝
    int[][] JaggedArrayDeepCopy(int[][] array)
    {
        int[][] res=new int[array.Length][];
        for(int i=0;i<array.Length;i++)
        {
            res[i]=(int[])array[i].Clone();
        }
        return res;
    }
}
```

#### 371. 两整数之和

解题思路

```
【解法】
位运算

【思路】
先用与运算和左移运算求进位
再用a异或b求无进位加法
然后再用位运算把进位结果与无进位加法相加
直到没有进位，计算结束

【异或】
异或的数学符号是⊕，在计算机中用^作运算符。
如果a、b两个值不相同，则异或结果为1。
如果a、b两个值相同，异或结果为0。

【移位】
左移位 <<
右移位 >>

【语法】
c#采用unicode编码方式，
可以使用中文变量
虽然这不是好习惯

【计组】
计算机中所有的数都是以补码的形式存储的
正数的源码、反码、补码都相同
负数的反码，符号位为1，其余位取反
负数的补码是反码+1

【补码】
计算机中所有的数都是以补码的形式存储的
用一个字节，也就是8位来表示数，最高位设置为符号位，可表示2^8=256个有符号数
用00000000~11111111表示0~127、-128~-1
-1 11111111
-2 11111110
-127 10000001
-128 10000000
127 01111111
2 00000010
1 00000001
0 00000000
```

```
【补码】
计算机中所有的数都是以补码的形式存储的
用一个字节，也就是8位来表示数，最高位设置为符号位，可表示2^8=256个有符号数
用00000000~11111111表示0~127、-128~-1
-1 11111111
-2 11111110
-127 10000001
-128 10000000
127 01111111
2 00000010
1 00000001
0 00000000
```

![image.png](1632638079-xlCdcU-image.png)

```
【无符号整数】
有符号位的表达形式是最高位是符号位，其余是数值
int型整数的最高位是符号位
一般来说，使用左移位运算时，符号位会被移去
负数的符号位会丢失
left shift of negative value
所以，如果用c++进行移位，必须使用unsigned int的数据类型
如果是Java、C#则不必
```

代码

```
public class Solution {
    public int GetSum(int a, int b) {
        while (b != 0) 
        {
            int 进位 = (a & b) << 1;
            int 无进位加法 = a ^ b;
            a = 无进位加法;
            b = 进位;
        }
        return a;
    }
}
```

#### 141. 环形链表

解题思路

```
【思路】
如果两个游标，一个一次走一步，一个一次走两步，如果走到同一个位置，说明有环

【语法】
遍历链表循环的结束出口
指针指向到空时，结束遍历
是while(p!=null)
而不是while(p.next!=null)，这样写的话
要么最后一个节点无法被遍历到（一个循环前进一步）
要么p本身就不存在，指针指向了空（一个循环前进了两步），报错：空引用
```

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public bool HasCycle(ListNode head) 
    {
        ListNode fast=head;
        ListNode slow=head;
        while(fast!=null)
        {
            fast=fast.next;
            if(fast==slow)
                return true;
            if(fast!=null)
                fast=fast.next;
            if(fast==slow)
                return true;
            slow=slow.next;
        }
        return false;
    }
}
```

#### 21. 合并两个有序链表

```
解题思路

两个游标pq分别负责遍历两个链表
r游标负责遍历临时链表
res指针负责保存临时链表的头节点
当两个链表都没有遍历到末尾时
如果两个值相等：
先把r当前指向的节点指向其中一个节点
再让r指向这个节点
这个节点所在链表的游标前移
然后让这个节点指向另一个与它的值相同的节点
然后r再指向那个节点
那个节点所在链表的游标前移
如果不等：
先把r当前指向的节点指向小的那个节点
再让r指向这个节点
最后小的那个节点所在的链表的游标前移
当其中一个链表遍历到末尾时
让r的当前节点指向长的那个链表的游标
最后返回头节点的下一个元素
```

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode MergeTwoLists(ListNode l1, ListNode l2) {
        ListNode p=l1;
        ListNode q=l2;
        ListNode r=new ListNode();
        ListNode res=r;

        while(p!=null&&q!=null)
        {
            if(p.val>q.val)
            {   
                r.next=q;
                r=q;
                q=q.next;
            }
            else if(p.val<q.val)
            {
                r.next=p;
                r=p;
                p=p.next;
            }
            else if(p.val==q.val)
            {
                r.next=p;
                r=r.next;
                p=p.next;
                r.next=q;
                r=q;
                q=q.next;
            }
        }

        if(p==null&&q!=null)
		    r.next=q;
	    else if(q==null&&p!=null)
		    r.next=p;
            
        return res.next;
    }
}
```

#### 203. 移除链表元素

解题思路

```
【思路】
可以用两个指针,一个用于遍历，一个用于记录待删除节点的位置
也可以用一个指针，遍历的时候只对下一个节点的值进行判断，这样就不用记录待删除节点的上一个节点了
【语法】
新建一个头节点并让其指向链表的第一个节点
ListNode r=new ListNode();
r.next=p;
【自评】
面向测试样例编程..这代码写得太烂了
```

官方解答

```
public class Solution {
    public ListNode RemoveElements(ListNode head, int val) 
    {
        if (head == null) 
            return head;
        //递归
        //当前节点指向下下个节点或下一个节点
        head.next = RemoveElements(head.next, val);
        return head.val == val ? head.next : head;
    }
}
```

自己写的屎山代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */

public class Solution {
    public ListNode RemoveElements(ListNode head, int val) {
        ListNode p=head;
        ListNode r=new ListNode();
        r.next=p;

        if(p==null)
        {
            return null;
        }

        while(p!=null && p.val==val && p.next!=null)
        {
            r.next=p.next;
            p=p.next;
        }
        
        //如果下一个节点是与目标值匹配，则删除
        while(p!=null && p.next!=null)
        {
            if(p.next.val==val)
            {
                p.next=p.next.next;
            }
            else
                p=p.next;
        }

        if(p!=null && p.val==val && p.next==null)
        {
            return null;
        }

        return r.next;
    }
}
```

#### 20. 有效的括号

解题思路

```
【思路】
用栈来存储待匹配的左括号
用哈希表来保存左右括号
遍历字符串
如果是左括号（是哈希表的键）
则将其值进栈，也就是与其匹配的右括号
如果不是左括号，说明是右括号
如果栈不为空，将其出栈，不匹配就直接结束
如果栈为空，该右括号则无匹配对象，不匹配，直接结束
遍历完字符串，如果栈为空，则匹配成功
如果不为空，说明有左括号未匹配
【语法】
Stack<char> stack=new Stack<char>();
```

代码

```
public class Solution {
    public bool IsValid(string s) {
        Stack<char> stack=new Stack<char>();
        Hashtable ht=new Hashtable();
        ht.Add('(',')');
        ht.Add('[',']');
        ht.Add('{','}');
        foreach(var c in s)
        {
            if(ht.Contains(c))
                stack.Push((char)ht[c]);
            else
            {
                if(stack.Count!=0)
                //if(stack.Count!=0&&stack.Pop()!=c) 这种写法无法通过
                {
                    char top=stack.Pop();
                    if(top!=c)
                        return false;
                }
                else if(stack.Count==0)
                    return false;
            }
        }
        return stack.Count==0?true:false;
    }
}
```

#### 350. 两个数组的交集 II

解题思路

```
【思路】
利用两个哈希表分别统计两个数组中的数字出现频次
用Intersect()对两个数组元素求交集(不存在重复元素)
再用ToArray()转成数组
然后遍历交集元素数组
依据两个哈希表中最小的频次将数字添加到列表
再把列表转成数组
【语法】
nums1.Intersect(nums2).ToArray();
```

代码

```
public class Solution {
    public int[] Intersect(int[] nums1, int[] nums2) {
        Hashtable ht1=ElementCount(nums1);
        Hashtable ht2=ElementCount(nums2);
        int[] elements=nums1.Intersect(nums2).ToArray();
        List<int> list=new List<int>();
        foreach(var i in elements)
        {
            int min=Math.Min((int)ht1[i],(int)ht2[i]);
            while(min!=0)
            {
                list.Add(i);
                min--;
            }
        }
        return list.ToArray();
    }
    Hashtable ElementCount(int[] array)
    {
        Hashtable ht= new Hashtable();
        foreach(var i in array)
        {
            if(ht.Contains(i))
                ht[i]=(int)ht[i]+1;
            else
                ht.Add(i,1);
        }
        return ht;
    }
}
求交集（忽略重复元素）
return nums1.Where(x=>nums2.Contains(x)).Distinct().ToArray();
return nums1.Intersect(nums2).ToArray();
查看哈希表
foreach(DictionaryEntry i in ht1)
{
    Console.Write(i.Key+" "+i.Value+"\n");
}
```

#### 349. 两个数组的交集

解题思路

```
用求交集的Intersect函数比用Linq手写的查询语句更快
```

代码

```
public class Solution {
    public int[] Intersection(int[] nums1, int[] nums2) {
        return nums1.Intersect(nums2).ToArray();
        //return nums1.Where(x=>nums2.Contains(x)).Distinct().ToArray();
    }
}
```

#### 83. 删除排序链表中的重复元素

解题思路

```
【思路】
当前节点不为空，并且当前节点的指针域不为空时，进行以下循环操作：
当链表当前节点的值等于下一节点的值时，当前节点指向下下个节点。
当链表当前节点的值不等于下一节点的值时，游标前移。
【技巧】
画图
```

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode DeleteDuplicates(ListNode head) 
    {
        ListNode p=head;
        while(p!=null&&p.next!=null)
        {
            if(p.val==p.next.val)
            {
                p.next=p.next.next;
            }
            else
                p=p.next;
        }
        return head;
    }
}
```

#### 面试题 08.06. 汉诺塔问题

解题思路

```
【注意】
无论何时，盘子只能叠在比它大的盘子上。

【思路】
写一个递归的Move函数，将n个盘借助middle柱子，从depart柱子移到target柱子。
首先，先将n-1个盘子从depart柱子移到middle柱子，临时存放
然后，将depart柱子剩下的最后一个盘子移到target柱子。
最后，把middle柱子的n-1个盘子，借助depart柱子移到target柱子
递归出口：如果只移一个盘子，直接把这个盘子移过去就好，然后记得return，不然栈会溢出
【递归】
函数调用自己，分解问题规模，然后在递归出口返回

【语法】
IList<int> 数据类型
获取长度
不是使用list.Length
而是使用list.Count
添加元素
Add()
移除某一位置元素
RemoveAt()
```

代码

```
public class Solution {
    public void Hanota(IList<int> A, IList<int> B, IList<int> C) 
    {
        Move(A,B,C,A.Count);
    }

    void Move(IList<int> depart, IList<int> middle, IList<int> target,int n)
    {
        if(n==1)
        {
            target.Add(depart[depart.Count - 1]);
            depart.RemoveAt(depart.Count - 1);
            return;//StackOverflowException
        }
        
        Move(depart,target,middle,n-1);
        Move(depart,middle,target,1);
        Move(middle,depart,target,n-1);
    }
}
```

#### 144. 二叉树的前序遍历

解题思路

```
【思路】
递归调用先序遍历函数，对根节点进行先序遍历
如果遍历到的节点为空，return结束递归
如果不为空，输出节点的值
递归遍历左节点之后
再递归遍历右节点
【优化】
把res列表作为全局遍历写在类的外面，就不必每次都传参了
【语法】
左右节点的写法
在C#和Java中没有指针的概念，所以链表的next域或者树的左右节点，都是使用成员变量来存储
比如root.left或root.right，而非T->lchild或T->rchild
判断节点为空
是if(root==null)
不能写作if(root)
因为root不是bool类型的变量
【先序遍历】
根左右
```

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    List<int> list=new List<int>();
    public IList<int> PreorderTraversal(TreeNode root) {
        PreorderTraverse(root);
        return list;
    }
    void PreorderTraverse(TreeNode root)
    {
    if(root==null)
    return;
    //Console.Write(root.val);
    list.Add(root.val);
    PreorderTraverse(root.left);
    PreorderTraverse(root.right);
    }
}
```

#### 94. 二叉树的中序遍历

解题思路

【中序遍历】
左根右

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    List<int> list=new List<int>();
    public IList<int> InorderTraversal(TreeNode root) {
        InorderTraverse(root);
        return list;
    }
    void InorderTraverse(TreeNode root)
    {
        if(root==null)
            return;
        InorderTraverse(root.left);
        list.Add(root.val);
        //Console.Write(root.val);
        InorderTraverse(root.right);
    }
}
```

#### 145. 二叉树的后序遍历

解题思路

【后序遍历】
左右根

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    List<int> list=new List<int>();
    public IList<int> PostorderTraversal(TreeNode root) {
        PostorderTraverse(root);
        return list;
    }
    void PostorderTraverse(TreeNode root)
    {
        if(root==null)
            return;
            
        PostorderTraverse(root.left);
        PostorderTraverse(root.right);
        list.Add(root.val);
        //Console.Write(root.val);
    }
}
```

#### 102. 二叉树的层序遍历

解题思路

```
【层序遍历】
先将根节点入队
当队列不为空时，循环读取队列中的节点，读取后出队，并将其子节点加入队列
如果队列为空，遍历结束

【按层次将元素存放至列表】
在遍历某一层之前，先获取该层的元素个数
然后根据记录的个数遍历，将该层的数加入列表
再把该列表加入列表

【语法】
声明一个队列
Queue<TreeNode> queue = new Queue<TreeNode>();
Queue queue = new Queue();
*声明一个列表的列表
IList<IList<int>> listoflist=new List<IList<int>>();
入队
queue.Enqueue(root);
出队
TreeNode node=(TreeNode)queue.Dequeue();
出队的元素为Object类型，需要强制类型转换
队列元素个数
queue.Count
左节点
node.left
右节点
node.right
```

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    //用队列存储某一层的节点
    Queue<TreeNode> queue = new Queue<TreeNode>();
    //返回值
    IList<IList<int>> listoflist=new List<IList<int>>();
    public IList<IList<int>> LevelOrder(TreeNode root) {
        LevelTraverse(root);
        return listoflist;
    }
    
    void LevelTraverse(TreeNode root) 
    {
        //根节点入队
        queue.Enqueue(root);
        //记录某一层中的元素个数
        int LevelCount=0;

        //遍历队列
        while(queue.Count!=0)
        {
            //某一层的节点个数
            LevelCount=queue.Count;
            //存储某一层的元素
            IList<int> list=new List<int>();
            //某一层的第一个节点
            TreeNode node=(TreeNode)queue.Dequeue();
            //如果队列为空，结束
            if(node==null) break;

            //遍历某一层的所有元素
            for(int i=0;i<LevelCount;i++)
            {
                //将当前元素加入列表
                list.Add(node.val);
                //Console.Write(node.val+" ");

                //如果左右节点不为空，则入队
                if(node.left!=null)
                    queue.Enqueue(node.left);
                if(node.right!=null)
                    queue.Enqueue(node.right);
                
                //如果不是该行末尾元素，队列元素出队
                if(i!=LevelCount-1)
                    node=(TreeNode)queue.Dequeue(); 
            }
            //将某一层的所有元素的列表加入列表
            listoflist.Add(list);
            //Console.Write("\n");
        }
    }
}
```

无注释代码

```
public class Solution {
    public IList<IList<int>> LevelOrder(TreeNode root) 
    {
        Queue<TreeNode> queue = new Queue<TreeNode>();
        IList<IList<int>> listoflist=new List<IList<int>>();
        queue.Enqueue(root);
        int LevelCount=0;

        while(queue.Count!=0)
        {
            LevelCount=queue.Count;
            IList<int> list=new List<int>();
            TreeNode node=(TreeNode)queue.Dequeue();
            if(node==null) break;

            for(int i=0;i<LevelCount;i++)
            {
                list.Add(node.val);

                if(node.left!=null)
                    queue.Enqueue(node.left);
                if(node.right!=null)
                    queue.Enqueue(node.right);
                
                if(i!=LevelCount-1)
                    node=(TreeNode)queue.Dequeue(); 
            }
            listoflist.Add(list);
        }
        return listoflist;
    }
}
```

如果不需要分层输出，就无需记录每一层的元素个数

```
void LevelOrder(TreeNode root) 
{
    Queue<TreeNode> queue = new Queue<TreeNode>();
    queue.Enqueue(root);
    while(queue.Count!=0)
    {
        TreeNode node=(TreeNode)queue.Dequeue();
        if(node==null) break;
        Console.Write(node.val+" ");
        if(node.left!=null)
            queue.Enqueue(node.left);
        if(node.right!=null)
            queue.Enqueue(node.right);
    }
    return;
}
```

#### 剑指 Offer 32 - I. 从上到下打印二叉树

解题思路

```
先将根节点入队
当队列不为空时，循环读取队列中的节点，读取后出队，并将其子节点加入队列
如果队列为空，遍历结束
```

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int[] LevelOrder(TreeNode root) 
    {
        List<int> list=new List<int>();
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        while(queue.Count!=0)
        {
            TreeNode node=(TreeNode)queue.Dequeue();
            if(node==null) break;
            Console.Write(node.val+" ");
            list.Add(node.val);
            if(node.left!=null)
                queue.Enqueue(node.left);
            if(node.right!=null)
                queue.Enqueue(node.right);
        }
        return list.ToArray();
    }
}
```

#### 剑指 Offer 55 - I. 二叉树的深度

解题思路1 层序遍历

```
【层序遍历】
先将根节点入队
当队列不为空时，循环读取队列中的节点
队列中存放某一层的元素
然后遍历该层的元素，将其子节点加入队列
每遍历一层，统计层数
如果队列为空，遍历结束
```

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int MaxDepth(TreeNode root) 
    {
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        int LevelCount=0;
        int maxdepth=0;

        while(queue.Count!=0)
        {
            LevelCount=queue.Count;
            TreeNode node=(TreeNode)queue.Dequeue();
            if(node==null) break;

            for(int i=0;i<LevelCount;i++)
            {
                if(node.left!=null)
                    queue.Enqueue(node.left);
                if(node.right!=null)
                    queue.Enqueue(node.right);
                
                if(i!=LevelCount-1)
                    node=(TreeNode)queue.Dequeue(); 
            }
            maxdepth++;
        }
        return maxdepth;
    }
}
```

解题思路2 递归遍历

从根节点递归遍历二叉树
每递归遍历子节点，当前层次+1
遍历完子节点后，当前层次-1
然后用深度depth记录最大的层次

代码

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int x) { val = x; }
 * }
 */
public class Solution 
{
    int depth=1,currentLevel=1;
    void Traverse(TreeNode root)
    {
        if(root!=null)
        {
            depth=Math.Max(depth,currentLevel);
            Console.Write(depth);
            currentLevel++;
            Traverse(root.left);
            Traverse(root.right);
            currentLevel--;
        }
    }

    public int MaxDepth(TreeNode root) 
    {
        if(root!=null)
            Traverse(root);
        else
            depth=0;
        return depth;
    }
}
```

#### 617. 合并二叉树

解题思路

```
如果某一位置上，有一个节点为空，返回非空节点
如果两个节点均不为空，创建一个新节点
要赋三个值：this.val、this.lef、this.right
其值等于两个节点的值相加
其左节点等于递归调用自身，合并该位置的左节点的返回值
其右节点等于递归调用自身，合并该位置的右节点的返回值

【语法】
*可空对象不能强制转换为int型
不能这样写：node.val=(int)(root1?.val+root2?.val);
报错：Nullable object must have a value
可以这样：node.val=(root1==null?0:root1.val)+(root2==null?0:root2.val);
```

别人的写法

```
public class Solution {
    public TreeNode MergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        TreeNode node = new TreeNode(root1.val+root2.val);
        node.left = MergeTrees(root1.left,root2.left);
        node.right = MergeTrees(root1.right,root2.right);
        return node;
    }
}
```

我的写法

```
public class Solution {
    public TreeNode MergeTrees(TreeNode root1, TreeNode root2) 
    {
        if(root1==null&&root2==null)
            return null;

        TreeNode node=new TreeNode();
        node.val=(root1==null?0:root1.val)+(root2==null?0:root2.val);
        //Console.Write(node.val+" ");
        node.left=MergeTrees(root1?.left,root2?.left);
        node.right=MergeTrees(root1?.right,root2?.right);
        return node;
    }
}
```

最初的代码

高度具体，没有对多个条件进行抽象合并

```
public class Solution {
    public TreeNode MergeTrees(TreeNode root1, TreeNode root2) 
    {
        return Traverse(root1,root2);
    }

    TreeNode Traverse(TreeNode root1, TreeNode root2)
    {
        if(root1==null&&root2==null)
            return null;
            
        TreeNode root3=new TreeNode();

        if(root1!=null&&root2!=null)
        { 
            root3.val=root1.val+root2.val;
            Console.Write(root3.val+" ");
            root3.left=Traverse(root1.left,root2.left);
            root3.right=Traverse(root1.right,root2.right);
        }
        else if(root1!=null&&root2==null)
        {
            root3.val=root1.val;
            Console.Write(root3.val+" ");
            root3.left=Traverse(root1.left,null);
            root3.right=Traverse(root1.right,null);
        }
        else if(root1==null&&root2!=null)
        {
            root3.val=root2.val;
            Console.Write(root3.val+" ");
            root3.left=Traverse(null,root2.left);
            root3.right=Traverse(null,root2.right);
        }
        return root3;
    }
}
```

#### 剑指 Offer II 045. 二叉树最底层最左边的值

解题思路

获取层序遍历返回的结果
然后输出交错数组最后一个数组的第一个元素

代码

```
public class Solution {
    public int FindBottomLeftValue(TreeNode root) 
    {
        return LevelOrder(root);
    }

    public int LevelOrder(TreeNode root) 
    {
        Queue<TreeNode> queue = new Queue<TreeNode>();
        IList<IList<int>> listoflist=new List<IList<int>>();
        queue.Enqueue(root);
        int LevelCount=0;

        while(queue.Count!=0)
        {
            LevelCount=queue.Count;
            IList<int> list=new List<int>();
            TreeNode node=(TreeNode)queue.Dequeue();
            if(node==null) break;

            for(int i=0;i<LevelCount;i++)
            {
                list.Add(node.val);

                if(node.left!=null)
                    queue.Enqueue(node.left);
                if(node.right!=null)
                    queue.Enqueue(node.right);
                
                if(i!=LevelCount-1)
                    node=(TreeNode)queue.Dequeue(); 
            }
            listoflist.Add(list);
        }
        return listoflist[listoflist.Count-1][0];
    }
}
```

#### 700. 二叉搜索树中的搜索

解题思路

```
二叉搜索树是一种节点值之间具有一定数量级次序的二叉树，对于树中每个节点：
若其左子树存在，则其左子树中每个节点的值都不大于该节点值；
若其右子树存在，则其右子树中每个节点的值都不小于该节点值。
```

正确写法

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    public TreeNode SearchBST(TreeNode root, int val) {
        TreeNode sub=null;
        if(root==null)
            return null;
        if(val<root.val)
            sub=SearchBST(root.left,val);
        else if(val>root.val)
            sub=SearchBST(root.right,val);
        else if(val==root.val)
            return root;
        return sub;
    }
}
```

错误写法

没有保存递归函数的返回值

```
public class Solution {
    public TreeNode SearchBST(TreeNode root, int val) {
        if(root==null)
            return null;
        if(val<root.val)
            SearchBST(root.left,val);
        else if(val>root.val)
            SearchBST(root.right,val);
        else if(val==root.val)
            return root;
        return null;
    }
}
```

改进

```
public class Solution {
    public TreeNode SearchBST(TreeNode root, int val) {
        if(root==null)
            return null;
        if(val<root.val)
            return SearchBST(root.left,val);
        else if(val>root.val)
            return SearchBST(root.right,val);
        else if(val==root.val)
            return root;
        return null;
    }
}
```

别人的写法

```
public class Solution {
    public TreeNode SearchBST(TreeNode root, int val) {
        if(root == null || root.val == val) return root;
        return root.val > val ? SearchBST(root.left,val) : SearchBST(root.right,val);
    }
}
```

#### 剑指 Offer 10- I. 斐波那契数列

代码

```
public class Solution {
    public int Fib(int n) {
        int[] dp=new int[101];
        dp[0]=0;
        dp[1]=1;
        for(int i=2;i<=n;i++)
        {
            dp[i]=(dp[i-2]+dp[i-1])%1000000007;
        }
        return dp[n];
    }
}
```

错误

```
看题：第一个数是0
i<=n，注意=
return dp[n];
dp存放的是数列第i项的值
数列从0开始，第n项的值就是dp[n]
```

```
public class Solution {
    public int Fib(int n) 
    {
        int[] dp=new int[101];
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<n;i++)
        {
            dp[i]=dp[i-2]+dp[i-1];
        }
        return dp[n-1];
    }
}
```

#### 148. 排序链表

冒泡排序 O(n2)

【结果没问题，超时，无法通过】

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode SortList(ListNode head) 
    {
        LSort(head);
        return head;
    }

    int ListLength(ListNode L)
    {
        //遍历链表用的指针，存放当前遍历节点的地址 
        ListNode p;
        //p指向第一个结点
        p=L;
        
        //遍历链表统计的结点个数     
        int i=0;        
        while(p!=null)
        {
            i++;
            //使指针指向下一个节点（存放下一个节点的地址） 
            p=p.next;   
        } 
        //Console.Write(i);
        return i;                             
    }

    //链表排序 
    void LSort(ListNode L)
    {
        //获取链表长度 
        int n=ListLength(L);
        //遍历链表用的指针，存放当前遍历节点的地址 
        ListNode p;
        //从头到尾，每两两比较完一轮，一定能在n-1-i范围内选出一个极大值，后面就多了一个确定位置的数
        //n个元素要比较n-1轮 
        //i代表已经比好的，位于末尾的元素个数 
        for(int i=0;i<n-1;i++)
        {
            //从第一个结点开始 
            p=L;
            //每运行完一次大循环，后面就有一个数确定位置，无需比较，下一次只需要比较前面n-1-i个即可 
            //小循环遍历次数计数器，每比较完一轮小循环，计数器j归零 
            int j=0;
            while(p!=null&&j<n-1-i)
            {
                //比较两个相邻节点数据域的大小，把大的放在前面 
                if(p.val>p.next.val)
                {
                    //Console.Write("*");
                    int t=p.val;
                    p.val=p.next.val;
                    p.next.val=t;
                }
                //指向下一节点 
                p=p.next;
                j++;
            }
        }
        return;
    }
}
```

#### 912. 排序数组

```
快速排序

平均时间复杂度为 O(nlogn)
快速排序无法通过，因为当数组有序时，快速排序的时间复杂度变为O(n2)
需要使用随机化的快速排序
```

##### 随机化快速排序 代码

```
public class Solution {
    public int[] SortArray(int[] nums) 
    {
        RandomQuickSort(nums,0,nums.Length-1);
        return nums;
    }

    void RandomQuickSort(int[] a, int left, int right) {
        if (left < right) {
            int p = randomPartition(a, left, right);
            RandomQuickSort(a, left, p - 1);
            RandomQuickSort(a, p + 1, right);
        }
    }

    //快速排序数组划分
    int partition(int[] a, int left, int right) {
        int x = a[right];
        int p = left - 1;
        for (int i = left; i < right; i++) {
            if (a[i] <= x) {
                p++;
                swap(a, p, i);
            }
        }
        swap(a, p+1, right);
        return p+1;
    }

    //随机化划分
    int randomPartition(int[] a, int left, int right) {
        Random random = new Random();
        int r = random.Next(0,right - left)+ left; //生成一个随机数，即是主元所在位置
        swap(a, right, r); //将主元与序列最右边元素互换位置，这样就变成了之前快排的形式。
        return partition(a, left, right); //直接调用之前的代码
    }

    //交换数组a中的a[i]和a[j]
    void swap(int[] a, int i, int j) 
    {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```

##### 快速排序 最差O（n2）

有序的情况下，O（n2），会超时

数组有n个元素，因为要递归运算，算出pivot的位置，然后递归调用左半部分和有半部分，这个时候理解上是若第一层的话就是n/2，n/2，若是第二层就是n/4,n/4,n/4,n/4这四部分，即n个元素理解上是一共有几层2^x=n，x=logn，然后每层都是n的复杂度，那么平均就是O(nlogn)的时间复杂度。但这种肯定是平均情况，如果你是标准排序的情况下，如果已经是ascending的顺序，那么递归只存在右半部分了，左半部分都被淘汰了。(n-1)+(n-2)+....+1，这个复杂度肯定就是O(n^2)，这种情况还不如用插入排序。

```
public class Solution {
    public int[] SortArray(int[] nums) {
        QuickSort(nums,0,nums.Length-1);
        return nums;
    }

    void QuickSort(int[] nums, int l, int r)
    {
        if (l < r)
        {
            int i = l, j = r;
            int pivot = nums[l];
            while (i < j)
            {
                while(i < j && nums[j]>= pivot) // 从右向左找第一个小于pivot的数
                    j--;
                if(i < j) 
                    nums[i++] = nums[j];
                
                while(i < j && nums[i] < pivot) // 从左向右找第一个大于等于pivot的数
                    i++;  
                if(i < j) 
                    nums[j--] = nums[i];
            }
            nums[i] = pivot;
            QuickSort(nums, l, i - 1); // 递归调用 
            QuickSort(nums, i + 1, r);
        }
    }
}
```

##### 快速排序 更好的写法

```
public class Solution {
    public int[] SortArray(int[] nums) {
        QuickSort(nums,0,nums.Length-1);
        return nums;
    }

    void QuickSort(int[] nums, int left, int right)
    {
        if (left < right)
        {
            //先对整体进行一次划分排序
            int pivotIndex = Partition(nums,left,right);
            //分别再对pivot左边、右边的数进行划分
            QuickSort(nums, left, pivotIndex - 1);
            QuickSort(nums, pivotIndex + 1, right);
        }
    }

    //让pivot左边的数都比它小，右边的数都比它大
    int Partition(int[] nums, int left, int right)
    {
        //基准取左边
        int pivot=nums[left];
        int low = left, high = right;
        while (low < high)
        {
            // 从右向左找第一个小于基准的数，把它放到low的位置上，这个位置最终在基准的左边
            while(low < high && nums[high]>= pivot) high--;
            if(low < high) nums[low++] = nums[high];
            
            // 从左向右找第一个大于等于基准的数，把它放到high的位置上，这个位置最终在基准的右边
            while(low < high && nums[low] < pivot) low++; 
            if(low < high) nums[high--] = nums[low];
        }
        //最后把pivot放到low的位置
        nums[low] = pivot;
        //返回pivot值的下标
        return low;
    }
}
```

##### 归并排序

先把数组拆分成单个元素一组
再两组两组地合并，合并的时候在组内排序
合并完成后把结果给原数组

```
public class Solution {
    public int[] SortArray(int[] nums) {
        MergeSort(nums,0,nums.Length-1);
        return nums;
    }

    void MergeSort(int[]nums,int left,int right)
    {
        if(left>=right) return;
        int mid=(left+right)/2;//括号
        MergeSort(nums,left,mid);
        MergeSort(nums,mid+1,right);
        Merge(nums,left,mid,right);
    }

    void Merge(int[] nums,int left,int mid,int right)
    {
        int[] temp=new int[right-left+1];
        int p1=left;
        int p2=mid+1;
        int count=0;

        while(p1<=mid && p2<=right)//<=
        {
            if(nums[p1]>nums[p2])
            {
                temp[count++]=nums[p2++];
            }
            else
            {
                temp[count++]=nums[p1++];
            }
        }
        while(p1<=mid)
        {
            temp[count++]=nums[p1++];
        }
        while(p2<=right)
        {
            temp[count++]=nums[p2++];
        }

        for(int i=0;i<count;i++)
        {
            nums[left+i]=temp[i];//left+i
        }
    }
}
```

##### 快速排序 

可通过版本 1892ms

```
public class Solution {
    public int[] SortArray(int[] nums) {
        QuickSort(nums,0,nums.Length-1);
        return nums;
    }

    void QuickSort(int[] nums, int left, int right)
    {
        if(left>=right) return;
        int pivotIndex = Partition(nums,left,right);
        QuickSort(nums,left,pivotIndex-1);//-1
        QuickSort(nums,pivotIndex+1,right);
    }

    int Partition(int[] nums, int left, int right)
    {
        int pivot = nums[left];
        while(left<right)
        {
            while (left < right && nums[right] >= pivot) right--;
            if (left < right) nums[left++] = nums[right];
            while (left < right && nums[left] < pivot) left++;
            if (left < right) nums[right--] = nums[left];
        }
        nums[left] = pivot;//首尾呼应
        return left;
    }
}
```

##### 堆排序

```
public class Solution {
    public int[] SortArray(int[] nums) {
        HeapSort(nums);
        return nums;
    }

    //堆排序
    void HeapSort(int[] nums)
    {
        //构建一个最大堆
        toMaxHeap(nums);
        //从最后一个节点开始，将根节点(最大值)与最后一个节点交换
        //那么最后一个位置就排好了，然后再调整前面的未排序部分，每次都确保根节点是局部最大值
        for (int i = nums.Length - 1; i > 0; i--)
        {
            Swap(nums, 0, i);
            HeapAdjust(nums, 0, i);
        }
    }

    //构建最大堆：所有的父结点都比它的孩子结点数值大
    void toMaxHeap(int[] nums)
    {
        //从最后一个非叶节点开始，往前遍历所有非叶节点，将其子树调整为大根堆
        for (int i = nums.Length / 2 - 1; i >= 0; i--)
        {
            HeapAdjust(nums, i, nums.Length);
        }
    }

    //堆调整:将子树i调整为最大堆
    //unsortedHeapSize：堆中还未排好序的元素个数
    void HeapAdjust(int[] nums, int i, int unsortedHeapSize)
    {
        //当前节点i的左子节点的下标
        int left = 2 * i + 1;
        //当前节点i的右子节点的下标
        int right = left + 1;
        //将根、左、右中最大值摆放至根节点
        int max = i;
        if (left < unsortedHeapSize && nums[left] > nums[max])
            max = left;
        if (right < unsortedHeapSize && nums[right] > nums[max])
            max = right;
        if (max != i)
        {
            Swap(nums, i, max);
            //递归调整下一层
            HeapAdjust(nums, max, unsortedHeapSize);
        }
    }

    void Swap(int[] nums, int i, int j)
    {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

#### 14. 最长公共前缀

解题思路

```
【语法】
从0位置开始截取i个字符
str.Substring(0, i);
```

代码

```
public class Solution {
    public string LongestCommonPrefix(string[] strs) 
    {
        if (strs.Length==0)
        {
            return "";
        }
        
        //遍历第一个字符串中的字符
        for (int i = 0; i < strs[0].Length; ++i) 
        {
            //第一个字符串的第i个字符
            char c = strs[0][i];
            //遍历其他字符串
            for (int j = 1; j < strs.Length; ++j) 
            {
                //如果i已经达到了j的末尾，或者j的第i个字符与第一个字符串的第i个字符不等
                if (i == strs[j].Length || strs[j][i] != c) 
                {
                    //从0位置开始截取i个字符
                    return strs[0].Substring(0, i);
                }
            }
        }
        //如果没有提前返回，说明公共字符串是第一个字符串本身
        return strs[0];
    }
}
```

#### 7. 整数反转

解题思路

```
从一个数的末尾每次取一个数
将上一次的res*10，然后把这个数加到res中
再把这个数除10，以便下次取末尾的数
如果除到0了，结束循环
反转后整数的结果可能超过 32 位的有符号整数的范围，所以需要用long保存
如果大于int可以存储的范围，返回0
```

代码

```
public class Solution {
    public int Reverse(int x)
    {
        long res = 0;

        while (x != 0)
        {
            int temp = x % 10;
            res = res * 10 + temp;
            x = x / 10;
        }

        //超过 32 位的有符号整数的范围
        if (res>int.MaxValue || res<int.MinValue)
        {
            res = 0;
        }

        return (int)res;
    }
}
```

#### 3. 无重复字符的最长子串

解题思路

```
【思路】
创建一个队列用于存储无重复字符的最长子串
遍历所给字符串，如果队列中无此字符，则将其加入队列
如果队列中已有此子符，则将当前长度，与最大进行比较，然后将队列中重复元素之前的元素全部出队

【语法】
Queue queue=new Queue();
queue.Contains(c)
(char)queue.Dequeue()
```

代码

```
public class Solution {
    public int LengthOfLongestSubstring(string s) {
        Queue queue=new Queue();
        int maxlen=0;
        foreach(var c in s)
        {
            if(queue.Contains(c))
            {
                while(c!=(char)queue.Dequeue())
                {

                }
                queue.Enqueue(c);
            }
            else
            {
                queue.Enqueue(c);
                maxlen=Math.Max(maxlen,queue.Count);
                // foreach(var i in queue)
                // {
                //     Console.Write(i);
                // }
                // Console.Write("\n");
            }
        }
        return maxlen;
    }
}
```

#### 2. 两数相加

【思路】
当两个节点均不为空时，两数之和=p.val+q.val+上一次计算的进位
本次计算的进位为本次的两数之和/10
当前位的值为两数之和%10
只要下一位有任一节点 或者 下一位没有节点但是有进位时
就创建下一位的节点

【语法】
尾插法创建链表

```
//头指针
ListNode head=new ListNode();
//尾指针
ListNode r=head;
//给节点赋值
r.val=value;
//创建一个新节点，让尾指针指向新节点
r.next=new ListNode();
//尾指针后移
r=r.next;
```

【可空运算符】
如果节点p可能为空，在其非空时，要p的值，在其空时，想让其为0，写作：
(p?.val??0) 或者 (p==null?0:p.val)
如果节点p可能为空，在其非空时，需要遍历下一个节点，写作：
p=p?.next;
如果不对可能为空的p节点进行一个空判断（?），则会报空引用
使用可空运算符，可大大降低代码量，将三种情况合为一种

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        int 进位=0;
        int sum;
        ListNode p=l1;
        ListNode q=l2;
        //头指针
        ListNode head=new ListNode();
        //尾指针
        ListNode r=head;
        while(p!=null&&q!=null)
        {
            sum=p.val+q.val+进位;
            进位=sum/10;
            sum=sum%10;
            r.val=r.val+sum;
            if(p.next!=null||q.next!=null||进位!=0)
            {
                r.next=new ListNode();
                r=r.next;
            }
            p=p.next;
            q=q.next;
        }
        while(p!=null&&q==null)
        {
            Console.Write("#");
            sum=p.val+进位;
            进位=sum/10;
            sum=sum%10;
            r.val=r.val+sum;
            if(p.next!=null||进位!=0)
            {
                r.next=new ListNode();
                r=r.next;
            }
            p=p.next;
        }
        while(p==null&&q!=null)
        {
            sum=q.val+进位;
            进位=sum/10;
            sum=sum%10;
            r.val=r.val+sum;
            if(q.next!=null||进位!=0)
            {
                r.next=new ListNode();
                r=r.next;
            }
            q=q.next;
        }
        if(p==null&&q==null&&进位!=0)
        {
            r.val=进位;
        }
        return head;
    }
}
```

优化写法

```
public class Solution {
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        int 进位=0;
        int sum;
        ListNode p=l1;
        ListNode q=l2;
        //头指针
        ListNode head=new ListNode();
        //尾指针
        ListNode r=head;
        while(p!=null||q!=null)
        {
            sum=(p?.val??0)+(q?.val??0)+进位;
            //sum=(p==null?0:p.val)+(q==null?0:q.val)+进位;
            进位=sum/10;
            sum=sum%10;
            r.val=r.val+sum;
            if(p?.next!=null||q?.next!=null||进位!=0)
            {
                r.next=new ListNode();
                r=r.next;
            }
            p=p?.next;
            q=q?.next;
        }
        if(p==null&&q==null&&进位!=0)
        {
            r.val=进位;
        }
        return head;
    }
}
```

#### 445. 两数相加 II

解题思路

```
【思路】
先将两个链表元素压栈
然后再出栈，求和，求进位
最后用头插法建立链表

【头插法】
新节点指针指向创建的新节点
r=new ListNode(和);
新节点指向头节点
r.next=head;
头节点指针指向新节点
head=r;
```

代码

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        Stack<ListNode> s1=new Stack<ListNode>();
        Stack<ListNode> s2=new Stack<ListNode>();
        ListNode p1=l1;
        ListNode p2=l2;

        //将两个链表的元素全部压栈
        while(p1!=null||p2!=null)
        {
            if(p1!=null)s1.Push(p1);
            if(p2!=null)s2.Push(p2);
            p1=p1?.next;
            p2=p2?.next;
        }

        //头指针
        ListNode head=null;
        //新节点指针
        ListNode r;
        int 进位=0;
        int 和=0;
        while(s1.Count>0||s2.Count>0)
        {
            int num1=s1.Count>0?s1.Pop().val:0;
            int num2=s2.Count>0?s2.Pop().val:0;
            和=num1 + num2 + 进位;
            进位=和/10;
            和=和%10;

            //头插法
            r=new ListNode(和);
            r.next=head;
            head=r;
        }

        if(p1==null && p2==null && 进位>0)
        {
            r=new ListNode(进位);
            r.next=head;
            head=r;
        }

        return head;
    }
}
```

#### 29. 两数相除

解题思路

```
【语法】
2的31次方-1是int型整数（32位）能表示的最大值
这个最大值有一个常量叫:int.MaxValue
```

【try-catch处理异常】

```
try
{
    //可能发生异常的代码：除法移除
    r=dividend/divisor;
}
catch
{
    //异常处理
    return int.MaxValue;
}
```

代码

```
public class Solution {
    public int Divide(int dividend, int divisor) {
        int r=0;
        try
        {
            r=dividend/divisor;
        }
        catch
        {
            return int.MaxValue;
        }
        return r;
    }
}
```

#### 剑指 Offer 18. 删除链表的节点

解题思路

```
删除中间节点：
在往前遍历之前，记录下上一个节点
把上一节点指向后一节点

删除头节点：
一般来说，删除中间的节点和删除头节点的操作不同
如果单独写一个逻辑来处理，会比较麻烦
不如加一个头节点，让头节点指向当前的第一个节点
最后返回头节点的next

【语法】
可空判断只能写在运算符的右侧
p=p?.next;(√)
lastNode?.next=p.next;(x)
```

代码

```
public class Solution {
    public ListNode DeleteNode(ListNode head, int val) {
        ListNode p=head;
        ListNode lastNode=new ListNode();
        lastNode.next=head;
        head=lastNode;
        while(p!=null)
        {
            if(p.val==val)
            {
                lastNode.next=p.next;
            }
            lastNode=p;
            p=p.next;
        }
        return head.next;
    }
}
```

#### 剑指 Offer 22. 链表中倒数第k个节点

解题思路

```
先遍历获取长度
再找倒数第k个：count-k==0
```

代码

```
public class Solution {
    public ListNode GetKthFromEnd(ListNode head, int k) {
        ListNode p=head;
        int count=0;
        while(p!=null)
        {
            count++;
            p=p.next;
        }
        p=head;
        while(count-k>0)
        {
            p=p.next;
            count--;
        }
        return p;
    }
}
```

#### 剑指 Offer 52. 两个链表的第一个公共节点

代码

```
public class Solution {
    public ListNode GetIntersectionNode(ListNode headA, ListNode headB) {
        ListNode boy=headA;
        ListNode girl=headB;
        //如果没有相遇，两个人都继续往前走
        while(boy!=girl)
        {
            //如果走到路的尽头还没有与ta相遇，就再走一次ta走过的路，这样，两个人走过的路程就相等了
            //如果有交集，就必将在交叉路口相遇
            boy=(boy==null)?headB:boy.next;
            girl=(girl==null)?headA:girl.next;
        }
        return boy;
    }
}
```

#### 剑指 Offer 57. 和为s的两个数字

解题思路

```
【双指针法】
左指针指向开头
右指针指向结尾
如果左右之和==目标值，则返回结果
因为这是一个升序列表，考虑到这个特性
如果和大于目标值，就让大的数小点，即right左移
如果和小于目标值，就让小的数大点，即left右移
```

代码

```
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        int n=nums.Length;
        int left=0;
        int right=n-1;
        int sum=0;
        while(left<right)
        {
            sum=nums[left]+nums[right];
            if(sum==target)
                return new int[]{nums[left],nums[right]};
            else if(sum>target)
                right--;
            else
                left++;
        }
        return new int[]{0,0};
    }
}
```

#### 剑指 Offer 21. 调整数组顺序使奇数位于偶数前面

暴力法

先将奇数加入列表
再将偶数加入列表
最后把列表转换为数组

```
public class Solution {
    public int[] Exchange(int[] nums) {
        List<int> list=new List<int>();
        foreach(var i in nums)
        {
            if(i%2==1)
                list.Add(i);
        }
        foreach(var i in nums)
        {
            if(i%2==0)
                list.Add(i);
        }
        return list.ToArray();
    }
}
```

双指针法

```
【思路】
一个指针保存插入位置
一个指针寻找奇数
如果找到奇数，就将奇数与插入位置的数交换
【语法】
返回一个空数组
if(nums.Length==0) return new int[]{};
【ref 传引用】
调用函数，传入实参时要加ref
Swap(ref nums[insertPosition],ref nums[oddFinder]);
在实现函数时，在形参的前面要加ref
```



```
void Swap(ref int i,ref int j)
{
    int temp=i;
    i=j;
    j=temp;
}
public class Solution {
    public int[] Exchange(int[] nums) {
        if(nums.Length==0) return new int[]{};
        int insertPosition=0;
        int oddFinder=0;
        while(oddFinder!=nums.Length)
        {
            if(nums[oddFinder]%2==1)
            {
                Swap(ref nums[insertPosition],ref nums[oddFinder]);
                insertPosition++;
            }
            oddFinder++;
        }
        return nums;
    }

    void Swap(ref int i,ref int j)
    {
        int temp=i;
        i=j;
        j=temp;
    }
}
```

#### 剑指 Offer 58 - I. 翻转单词顺序

解题思路

```
使用Split函数，对字符串以空格为间隔切片，切片后不保留空元素
将切片的结果存入栈内
然后依次弹出，加到字符串的尾部，并补上空格
【语法】
Split函数，切片后不保留空元素
string[] sa=s.Split(' ',StringSplitOptions.RemoveEmptyEntries);
注意，是以字符切片，所以用单引号
【字符串相加】
字符串可以相加，但不能对其中的字符进行修改，因为字符串是只读的
相加的过程，实际上是新创建了一个字符串
```

代码

```
public class Solution {
    public string ReverseWords(string s) {
        string[] sa=s.Split(' ',StringSplitOptions.RemoveEmptyEntries);
        Stack<string> ss=new Stack<string>();
        string res="";

        foreach(var i in sa)
        {
            ss.Push(i);
        }

        while(ss.Count>0)
        {
            res=res+ss.Pop();
            if(ss.Count>0)
                res=res+" ";
        }

        return res;
    }
}
```

#### 剑指 Offer 35. 复杂链表的复制

解题思路

待完善

错解

```
public class Solution {
    public Node CopyRandomList(Node head) 
    {
        if (head == null) 
        {
            return head;
        }

        // 先复制链表的节点，只复制val、next
        Node p=head;
        Node r=new Node(0);
        Node h=r;
        while (p!=null) 
        {
            //值是可以直接拷贝的
            r.next=new Node(p.val);
            //next不用拷贝，按遍历顺序生成就可以了
            r=r.next;
            //random无法直接拷贝，因为拷贝的值是原节点指向的原链表的地址，并非新链表的地址，所以一次遍历是无法实现的
            //如果拷贝的节点在原节点后面，就可以通过原节点找到该随机指针的位置了
            r.random=p.random;
            p=p.next;
        }

        return h.next;
    }
}
```

代码

```
public class Solution {
    public Node CopyRandomList(Node head) 
    {
        if (head == null) 
        {
            return head;
        }

        // 完成链表节点的复制
        Node p = head;
        while (p != null) 
        {
            Node copyNode = new Node(p.val);
            copyNode.next = p.next;
            p.next = copyNode;
            p = p.next.next;
        }

        // 完成链表复制节点的随机指针复制
        p = head;
        while (p != null) 
        {
            if (p.random != null) 
            { // 注意判断原来的节点有没有random指针
                p.next.random = p.random.next;
            }
            p = p.next.next;
        }

        // 将链表一分为二
        Node copyHead = head.next;
        p = head;
        Node curCopy = head.next;
        while (p != null) 
        {
            p.next = p.next.next;
            p = p.next;
            if (curCopy.next != null) 
            {
                curCopy.next = curCopy.next.next;
                curCopy = curCopy.next;
            }
        }
        return copyHead;
    }
}
```