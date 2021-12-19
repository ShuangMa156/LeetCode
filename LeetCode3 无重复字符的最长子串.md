# LeetCode3 无重复字符的最长子串

## 一、C++中的String标准模板库

1、字符串的构造

```
string str：生成空字符串

string s(str)：生成字符串为str的复制品

string s(str, strbegin,strlen)：将字符串str中从下标strbegin开始、长度为strlen的部分作为字符串初值

string s(cstr, char_len)：以C_string类型cstr的前char_len个字符串作为字符串s的初值

string s(num ,c)：生成num个c字符的字符串

string s(str, stridx)：将字符串str中从下标stridx开始到字符串结束的位置作为字符串初值
```

2、字符串的大小和容量

```
（1）size()和length()：返回string对象的字符个数，他们执行效果相同。

（2）max_size()：返回string对象最多包含的字符数，超出会抛出length_error异常

（3） capacity()：重新分配内存之前，string对象能包含的最大字符数
```

3、字符串的比较

```
（1） C ++字符串支持常见的比较操作符（>,>=,<,<=,==,!=），甚至支持string与C-string的比较(如 str<”hello”)。  
在使用>,>=,<,<=这些操作符的时候是根据“当前字符特性”将字符按字典顺序进行逐一地比较，字典排序靠前的字符小，。 
比较的顺序是从前向后比较，遇到不相等的字符就按这个位置上的两个字符的比较结果确定两个字符串的大小(前面减后面)
同时，string (“aaaa”) <string(aaaaa)。    
（2）另一个功能强大的比较函数是成员函数compare()。它支持多参数处理，支持用索引值和长度定位子串来进行比较。 它返回一个整数来表示比较结果，返回值意义如下：0：相等 1：大于 -1：小于 (A的ASCII码是65，a的ASCII码是97)
```

4、字符串的插入

```
字符串对象名称.push_back('a');// 尾插一个字符
insert(pos,char);//在制定的位置pos前插入字符char
```

5、字符串的拼接

```
// 方法一：append()
    string s1("abc");
    s1.append("def");
    cout<<"s1:"<<s1<<endl; // s1:abcdef

    // 方法二：+ 操作符
    string s2 = "abc";
    /*s2 += "def";*/
    string s3 = "def";
    s2 += s3.c_str();
    cout<<"s2:"<<s2<<endl; // s2:abcdef
```

6、字符串的遍历

```
 string s1("abcdef"); // 调用一次构造函数

    // 方法一： 下标法

    for( int i = 0; i < s1.size() ; i++ )
    {
        cout<<s1[i];
    }
    cout<<endl;

    // 方法二：正向迭代器

    string::iterator iter = s1.begin();
    for( ; iter < s1.end() ; iter++)
    {
        cout<<*iter;
    }
    cout<<endl;

    // 方法三：反向迭代器
    string::reverse_iterator riter = s1.rbegin();
    for( ; riter < s1.rend() ; riter++)
    {
        cout<<*riter;
    }
    cout<<endl;
```

7、字符串的删除

```
（1） iterator erase(iterator p);//删除字符串中p所指的字符

（2） iterator erase(iterator first, iterator last);//删除字符串中迭代器

区间[first,last)上所有字符

（3）string& erase(size_t pos = 0, size_t len = npos);//删除字符串中从索引

位置pos开始的len个字符

（4） void clear();//删除字符串中所有字符
```

8、字符串的替换

```
（1） string& replace(size_t pos, size_t n, const char *s);//将当前字符串从pos索引开始的n个字符，替换成字符串s
（2） string& replace(size_t pos, size_t n, size_t n1, char c); //将当前字符串从pos索引开始的n个字符，替换成n1个字符c
（3）string& replace(iterator i1, iterator i2, const char* s);//将当前字符串[i1,i2)区间中的字符串替换为字符串s
```

9、字符串的查找

