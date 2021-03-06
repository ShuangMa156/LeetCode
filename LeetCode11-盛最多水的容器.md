# LeetCode11-盛最多水的容器

## 题目描述

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。

示例 1：

![image-20220130125204979](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220130125204979.png)

输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
示例 2：

输入：height = [1,1]
输出：1
示例 3：

输入：height = [4,3,2,1,4]
输出：16
示例 4：

输入：height = [1,2,1]
输出：2


提示：

n == height.length
2 <= n <= 10^5
0 <= height[i] <= 10^4

## 题解

### 补充知识点：三目运算符？

①条件运算符？的使用格式：

```
测试表达式 ? 条件为true时的表达式 : 条件为false时的表达式表达式 ;
```

### 方法一：利用双指针控制容器高度的选择

（1）方法介绍：

①基本思想：移动指针，选择合适的容器边界，使构成的面积最大

利用双指针分别指示容器的左右两边，则容器的盛水量由容器的两侧的最低高度和水平宽度的乘积决定，适当地选择容器边界，使构成的容器的面积最大。

②容量计算：容器的宽度*容器的高度

容器的宽度：由容器左右两边相隔的距离决定

容器的高度：由容器左右两边高度的较小值决定（木桶的短板效应）

容器高度的选择：由指示容器左边界的左指针和指示容器右边界的右指针指示容器左右的高度，通过比较选择合适的容器的边界

③指针的初始状态：保证影响盛水量的其中一个因素------容器的宽度（即两个指针之间的距离）值最大。

左指针：指向数组中的第一个元素表示的容器的高度

右指针：指向数组中的最后一个元素表示的容器的高度

④指针移动的条件：移动双指针中指示的高度值较小者

左指针的移动方向：向右移动

右指针的移动方向：向左移动

由于指针的移动的过程中，左右指针之间的距离在逐渐减小（即容器的宽度在逐渐减小），如果保证得到更大的容器容量，则容器的高度需要增大。由于容器的高度取自容器左右比边界高度的较小值，所以只有边界的较小值改变，容器的高度才能发生变化。

指针移动：左右指针指示的高度较小者移动（左指针右移，右指针左移）

移动条件：该指针指示的高度值小于另一指针指示的高度值

⑤边界条件：左右指针能构成容器，即左指针指示的位置小于右指针指示的位置

⑥返回值：通过指针移动过程中计算对最大盛水量的计算，保留最大盛水量并返回

（2）具体实现：

```
class Solution {
public:
    int maxArea(vector<int>& height)
    {
        int left=0;//记录左指针的位置，并初始化为指向数组中的第一个元素所在的位置
        int right=height.size()-1;//记录右指针的位置，并初始化指向数组中最后一个元素所在的位置
        int maxArea=0;//记录容器的最大容量，初始化为0
        int width=0;//记录容器的高度
        int length=0;//记录容器的宽度
        while(left<right)
        {
           //width=min(height[left],height[right]);//容器的高度为左右指针指示的容器的左右边界中的较小值
           width=(height[left] < height[right]) ? height[left] : height[right];//通过三目运算符？比较得到容器最优边界的较小者最为容器的高度
           length=right-left;//容器的宽度为容器左右边界间的距离
          // maxArea=max(maxArea,length*width);//计算当前左右指针指示的容器边界构成的容器的盛水量，并与之前的最大盛水量比较，保留值较大者
           int area=width*length;//计算当前左右指针指示的容器边界构成的容器的盛水量
           maxArea=(maxArea < area) ?  area: maxArea;//与之前的最大盛水量比较，保留值较大者
           if(height[left]<height[right]) left++;//如果左指针指示的值较小，则向右移动左指针
           else right--;//如果右指针指示的值较小，则向左移动右指针
        }
        return maxArea;//返回最终的最大盛水量
    }
};
```

（3）运行结果：

![image-20220130151804114](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220130151804114.png)