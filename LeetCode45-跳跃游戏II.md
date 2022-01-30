# LeetCode45-跳跃游戏II

## 题目描述

给你一个非负整数数组 nums ，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

假设你总是可以到达数组的最后一个位置。

示例 1:

输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
示例 2:

输入: nums = [2,3,0,1,4]
输出: 2


提示:

1 <= nums.length <= 10^4
0 <= nums[i] <= 100

## 题解

### 自己解题：提交时提示溢出

```
class Solution {
public:
    int jump(vector<int>& nums) 
    {
      int step=0;//记录跳跃步数
        int end=nums.size()-1;
        int position=0;
        int max_jump=0;//记录在当前位置可以跳跃到的最远距离
        int next_position=0;
        while(position < end)
        {
            for(int i=1;i<nums[position]+1;i++)
            {
                if(max_jump<nums[position+i])
                {
                    max_jump=nums[position+i];
                    next_position=position+i;
                }
            }
            position=next_position;
            step++;
        }
        return step;
    }
};
```

![image-20220130174018201](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220130174018201.png)

### 方法一：贪心算法选择跳跃到下一次可以跳跃较远的位置

（1）方法描述：

①基本思想：利用贪心算法的思想，每次选择跳跃到在可跳范围内保证下一次跳跃的范围更广的位置。

②贪心性质：用最少的步数从数组的第一个位置跳跃到数组的最后一个位置。

③贪心选择：选择使下一次跳跃最大距离最远的当前可跳跃位置作为本次跳跃的终点。

可跳跃范围：数组中的每个位置i给出了从当前位置可继续跳跃的距离（元素个数）的最大值nums[i]，即可选择的跳跃长度的范围为[ 1,nums[i] ]。

贪心选择：在[ 1, nums[i] ]的范围内，选择对应位置在数组中的值最大者

④边界条件：

ⅰ 最后一次跳跃的位置必须到达数组的最后一个位置

ⅱ 当前可跳跃的位置到达边界时（即为当前位置），从当前位置跳跃到在当前位置选择的最优位置，并给跳跃步数加1



（2）具体实现：

```C++
class Solution {
public:
    int jump(vector<int>& nums) 
    {
        int step=0;//记录跳跃步数
        int end=0;//记录在当前位置可以跳跃到的最远距离
        int max_jump=0;//记录下一次可以跳跃到的最远距离
        for(int i=0;i<nums.size()-1;i++)//到达数组的最后一个位置时不跳跃
        {
            max_jump=max(max_jump,i+nums[i]);//当前的位置加上跳跃的距离为可跳跃的最远距离
            if(i==end)//到达本次跳跃的边界
            {
                step++;//步数加1
                end=max_jump;//更新当前可跳跃的最远距离
            }
        }
        return step;//返回跳跃的步数
    }
};

```

```
class Solution {
public:
    int jump(vector<int>& nums) 
    {
       int step=0;//记录跳跃步数
       int start=0;//记录起跳位置
       int end=1;//记录每次跳跃可到的最远位置
       while(end<nums.size())//当跳跃的最远位置不是数组中的最后一个位置时
       {
           int max_jump=0;//记录可跳跃的最远距离
           for(int i=start;i<end;i++)//从当前起跳位置到结束位置，依次判断本次跳跃的可达范围内跳跃值最大的一个位置
           {
               max_jump=max(max_jump,nums[i]+i);//距起跳位置距离为i的位置的可跳跃距离为nums[i],所以从起跳位置到下一次可跳跃的最远位置为i+nums[i],通过比较，记录可跳跃的最大位置
           }
           start=end;//更新下一次的起跳位置为当前可跳的最远位置
           end=max_jump+1;//更新下一次起跳的最远位置
           step++;//起跳步数加1
       }
       return step;//返回起跳步数
    }
};
```

（3）运行结果：

![image-20220130190316376](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220130190316376.png)

![image-20220130190408534](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220130190408534.png)

