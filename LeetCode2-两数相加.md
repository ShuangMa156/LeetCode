# LeetCode2-两数相加

1、题目描述

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例 1：

输入： l1= [2,4,3],l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.

2、解题代码

方法一：

解题思路：由于参与运算的两个数在链表中的存储是从低位到高位按位存储，输出的结果的存储方式也要求是是从低位到高位按位存储，所以直接从链表的第一个节点开始，按位相加，并将结果保存到创建的用于存储求和结果的链表中的对应节点。

需要注意的问题：

（1）由于最后要返回链表中第一个节点的位置，所以必须需要一个指向头结点的指针用于标记链表开始的位置；

（2）由于链表中每个节点的范围为[0,9]，所以两数按位相加时，还要考虑进位，并且运算完成后还要对进位状态进行判断，如果最高位还有进位，还需要申请一个节点存储最高位的数据；

（3）按位相加的过程中，要对参与运算的两个数对应位上的值分情况讨论，尤其是两个数的位数不相等的情况下；

（4）循环条件也要特别注意（自己刚开始写的时候将条件设置为了对每个节点的next指针域判空，检测结果一直不对，后来发现了问题）;

  (5)结构体的作用域符的问题，当定义为指针变量时用“->”，一般的结构体定义用“.”。

```
 Definition for singly-linked list.
 //链表节点的定义
 * struct ListNode {
 *     int val;//存储节点的数值
 *     ListNode *next;//指向下一个节点
 *     ListNode() : val(0), next(nullptr) {}//节点的无参构造
 *     ListNode(int x) : val(x), next(nullptr) {}//只包含数值部分的节点的有参构造
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}//包含数值和next指针域的节点的有参构造
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) //两数求和
    {
         ListNode* SumHead=new ListNode(0);//定义指向保存求和结果的链表的头结点的指针
         ListNode* CurNode=SumHead;//定义指向处理每一次求和运算的节点指针
         int SumCF=0;//初始化进位为0
         while(l1!=NULL || l2!=NULL )//当两个链表其中一个不为空时
         {
             //将两个多位整数的相加转化为每一位上两个整数的相加运算
             int sum=0;//初始化求和结果为0
             int num1=0;//初始化一个操作数为0
             int num2=0;//初始化另一个操作数为0
             if(l1)//当链表l1中的节点不为空时
             {
                 num1=l1->val;//修改参与运算的操作数的值
                 l1=l1->next;//链表中指向当前节点的指针后移，指向表示下一位的节点
             }
             if(l2)//当链表l2中的节点不为空时
             {
                 num2=l2->val;//修改参与运算的操作数的值
                 l2=l2->next;//链表中指向当前节点的指针后移，指向表示下一位的节点
             }
             sum=num1+num2+SumCF;//对当前的对应的位进行带进位的加法运算
             SumCF=sum/10;//计算运算后产生的进位
             sum=sum%10;//计算进位后该位上的数值
             ListNode* nextNode =new ListNode(sum);//申请一个新的节点用于保存进位后该位上的数值为当前位的数值
             CurNode->next=nextNode;//让当前节点的next指针域指向下一个节点
             CurNode=CurNode->next;//让指向当前节点的指针指向下一个节点
         }
         if(SumCF!=0)//运算完成后对最高位的进位状态进行判断
         {
             //如果最高位产生进位，则需要在申请一个节点用于保存最高位的进位
             CurNode->next=new ListNode(SumCF);
             CurNode= CurNode->next;
             CurNode->next=nullptr;
         }
         return SumHead->next;//返回链表中第一个节点的位置
    }
};

```

运行结果：可以看出虽然时间效率还行，但是消耗的内存空间比较大。

![image-20211212173646476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211212173646476.png)

方法二（借鉴与评论区的高人）：

解题思路：直接将进位与参与运算的两数的对应位进行运算，得到的结果即为求和结果，除以10得到进位值，取余10得到该位的十进制数值，减少了对求和结果的存储。

```
 Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
    {
         ListNode* SumLow=new ListNode(0);
         ListNode* CurNode=SumLow;
         int SumCF=0;
         while(l1!=NULL || l2!=NULL || SumCF!=0 )//三个条件之间是或的关系，只有当两个加数的十进制位均无数值且低位未产生进位时，退出循环，减少了对最高位进位情况的单独考虑
         {
             if(l1)
             {
                 SumCF+=l1->val;//与其中一个数求和
                 l1=l1->next;
             }
             if(l2)
             {
                 SumCF+=l2->val;//再将运算结果与另一个数求和
                 l2=l2->next;
             }
             CurNode->next=new ListNode(SumCF % 10);//保存当前十进制位的运算结果到一个新的节点,并将新节点作为当前节点的下一个节点
             CurNode=CurNode->next;//指向当前节点的指针后移
             SumCF/=10;//计算运算后的进位结果
         }
         return SumLow->next;//返回保存结果的链表中的第一个节点
    }
};
```

运行结果：明显看到节省了0.1MB的内存空间

![image-20211212191538687](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211212191538687.png)