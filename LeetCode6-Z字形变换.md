# Z字形变换

## 一、题目描述

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

P   A   H   N
A P L S I I G
Y   I   R
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);


示例 1：

输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
示例 2：
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
示例 3：

输入：s = "A", numRows = 1
输出："A"


提示：

1 <= s.length <= 1000
s 由英文字母（小写和大写）、',' 和 '.' 组成
1 <= numRows <= 1000

## 二、解题

### 方法一：顺序处理字符，按列排列字符，据行改变方向

![image-20220109162551824](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220109162551824.png)

1、方法描述

（1）基本思想：按顺序迭代字符串，确定字符串中的每个字符在哪一行（确定每个字符的位置）

字符串中的每个字符按行走Z字形，按列排列；当到达最后一行或第一行时，需改变方向，直至完成对字符串中每个字符的操作。

（2）排列规律：字符串按照Z字形排列，行数给定，按列优先排列（当行数递增时，该列中的每行都放置一个字符；当行数递减时，该列中只有一行放置字符）。方向改变发生在行边界处（行数递增的边界为（numRows-1）,行数递减的边界为0）

（3）边界情况：要求划分的行数只需一行，则Z字形变换的结果仍为字符串

（4）方向变换条件：

当前处理的行号为(numRows-1)时：方向变为行数递减

当前处理的行号为0时：方向变为行数递增

注：行数递增的方向和行数递减的方向时交替出现的

（5）处理方式：

注：先处理当前字符，在对行号进行判断

①字符处理：标记当前处理的行号和行数变化的方向，将对应的字符存储到对应的行中。

②方向判断：通过对当前所处理的行号进行判断，根据变换条件，决定是否改变方向。

③结果保存：由于每行保存的字符数量和存储时间都是不确定的，所以将每行的结果分开存储，后面处理的元素直接放到对应行的后面。

④最终结果：利用字符串的拼接，按照行号的顺序，将每行得到的字符串拼接成大小与原字符串相同的字符串。

2、具体实现

```
class Solution {
public:
    string convert(string s, int numRows) 
    {
        if(numRows==1)//只需按Z字形排列成一行时，则字符排列的顺序与原字符串相同
        {
            return s;
        }
        int nRowStr=min(numRows,int(s.size()));//将按Z字形排列的字符串每行当做一个新的字符串，便于最后结果的按行表达，而子字符串的个数为行数和字符串大小两者中的较小者，因为存在字符串中字符个数较少，排不满一列的情况。
        vector<string> rowStr(nRowStr);//创建字符串的数组，用于存储分别每行分配的字符
        int curRow=0;//指示当前操作的行
        int changeDirection=false;//初始时方向为行数递减的方向
        for(int i=0;i<s.size();i++)//对字符串中的每个字符进行处理
        {
            rowStr[curRow]+=s[i];//将当前操作字符放到当前操作行对应的字符串的末尾
            if(curRow==numRows-1 || curRow==0)//判断是否到达边界行，递增边界（numRows-1）,递减边界0
            {
                changeDirection=!changeDirection;//当到达边界时，改变方向
            }
            if(changeDirection)//方向为行数递增的方向，则当前操作行号增加
            {
                curRow++;
            }
            else//方向为行数递减方向，则当前操作行号减少
                curRow--;
        }
        string result;
        for(int j=0;j<nRowStr;j++)//利用字符串的拼接，将得到的每行字符串按行序拼接成完整的字符串
        {
            result+=rowStr[j];
        }
        return result;
    }
};
```

```
//优化版：
class Solution {
public:
    string convert(string s, int numRows) 
    {
        if(numRows==1)//只需按Z字形排列成一行时，则字符排列的顺序与原字符串相同
        {
            return s;
        }
        vector<string> rowStr(min(numRows,int(s.size())));//减少变量的定义
        int curRow=0;
        int changeDirection=false;//初始时方向为行数递减的方向
        for(char c:s)
        {
            rowStr[curRow]+=c;
            if(curRow==numRows-1 || curRow==0)
            {
                changeDirection=!changeDirection;//当到达边界时，改变方向
            }
            curRow+=changeDirection ? 1:-1;
        }
        string result;
        for(string row:rowStr)
        {
            result+=row;
        }
        return result;
    }
};
```

3、运行结果

![image-20220109153206740](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220109153206740.png)

### 方法二：根据排列的数学规律，根据下标按行访问字符

![image-20220109171225506](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220109171225506.png)

