# LeetCode4-寻找两个正序数组的中位数

## 知识点补充

1、算法的时间复杂度比较

O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n)

## 解题

题目描述：

给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。

算法的时间复杂度应该为 O(log (m+n)) 。

```
示例 1：
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
示例 2：
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
示例 3：
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
示例 4：
输入：nums1 = [], nums2 = [1]
输出：1.00000
示例 5：
输入：nums1 = [2], nums2 = []
输出：2.00000

提示：
nums1.length == m
nums2.length == n
0 <= m <= 1000
0 <= n <= 1000
1 <= m + n <= 2000
```

1、自己解题

结果：超出时间限制

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
        int mid1;
        int mid2;
        int m=nums1.size();
        int n=nums2.size();
        int left1=0;
        int right1=m-1;
        int left2=0;
        int right2=n-1;
        while(left1<=right1 && left2<=right2)
        {
            mid1=(left1+right1)/2;
            mid2=(left2+right2)/2;
            if(nums1[mid1]>nums2[mid2])
            {
                right1=mid1;
                left2=mid2;
            }
            else if(nums1[mid1]<nums2[mid2])
            {
                left1=mid1;
                right2=mid2;
            }
            else
              return mid1;
        }
        return nums1[mid1];
    }
};
```

### 借鉴他人的二分查找加划分数组法求解

1、解题思想

由于算法要求的时间复杂度为 O(log(m+n))，所以不能简单的通过合并两个有序数组后再求中位数解题，必须直接寻找中位数。所以采用二分查找的方法，对待排序的两个数组进行分割为两个长度相等的两部分，其中一个部分的元素总是大于另一个部分的元素。因为两个数组的元素数量之和有奇数和偶数两种情况，为了避免对两种情况的分类讨论，所以将数组的长度扩展为原来的两倍，全部当做偶数情况讨论。然后根据划分的两部分，是左半部分的最大数值小于右半部分的最小数值，并且两个数组相互之间也要满足此关系（即第一个数组分割线的左半部分的最大值小于第二个数组的右半部分的最小值，第二个数组分割线的左半部分的最大值小于第二个数组的右半部分的最小值），则分割线左半部分的最大值和右半部分的最小值的中位数为两个数组所有元素的中位数。

2、详细介绍

（1）切割数组：切割可以发生在一个数上---元素个数为奇数，切割可以发生在两个数中间（中间两个数的平均值）-----元素个数为偶数

①对于一个有序数组A,如果在第K个位置切割一下(切割不是发生在两数中间），那么左半部分的最大值LeftMax=右半部分的最小值RightMin=数组中的第K个数A[K]

②对于两个有序数组合并为一个有序数组后求第K个位置上的元素

设：Ci为第i个数组的切割的位置（i=1、2），LeftMaxi为第i个数组切割后的左半部分的最大值，RightMin为第i个数组切割后的右半部分的最小值。

![image-20211226194256341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211226194256341.png)

因为数组都是有序的，所以LeftMax1<=RightMin1,LeftMax2<=RightMin2

如果切割发生在某个数上，LeftMax1=RightMin1，LeftMax2=RightMin2

（2）奇偶处理

两个数组合并之后，元素个数可能是奇数，也可能是偶数，所以当元素均为偶数时不用分情况考虑。

方法：通过在数组中每个元素的前面加入虚拟的占位符'#'（虚拟机加，实际上不存在），将数组1的长度由m变为2m+1,将数组2的长度由n变为2n+1，两个数组的元素个数之和就变成了2m+2n+2,恒为偶数。

**加入虚拟元素之后，每个数组的元素扩展为原来的两倍，每个实际位置的元素可以通过现在的位置/2得到原来元素的位置。**

![image-20211226203456557](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211226203456557.png)

比如 2，原来在0位，现在是1位，1/2=0

比如 3，原来在1位，现在是3位，3/2=1

比如 5，原来在2位，现在是5位，5/2=2

比如 9，原来在3位，现在是7位，7/2=3

而对于切割，如果切割发生在‘#’上等于割在2个元素之间，切割发生在数字上等于把数字划到2个部分，总是有以下成立

**LeftMaxi = (Ci-1)/2 位置上的元素**
**RightMini = Ci/2 位置上的元素**

切割在3上，C = 3，LeftMax1=a[(3-1)/2]=A[1]，RightMin1=a[3/2] =A[1]，刚好都是3的位置！

切割在4/7之间‘#’，C = 4，LeftMax2=A[(4-1)/2]=A[1]=4 ，RightMin2=A[4/2]=A[2]=7

把2个数组看做一个虚拟的数组A，A有2m+2n+2个元素，割在m+n+1处，所以我们只需找到m+n+1位置的元素和m+n+2位置的元素就行了。

左边：A[m+n+1] = Max(LeftMax1,LeftMax2)

右边：A[m+n+2] = Min(RightMin1,RightMin2)

得到的中位数：Mid = (A[m+n+1]+A[m+n+2])/2 = (Max(LeftMax1,LeftMax2) + Min(RightMin1,RightMin2) )/2

（3）有序处理

切割后的有序状态为：两个数组左半部分的元素数量之和等于右半部分的元素数量之和，且左半部分的所有元素小于右半部分的所有元素。所以，第一个数组左半部分的最大值小于第二个数组右半部分的最小值，并且第二个数组左半部分的最大值小于第一个数组右半部分的最小值。

左半部分最大值：max(LeftMax1,LeftMax2)

右半部分最小值：min(RightMIn1,RightMin2)

如果左半部分所有元素的个数为k,那么第k个元素就是Max(LeftMax1,LeftMax2)



![image-20211226200354259](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211226200354259.png)

需要调整的状态：

①LeftMax1>RightMin2:说明数组1的左边元素太大，把对数组1切割的位置左移，即C1减小，使LeftMax1的值减小，则C2=K-C1也就相应的增大，使RightMin2增大

②LeftMax2>RightMin1:说明数组2的左边元素太大，把对数组2切割的位置左移，即C2减小，使LeftMAx2的值减小，则C1=K-C2也就相应增大了，使RightMin1增大

（4）如何切割：使用二分法完成数组的切割

由于数组给定后，元素的个数也就确定了。所以只需要确定一个数组中的切割位置，就可根据总的切割数据的数量计算出另一个数组的切割位置。为了提高效率，选择对两个数组中长度较短的一个进行切割。

假设用C1表示长度较短的数组中的切割位置，用C2表示长度较长的那个数组中的切割位置。

①当LeftMax1>RightMin2，则将C1向左二分，使C1减小，C2增大

②当LeftMax2<RightMin1，则将C1向右二分，使C1增大，C2减小

③当有个数组完全大于或小于中值，则可能会出现4种情况（假设n<m）

C1 = 0 —— 数组1整体都在右边了，所有元素都比中值大，中值在数组2中，数组1切割后的左边是空了，所以可以假定LeftMax1 = INT_MIN

C1 =2n —— 数组1整体都在左边了，所有元素都比中值小，中值在数组2中 ，数组1切割后的右边是空了，所以可以假定RightMin1= INT_MAX，来保证LMax2<RMin1恒成立

C2 = 0 —— 数组2整体在右边了，所有元素都比中值大，中值在数组1中 ，数组2切割后的左边是空了，所以可以假定LMax2 = INT_MIN

C2 = 2m —— 数组2整体在左边了，所有元都比中值小，中值在数组1中,数组2切割后的右边是空了，为了让LMax1 < RMin2 恒成立，可以假定RMin2 = INT_MAX

3、具体代码

（1）第一次尝试

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
       int m=nums1.size();//计算数组nums1的长度
       int n=nums2.size();//计算数组nums2的长度
       if(m>n)//当数组nums1的长度大于数组nums的长度时，通过递归调用函数和参数的传递方式，交换两个数组，保证数组nums1的长度是二者中较小的那个
       {
           return findMedianSortedArrays(nums2,nums1);//保证数组nums1的长度小于数组nums2的长度
       }
       //求割
       int LeftMax1,LeftMax2;//定义切割后左半部分的最大元素
       int RightMin1,RightMin2;//定义切割后右半部分的最小元素
       int C1,C2;//定义对两个元素切割的位置
       int Low=0;//定义二分法左边的位置，数组1中第一个元素所在的位置
       int High=2*m;//定义二分法右边的位置，虚拟扩大两倍后，数组1的最后一个元素所在的位置，即为数组1的最大长度的两倍所在的位置
       //二分法分割两个数组
       while(Low<=High)//当对数组1的二分过程未结束时，即数组1可二分
       {
           C1=(Low+High)/2;//定义对数组1切割的位置(切割发生在两个数中间)为数组1中的中位数所在的位置（此时数组1的长度为2*m）
           //由于数组长度扩展后的容量为（2m+2n+2），所以分割为两部分后左半部分和右半部分各有（m+n+1）个元素
           C2=m+n-C1;//定义对数组2切割的位置（根据元素分割的总数与数组1切割的位置选定数组2切割的位置）
           //对数组1的切割位置进行判断
           if(C1==0)//如果整个数组1均被切割为右半部分，则将其左半部分的最大值定义为整数最小值
               LeftMax1=INT_MIN;
           else//否则数组1左半部分的最大值为切割位置左边第一个元素
               LeftMax1=nums1[(C1-1)/2];//由于数组扩展后原来数组中位置为i的元素的位置变为2i,所以现在数组中(C1-1)位置上对应的原数组中位置（C1-1)/2
           if(C1==2*m)//如果整个数组1均被切割为左半部分，则将其右半部分的最小值定义为整数最大值
               RightMin1=INT_MAX;
           else//否则数组1右半部分的最小值为切割位置右边的第一个元素
               RightMin1=nums1[C1/2];//由于数组扩展后原来数组中位置为i的元素的位置变为2i,所以现在数组中C1位置上对应的原数组中位置C1(切割发生在两个数的中间)
           if(C2==0)//如果整个数组2均被切割为右半部分，则将其左半部分的最大值定义为整数最小值
               LeftMax2=INT_MIN;
           else//否则数组1左半部分的最大值为切割位置左边第一个元素
               LeftMax2=nums2[(C2-1)/2];//由于数组扩展后原来数组中位置为i的元素的位置变为2i,所以现在数组中(C1-1)位置上对应的原数组中位置（C1-1)/2
           if(C2==2*n)//如果整个数组2均被切割为左半部分，则将其右半部分的最小值定义为整数最大值
               RightMin2=INT_MAX;
           else
                RightMin2=nums2[C2/2];//由于数组扩展后原来数组中位置为i的元素的位置变为2i,所以现在数组中C1位置上对应的原数组中位置C1(切割发生在两个数的中间)
           if(LeftMax1>RightMin2)//如果数组1的左半部分的最大值大于数组2右半部分的最小值，则将二分的范围向左缩减，对数组1的左半部分进行二分
                High=C1-1;//将二分的最大位置缩小
           else if(LeftMax2>RightMin1)//如果数组2的左半部分的最大值大于数组1的右半部分的最小值，则将二分的范围向右缩减，即对数组1的右半部分进行二分
                Low=C1+1;//将二分的最小位置扩大
           else
                break;//其他满足有序状态的条件下直接跳出循环，不再进行二分
       }
       double result=(max(LeftMax1,LeftMax2)+min(RightMin1,RightMin2))/2.0;//计算划分后的整个数组的中位数（因为数组长度为偶数，所以中位数为大值和右半部分最小值的平均值）
       return result;//返回计算的中位数的结果
    }
};
```