```
（1） size_t find (const char* s, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找子串s，返回找到的位置索引， -1表示查找不到子串

（2）size_t find (charc, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找字符c，返回找到的位置索引，-1表示查找不到字符

（3）size_t rfind (const char* s, size_t pos = npos) const;

  //在当前字符串的pos索引位置开始，反向查找子串s，返回找到的位置索引， -1表示查找不到子串

（4） size_t rfind (char c, size_t pos = npos) const;

  //在当前字符串的pos索引位置开始，反向查找字符c，返回找到的位置索引，-1表示查找不到字符

（5）size_t find_first_of (const char* s, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找子串s的字符，返回找到的位置索引，-1表示查找不到字符

（6）size_t find_first_not_of (const char* s, size_t pos = 0) const;

  //在当前字符串的pos索引位置开始，查找第一个不位于子串s的字符，返回找到的位置索引，-1表示查找不到字符

（7）size_t find_last_of(const char* s, size_t pos = npos) const;

  //在当前字符串的pos索引位置开始，查找最后一个位于子串s的字符，返回找到的位置索引，-1表示查找不到字符

（8） size_tfind_last_not_of (const char* s, size_t pos = npos) const;

 //在当前字符串的pos索引位置开始，查找最后一个不位于子串s的字符，返回找到的位置索引，-1表示查找不到子串
```

10、字符串的排序

```
sort(s.begin(),s.end());
```

11、字符串的分割

```
strtok() & substr()
示例：
    方式一：
    char str[] = "I,am,a,student; hello world!";
    const char *split = ",; !";
    char *p2 = strtok(str,split);
    while( p2 != NULL )
    {
        cout<<p2<<endl;
        p2 = strtok(NULL,split);
    }
    方式二：
    string s1("0123456789");
    string s2 = s1.substr(2,5); // 结果：23456-----参数5表示：截取的字符串的长度
    cout<<s2<<endl;
```



## 二、解题

1、自己第一次写完，未通过测试

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        string a;
        bool flag=false;
        a.push_back(s[0]);
        for(int i=0;i<s.size()-1;i++)
        {
            for(int j=0;j<a.size();j++)
            {
                if(s[i]!=a[j])
                {
                    flag=true;
                }
                a.erase(0,1);
            }
             if(flag)
             {
                 a.push_back(s[i]);
             }
        }
        return a.size();
    }
};
```

2、参考网上的解题思路，用滑动窗口完成字符串的比较

（1）滑动窗口方法：通过不断地调整不重复子串的边界（左边界和右边界）找到最长的无重复子串

原理简介：**left 到 right 这个区间形成一个滑动窗口**，为了寻找满足条件的子串，窗口不停地在向前滑动，记录子串的长度是否是更长的子串

具体操作：

①初始化滑动窗口

假设已经找到一个不含重复字符子串 s[left...right]，s[left...right] 表示从字符串 s 的下标 left 到 right 的子串。

![image-20211219151244662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219151244662.png)

②寻找方式

子串数组的右边界 right 向右移，拓展子串长度，以寻找**最长子串**。

![image-20211219151333438](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219151333438.png)

③对字符串中的每一个字符依次判断，并移动滑动窗口

无重复时，右边界后移：将字符 s[right + 1] 跟子串 s[left...right] 中的每个字符进行比较，如果都不同，则将字符 s[right + 1] 也纳入到子串中。

![image-20211219151413039](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219151413039.png)

④有重复时，左边界后移

如果字符 s[right + 1] 跟子串 s[left...right] 中的某个字符相同，则将子串数组的左边界 left 右移，**刨除** s[left...right] 中的那个**重复的字符**；

![image-20211219151439343](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219151439343.png)

有无重复字符的判断：

当滑动窗口的右边界 right 向右拓展时，其对应的字符和当前找到的子串中有无重复字符的判断：设置一个数组记录 ASCII 码出现的频率，这样当 right 向右拓展时，就可以查找其对应的字符对应的 ASCII 码在该数组中相应的频率值的多少判断是否出现了重复字符。

方式一：

初始时右边界不在字符串中，每次判断的是right后面的那个字符与窗口中的字符是否重复

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int left=0;//初始化左边界指向字符串中的第一个元素
        int right=-1;//初始化右边界不在字符串中
        int length=s.size();//保存字符串的长度
        int ASC_Fre[128]={0};//初始化每个字符串所对应的ASCII码出现的次数均为0
        int result=0;//初始化保存最长子串长度结果的变量result的值为0
        while(left<length)//当串口仍在字符串中时（窗口的左边界未到达字符串的末尾）
        {
            if(right+1<length && ASC_Fre[s[right+1]]==0)//如果窗口的右边界也未到达字符串的末尾，并且滑动窗口后边的第一个字符在字符串中且在前面未出现过（对应的频次仍为0）
            {
                ASC_Fre[s[++right]]++;//则将滑动窗口的右边界后移一位，并该字符对应的ASCII码频次加1
            }
            else//否则将滑动窗口的左边界后移，并将该字符的频次减1
            {
                ASC_Fre[s[left++]]--;
            }
            result=max(result,right-left+1);//计算当前滑动窗口的大小（right-left+1），并与result的值比较，将较大的值赋给result变量
        }
        return result;//返回结果
    }
};
```