![image-20220109201551905](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220109201551905.png)

1、方法描述：

（1）基本思想：根据Z字形排列的结果，得出存在的数学周期规律，根据周期找到每行要存储的元素。根据规律，按行取出对应的元素进行存储。

（2）排列规律：按Z字形排列的结果中，每（numRows-1）的元素排布规律相同（先沿行数递增的方向按列排列，再按行数递减的方向按列排列），即排列2(numRows-1)=2*numRows-2个元素构成一个排列周期，即s[i]与s[i+2(numRows-1)]排列在同一行。

对所有自然数k(k=0,1,2,3,......)

①行号为0的行中：字符位于索引 k(2numRows-2)+0处，同行中相邻字符间的间距为（2numRows-2）

②行号为（numRows-1）的行中：字符位于索引 k(2numRows-2)+(numRows-1)处,同行中相邻字符的间距为（2numRows-2）

③行号为i，i在（0，numRow-1）之间（开区间）的行中(以下两种情况交替出现)：

行数递增方向：字符位于索引 k(2numRows-2)+i  处,同行中距前一个字符的间距为2i

行数递减方向：字符位于索引（k+1）(2numRows-2)-i  处，同行中距前一个字符的间距为（2numRows-2-2i）

（3）边界情况：要求划分的行数只需一行，则Z字形变换的结果仍为字符串

（4）处理方式：

方法一：按字符间距处理

 ①下边界行（行号为0）：相邻字符的间距为（2numRows-2）

 ②上边界行（行号为numRows-1）:相邻字符的间距为（2numRows-2）

 ③中间行（i∈（0，numRow-1））:

​         相邻字符的间距分别为：

​            ⅰ 递增--->递减:（2numRows-2-2i）递增与递减

​            ⅱ 递减--->递增：（2i）=(2numRows-2)-(2numRows-2-2i）

方法二：按周期规律处理

①下边界行（行号为0）：一个周期内只分配一个字符---->相邻字符相距一个周期

 ②上边界行（行号为numRows-1）:一个周期内只分配一个字符---->相邻字符相距一个周期

 ③中间行（i∈（0，numRow-1））:一个周期内分配两个字符----->两个字符的间距为（2numRows-2-2i），相邻周期对应字符（第一个字符--->第一个字符，第二个字符----->第二个字符）的间距也为一个周期

2、具体实现：

```
class Solution {
public:
    string convert(string s, int numRows) 
    {
       if(numRows==1)//边界情况
       {
           return s;
       }
       int n=s.size();//字符串的大小
       int TStr=2*numRows-2;//字符Z字形排布规律中一个周期中的字符个数
       int index=0;//指示字符在字符串中的下标
       int distance=0;//指示同一行字符在原字符串中的间距
       string result;//保存转换结果的字符串
       for(int i=0;i<numRows;i++)//按行分配字符，i指示行号
       {
           index=i;//初始时，字符在字符串中的下标位置与行号相同
           distance=i*2;//同一行中，相邻字符的间距为偶数的倍数
           while(index<n)//从字符串中的所有元素中，挑选属于第i行的字符
           {
               result+=s[index];//将符合要求的元素加入到保存结果的字符串中
               distance=TStr-distance;//计算字符的间距
               index+=(i==0 || i==numRows-1) ? TStr:distance;//（首尾两行的间距为TStr，中间的间距由distance给出）
           }
       }
       return result;
    }
};
```

```
class Solution {
public:
    string convert(string s, int numRows) 
    {
        if(numRows==1)//边界情况处理
        {
            return s;
        }
        string result;
        int n=s.size();//字符串的大小
        int TStr=2*(numRows-1);//字符排布的周期
        for(int i=0;i<numRows;i++)//按行分配字符，i指示行号
        {
            for(int j=0;j+i<n;j+=TStr)//按周期分配一行中的字符，j指示行数递增时时的次序，i+j指示字符在原字符串中的位置
            {
                result+=s[j+i];//行数递增时，将一个周期中该行的第一个字符加到字符串中，相邻周期的第一个字符相距一个周期
                if(i!=0 && i!=numRows-1 && j+TStr-i<n)//中间行
                {
                    result+=s[j+TStr-i];//行数递减时，将一个周期中该行的第二个的字符加入到字符串中，相邻周期的第二个字符相距一个周期
                }
            }
        }
    return result;
    }
};
```

3、运行结果：

![image-20220109194111751](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220109194111751.png)