![image-20211226170622781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211226170622781.png)

（2）修改

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
       int m=nums1.size();
       int n=nums2.size();
       if(m>n)
       {
           return findMedianSortedArrays(nums2,nums1);//保证数组nums1的长度小于数组nums2的长度
       }
       //求割
       int LeftMax1,LeftMax2;
       int RightMin1,RightMin2;
       int C1,C2;
       int Low=0;
       int High=2*m;//最大长度的两倍
       //二分法分割两个数组
       while(Low<=High)
       {
           C1=(Low+High)/2;
           C2=m+n-C1;
           //将切割后左右部分的分情况讨论部分的代码进行缩减
           LeftMax1=(C1==0) ? INT_MIN:nums1[(C1-1)/2];
           RightMin1=(C1==2*m) ? INT_MAX:nums1[C1/2];
           LeftMax2=(C2==0) ? INT_MIN:nums2[(C2-1)/2];
           RightMin2=(C2==2*n) ? INT_MAX:nums2[C2/2];
           if(LeftMax1>RightMin2)
                High=C1-1;
           else if(LeftMax2>RightMin1)
                Low=C1+1;
           else
                break;
       }
       //直接返回计算结果，不再创建一个变量保存计算结果
       return (max(LeftMax1,LeftMax2)+min(RightMin1,RightMin2))/2.0;
    }
};
```

![image-20211226171107115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211226171107115.png)

（3）优化部分代码

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
       //先对两个数组的长度进行判断，对数组1长度大于数组2长度的情况进行处理，将数组1和数组2进行交换
       if(nums1.size()>nums2.size())//用两个向量的交换代替函数的递归调用
       {
           vector<int> temp=nums1;//创建临时整型向量最为交换时的中间向量
           nums1=nums2;
           nums2=temp;
       }
       int m=nums1.size();//计算数组1的长度
       int n=nums2.size();//计算数组2的长度
       //求割
       int LeftMax1,LeftMax2;
       int RightMin1,RightMin2;
       int C1,C2;
       int Low=0;
       int High=2*m;//最大长度的两倍
       //二分法分割两个数组
       while(Low<=High)
       {
           C1=(Low+High)/2;
           C2=m+n-C1;
           LeftMax1=(C1==0) ? INT_MIN:nums1[(C1-1)/2];
           RightMin1=(C1==2*m) ? INT_MAX:nums1[C1/2];
           LeftMax2=(C2==0) ? INT_MIN:nums2[(C2-1)/2];
           RightMin2=(C2==2*n) ? INT_MAX:nums2[C2/2];
           if(LeftMax1>RightMin2)
                High=C1-1;
           else if(LeftMax2>RightMin1)
                Low=C1+1;
           else
                break;
       }
       return (max(LeftMax1,LeftMax2)+min(RightMin1,RightMin2))/2.0;
    }
};
```

![image-20211226171320492](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211226171320492.png)