方式二：

不同之处：初始时右边界的位置不同，此时left和right初始时均指向字符串开头的位置，每次判断的是右边界指示的位置，其实right指向的是窗口外的第一个元素

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int left=0;
        int right=left;
        int length=s.size();
        int ASC_Fre[128]={0};
        int result=0;
        while(left<length)
        {
            if(right<length && ASC_Fre[s[right]]==0)
            {
                ASC_Fre[s[right++]]++;
            }
            else
            {
                ASC_Fre[s[left++]]--;
            }
            result=max(result,right-left);//当前窗口的大小为(right-left)的值
        }
        return result;
    }
};
```

运行结果：

![image-20211219152044641](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219152044641.png)

（2）用双指针维护滑动窗口的大小

方法简介：利用双指针p1,p2，p1记录滑动窗口的左边界对应的字符在字符串中的位置，p2记录滑动窗口右边界的后一个元素在字符串中的位置，维护滑动窗口的变化。初始时，p1=p2，均指向字符串开始的位置。循环中，p2不断向后移动，窗口的长度变长，而p1则根据窗口内是否有重复字符而控制窗口的长度。

![image-20211219172045220](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219172045220.png)

滑动窗口本质上就是一种枚举方式，枚举对于p2的各种取值，子串所能取到的最大长度，但是这种枚举前后有相关性，因为我们只用考虑窗口元素的“进去”，大大减少了枚举次数。

具体实现：

①重复字符的判断：创建一个哈希表，记录各个字符最近一次出现的下标。初始时，字符都未出现，所以下标都置为0，那么这也意味着我们记录的下标要从1开始。

②窗口的变化：

ⅰ重复字符在窗口外，窗口大小不发生改变

如果当前p2指针指向的字符串中的元素对应的字符最近一次出现的下标小于p1指向的元素的下标，则不用修改p1(即如果hash[s[p2]]小于p1,也就意味着hash[s[p2]]在我们的窗口之外，我们不需要对维护窗口的左指针p1进行修改)

![image-20211219165402038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219165402038.png)

ⅱ重复字符在窗口内，窗口缩小

如果当前p2指针指向的字符串中的元素对应的字符最近一次出现的下标大于p1指向的元素的下标，则需要修改p1（即如果hash[s[p2]]大于p1,也就意味着hashs[p2]在我们的窗口内，那我们需要对我们的窗口长度发生截断，同时更新p1的值。）

![image-20211219165508238](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219165508238.png)

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int result=0;//记录最后的结果，即字符串中最长子串的长度
        int p1=0;//p1指示滑动窗口的左边界，初始时指向字符串开头的第一个字符
        int p2=0;//p2指示活动窗口的右边界，初始时指向字符创开头的第一个字符
        int HashChar[128]={0};//创建存储每个字符最近一次在字符串中访问到的位置，设置数组的容量为128,是因为计算机中每个字符是用8位二进制的ASCII码表示的，实际使用的是7为二进制表示的127个字符
        while(s[p2])//当p2指示的位置仍在字符串中，即p2指示的字符存在（为那127个字符）
        {
            if(HashChar[s[p2]] && HashChar[s[p2]]>p1)//如果p2指示的字符最近一次出现在前面遍历过的字符中并且在窗口中（即p2指示字符在哈希表中的映射（最近一次出现在字符串中的下标）值大于p1指向的字符串中的位置）
            {
                p1=HashChar[s[p2]];//则需要更改窗口的左边界的位置为p2指示的字符最近一次在字符串中出现的位置
            }
            HashChar[s[p2]]=p2+1;//更新p2指示的字符在哈希表中的映射为最近出现一次出现的位置（当前p2指示的位置的下一个，即跨过那个重复的字符）
            result=max(result,p2-p1+1);//更新当前滑动窗口的长度
            p2++;//滑动窗口的右边界后移
        }
        return result;//返回最终结果，滑动滑动窗口的大小
    }
};
```

运行结果：

![image-20211219172457134](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211219172457134.png)

