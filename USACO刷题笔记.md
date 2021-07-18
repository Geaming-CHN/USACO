# USACO刷题笔记

[TOC]

## 前言

笔者为重庆大学CS专业学生Geaming，小学期刚好刷了一下USACO的题，因为之前并没有接触过，写的代码~~绝对~~可能不是最优的，但个人来说比较好理解，欢迎大家提交新的答案。

## 7-1 1.1.1 你要乘坐的飞碟在这里 (100 分)

一个众所周知的事实,在每一彗星后面是一个不明飞行物 UFO. 这些不明飞行物时常来收集来自在 地球上忠诚的支持者. 不幸地,他们的空间在每次旅行只能带上一群支持者. 他们要做的是用一种 聪明的方案让每一个团体人被彗星带走. 他们为每个彗星起了一个名字,通过这些名字来决定一个 团体是不是特定的彗星带走. 那个相配方案的细节在下面被给出； 你的工作要写一个程序来通过团体的名字和彗星的名字来决定一个组是否应该与在那一颗彗星后 面的不明飞行物搭配. 团体的名字和彗星的名字都以下列各项方式转换成一个数字: 这个后的数字代表名字中所有字 母的信息,"A" 是 1 和 "Z" 是 26. 举例来说,团体 "USACO" 会是 21∗19∗1∗3∗15=17955 . 如果团体的数字 mod 47 等于彗星的数字 mod 47,那么你要告诉这个团体准备好被带走! 写一个程序读入彗星的名字和团体的名字,如果搭配打印"GO"否者打印"STAY" 团体的名字和彗星的名字将会是没有空格或标点的一串大写字母（不超过 6 个字母）, 如：
Input      Output
COMETQ   GO
HVNGAT
ABSTAR    STAY
USACO

### 输入格式:

第 1 行: 彗星的名字（一个长度为1到6的字符串）

第 2 行: 团体的名字（一个长度为1到6的字符串）

### 输出格式:

单独一行包含"STAY"或"GO".

### 输入样例:

在这里给出一组输入。例如：

```in
COMETQ  
HVNGAT
```

### 输出样例:

在这里给出相应的输出。例如：

```out
GO
```

### 答案:

- 使用ASCII码进行计算

```C++
#include <iostream>
#include <string>

using namespace std;

int main()
{
    string S_Comet, S_Group;
    int I_Comet=1, I_Group=1;
    cin >> S_Comet >> S_Group;
    //计算
    for (unsigned int i = 0; i < S_Comet.size();i++)
    {
        I_Comet *= int(S_Comet[i] - 'A' + 1);
    }
    for (unsigned int i = 0; i < S_Group.size();i++)
    {
        I_Group *= int(S_Group[i] - 'A' + 1);
    }
    if (I_Group % 47==I_Comet % 47)
        cout << "GO" << endl;
    else
        cout << "STAY" << endl;
}
```

## 7-2 1.1.2 贪婪的礼物送礼者 (90 分)

对于一群要互送礼物的朋友,你要确定每个人送出的礼物比收到的多多少(and vice versa for those who view gift giving with cynicism). 在这一个问题中,每个人都准备了一些钱来送礼物,而这些钱将会被平均分给那些将收到他的礼物 的人. 然而,在任何一群朋友中,有些人将送出较多的礼物(可能是因为有较多的朋友),有些人有准备了较多的钱. 给出一群朋友, 没有人的名字会长于 14 字符,给出每个人将花在送礼上的钱,和将收到他的礼物 的人的列表, 请确定每个人收到的比送出的钱多的数目.

### 输入格式:

第 1 行:人数 NP,2<=*N**P*<=10

第 2 到 NP+1 行:这 NP 个在组里人的名字 (一个名字一行 )

第 NP＋2 到最后:这里的 NP 段内容是这样组织的:

第一行是将会送出礼物人的名字.

第二行包含两个数字: 第一个是原有的钱的数目（在 0 到 2000 的范围里）,第二个 NGi 是将收到 这个送礼者礼物的人的个数，如果NGi是非零的, 在下面 NGi 行列出礼物的接受者的名字（一个名字一行）。

### 输出格式:

输出 NP 行

每行是一个的名字加上空格再加上收到的比送出的钱多的数目.

对于每一个人,他名字的打印顺序应和他在输入的2到NP＋1行中输入的顺序相同.所有的送礼的钱都是整数.

每个人把相同数目的钱给每位要送礼的朋友,而且尽可能多给,不能给出的钱被送礼者自己保留.

### 输入样例:

在这里给出一组输入。例如：

```in
5
dave
laura
owen
vick
amr
dave
200 3
laura
owen
vick
owen
500 1
dave
amr
150 2
vick
owen
laura
0 2
amr
vick
vick
0 0
```

### 输出样例:

在这里给出相应的输出。例如：

```out
dave 302
laura 66
owen -359
vick 141
amr -150
```

### 答案:

- struct的使用

```C++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

struct Friend
{
    string F_Name;
    int F_Total;
    int F_N;
    int F_Receive = 0;
};

int FIND(vector<Friend>, string);

int main()
{
    int N;
    cin >> N;
    vector<Friend> V_Friends(N);
    //名字输入
    for (int i = 0; i < N;i++)
    {
        cin >> V_Friends[i].F_Name;
    }
    //后续信息输入
    for (int i = 0; i < N;i++)
    {
        string name;
        cin >> name;
        int transfer = 0;
        //输入总金额，分发人数
        cin >> V_Friends[FIND(V_Friends, name)].F_Total >> V_Friends[FIND(V_Friends, name)].F_N;
        V_Friends[FIND(V_Friends, name)].F_Receive -= V_Friends[FIND(V_Friends, name)].F_Total;
        if (V_Friends[FIND(V_Friends, name)].F_N!=0)
            transfer = V_Friends[FIND(V_Friends, name)].F_Total / V_Friends[FIND(V_Friends, name)].F_N;
        //除法时注意0的问题
        else
            transfer = 0;
        V_Friends[FIND(V_Friends, name)].F_Receive += V_Friends[FIND(V_Friends, name)].F_Total - transfer * V_Friends[FIND(V_Friends, name)].F_N;
        for (int j = 0; j < V_Friends[FIND(V_Friends, name)].F_N;j++)
        {
            string Receive_Name;
            cin >> Receive_Name;
            V_Friends[FIND(V_Friends, Receive_Name)].F_Receive += transfer;
        }
    }
    for (int i = 0; i < V_Friends.size();i++)
    {
        cout << V_Friends[i].F_Name << ' ' << V_Friends[i].F_Receive << endl;
    }
}

int FIND(vector<Friend>V_F,string Name)
{
    for (int i = 0; i < V_F.size();i++)
    {
        if (Name==V_F[i].F_Name)
            return i;
    }
    return -1;
}
```

## 7-3 1.1.3 黑色星期五 (80 分)

13 号又是星期五是一个不寻常的日子吗?
13 号在星期五比在其他日少吗?为了回答这个问题,写一个程序来计算在 n 年里 13 日落在星期一,星期二......星期日的次数.这个测试从 1900 年 1 月 1 日到 1900+n-1 年 12 月 31 日.n 是一个非负数且不大于 400.
这里有一些你要知道的: 1900 年 1 月 1日是星期一.
4,6,11 和 9 月有 30天.其他月份除了 2 月有 31 天.闰年 2 月有 29 天,平年 2 月有 28 天.
年份可以被 4 整除的为闰年(1992=4*498 所以 1992 年是闰年,但是 1990 年不是闰年)
以上规则不适合于世纪年.可以被 400 整除的世纪年为闰年,否则为平年.所以,1700,1800,1900 和 2100 年是平年,而 2000 年是闰年.
请不要预先算好数据!

### 输入格式:

一个整数 n.

### 输出格式:

七个在一行且相分开的整数,它们代表 13 日是星期六,星期日,星期一.....星期五的次数.

### 输入样例:

在这里给出一组输入。例如：

```in
20
```

### 输出样例:

在这里给出相应的输出。例如：

```out
36 33 34 33 35 35 34
```

### 答案:

- **暴力求解**

```C++
#include <iostream>

using namespace std;


bool Leap(int);

int main()
{
    int Weeks[7] = {0};
    int Month1[12] = {0,31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30};
    int Month2[12] = {0,31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30};
    int n;
    cin >> n;
    //1900.1.1---1900+n-1.12.31
    int Before_Days = 0;
    int Days = 0;
    for (int i = 0; i < n ; i++)
    {
        int Year = 1900;
        Year += i; //年份计算
        int Month[12] = {0};
        if (Leap(Year))
            for (int i = 0; i < 12;i++)
                Month[i] = Month1[i];
        else
            for (int i = 0; i < 12;i++)
                Month[i] = Month2[i];
        Days += Before_Days;               //之前年份的天数
        for (int j = 0; j < 12;j++)//12个月
        {
            Days += Month[j];
            int Weekday = 0;
            Weekday = (Days + 13)%7;
            Weeks[Weekday]++;
        }
        Days += 31;
    }
    cout << Weeks[6] << ' ' << Weeks[0] << ' ' << Weeks[1] << ' ' << Weeks[2] << ' '
         << Weeks[3] << ' ' << Weeks[4] << ' ' << Weeks[5];
}

bool Leap(int Year)
{
    if (Year % 4 == 0 && Year %100 != 0)
        return true;
    else if (Year%100==0&&Year%400==0)
        return true;
    else
        return false;
}
```

## 7-4 1.1.4 破碎的项链 (80 分)

你有一条由 N 个红色的,白色的,或蓝色的珠子组成的项链(3<=N<=350),珠子是随意安排的. 这里 是 n=29 的二个例子 :

12                      12

r b b r                      b r r b

r    b                    b    b

r      r                  b      r

r       r                 w       r

b        r                w        w

b         b               r         r

b         b               b         b

b         b               r         b

r        r                b        r

b       r                 r       r

b      r                  r      r

r     r                   r     b

r b r                     r r w

图片 A                    图片 B

r 代表 红色的珠子

b 代表 蓝色的珠子

w 代表 白色的珠子

第一和第二个珠子在图片中已经被作记号.

图片 A 中的项链可以用下面的字符串表示:

brbrrrbbbrrrrrbrrbbrbbbbrrrrb .

假如你要在一些点打破项链,展开成一条直线,然后从一端开始收集同颜色的珠子直到你遇到一个 不同的颜色珠子,在另一端做同样的事.(颜色可能与在这之前收集的不同) 确定应该在哪里打破项 链来收集到大多数的数目的子.

Example 举例来说,在图片 A 中的项链,可以收集到 8 个珠子, 在珠子 9 和珠子 10 或珠子 24 和珠子 25 之间打断项链. 在一些项链中,包括白色的珠子如图 片 B 所示. 当收集珠子的时候,一个被遇到的白色珠子可以被当做红色也可以被当做蓝色. 表现项链的字符串将会包括三符号 r , b 和 w .

写一个程序来确定从一条被供应的项链大可以被收集珠子数目.

### 输入格式:

第 1 行: N, 珠子的数目

第 2 行: 一串度为 N 的字符串, 每个字符是 r , b 或 w.

### 输出格式:

单独的一行包含从被供应的项链可以被收集的珠子数目的大值.

### 输入样例:

在这里给出一组输入。例如：

```in
29
wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
```

### 输出样例:

在这里给出相应的输出。例如：

```out
11
```

### 解题思路：

- **提交时有一个点始终没有通过，查询输入输出文件后发现，如果相遇是w的话会重复计算。**

### 答案:

```C++
#include <iostream>
#include <string>

using namespace std;

int Count(string);//计数

int main()
{
    //完成输入
    int N;
    int Max = 0;
    string Necklace;
    cin >> N;
    cin >> Necklace;
    //寻找断点
    for (int i = 0; i < Necklace.size();i++)
    {
        string N_Necklace;
        N_Necklace = Necklace.substr(i, N - i) + Necklace.substr(0, i);
        if (Max < Count(N_Necklace))
            Max = Count(N_Necklace);
    }
    cout << Max;
}

int Count(string S)
{
    char C1 = S[0];
    char C2 = S[S.size() - 1];
    int P_C1 = 0;
    int P_C2 = S.size() - 1;
    while (C1 == 'w')
    {
        P_C1++;
        C1 = S[P_C1];
    }
    while (C2 == 'w')
    {
        P_C2--;
        C2 = S[P_C2];
    }

    int L, R;
    L = R = 0;
    int P_L = 0;
    int P_R = 0;
    for (int i = 0; i < S.size();i++)
    {
        if ((C1 == S[i])||(S[i] == 'w'))
        {
            L++;
            P_L = i;
        }
        else
            break;
    }
    if (P_L !=S.size()-1)
    {
        for (int i = 0; i < S.size() - 1;i++)
        {
            if ((C2 == S[S.size()-i-1])||(S[S.size()-i-1] == 'w'))
            {
                R++;
                P_R = S.size() - i - 1;
            }
            else
                break;
        }
    }

    if((P_R == P_L)&&(S[P_R]=='w')) //最后相遇为w，会重复计算
        return R + L - 1;
    return R + L;
}
```

## 7-5 1.2.1 挤牛奶 (80 分)

三个农民每天清晨 5 点起床,然后去牛棚给 3 头牛挤奶.第一个农民在 300 时刻(从 5 点开始计时, 秒为单位)给他的牛挤奶,一直到 1000 时刻.第二个农民在 700 时刻开始,在 1200 时刻结束.第三个 农民在 1500 时刻开始 2100 时刻结束.期间最长的至少有一个农民在挤奶的连续时间为 900 秒(从 300 时刻到 1200 时刻),而最长的无人挤奶的连续时间(从挤奶开始一直到挤奶结束)为 300 秒(从 1200 时刻到 1500 时刻).

你的任务是编一个程序,读入一个有 N 个农民(1 <= N <= 5000)挤 N 头牛的工作时间列表,计算以下两点(均以秒为单位):

• 长至少有一人在挤奶的时间段.

• 长的无人挤奶的时间段.

### 输入格式:

Line 1: 一个整数 N.

Lines 2..N+1: 每行两个小于 1000000 的非负整数,表示一个农民的开始时刻与结束时刻.

### 输出格式:

一行,两个整数,即题目所要求的两个答案.

### 输入样例:

在这里给出一组输入。例如：

```in
3 
300 1000 
700 1200 
1500 2100
```

### 输出样例:

在这里给出相应的输出。例如：

```out
900 300
```

### 解题思路：

- **数据离散化**
- **两组数据合并**

### 答案:

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
    //输入数据
    int N,Time_0=0,Time_1=0;
    cin >> N;
    vector<int>Begin_Time(N), End_Time(N), All_Time(N*2);
    for (int i = 0; i < N;i++)
    {
        cin >> Begin_Time[i] >> End_Time[i];
        All_Time[2 * i] = Begin_Time[i];
        All_Time[2 * i + 1] = End_Time[i];
    }
    //排序
    sort(All_Time.begin(), All_Time.end()); //从小到大排序
    int sum1 = 0, sum2 = 0;
    for (int i = 1; i < 2 * N;i++)
    {
        int p = 0;//是否有人工作标识
        for (int j = 0; j < N;j++)
        {
            if(All_Time[i]>Begin_Time[j]&&All_Time[i]<=End_Time[j])
            {
                sum1 += All_Time[i] - All_Time[i - 1];
                Time_1 = max(sum1, Time_1);
                sum2 = 0;
                p = 1;
                break;
            }
        }
        if (p==0)
        {
            sum2 += All_Time[i] - All_Time[i - 1];
            Time_0 = max(sum2, Time_0);
            sum1 = 0;
        }
    }
    cout << Time_1 << ' ' << Time_0 << endl;
}
```

## 7-6 1.2.2 方块转换 (70 分)

一块 N x N（1<=N<=10）正方形的黑白瓦片的图案要被转换成新的正方形图案.

写一个程序来找出将原始图案按照以下列转换方法转换成新图案的小方式:

\#1:转 90 度:图案按顺时针转 90 度.

\#2:转 180 度:图案按顺时针转 180 度.

\#3:转 270 度:图案按顺时针转 270 度.

\#4:反射:图案在水平方向翻转（形成原图案的镜像）.

\#5:组合:图案在水平方向翻转,然后按照#1-#3 之一转换.

\#6:不改变:原图案不改变.

\#7:无效转换:无法用以上方法得到新图案. 如果有多种可用的转换方法,请选择序号小的那个.

### 输入格式:

第一行: 单独的一个整数 N.

第二行到第 N+1 行: N 行每行 N 个字符（不是“@”就是“-”）；这是转换前的正方形.

第 N+2 行到第 2*N+1 行: N 行每行 N 个字符（不是“@”就是“-”）；这是转换后的正方形.

### 输出格式:

单独的一行包括1到7之间的一个数字（在上文已描述）表明需要将转换前的正方形变为转换后的正方形的转换方法.

### 输入样例:

在这里给出一组输入。例如：

```in
3 
@-@ 
--- 
@@- 
@-@ 
@-- 
--@
```

### 输出样例:

在这里给出相应的输出。例如：

```out
1
```

### 解题思路：

- **函数应用**
- **二维化成一维**
- **N\*N的下标变化**

### 答案：

```C++
#include <iostream>
#include <vector>

using namespace std;

vector<char> Operation1(vector<char> V_Square, int N);
vector<char> Operation2(vector<char> V_Square, int N);
vector<char> Operation3(vector<char> V_Square, int N);
vector<char> Operation4(vector<char> V_Square, int N);
int main()
{
    int N;
    vector<char> O_Square,N_Square;
    cin >> N;
    //完成输入
    for (int i = 0; i < N * N;i++)
    {
        char c;
        cin >> c;
        O_Square.push_back(c);
    }
    for (int i = 0; i < N * N;i++)
    {
        char c;
        cin >> c;
        N_Square.push_back(c);
    }
    if(N_Square==Operation1(O_Square, N))
        cout << "1" << endl;
    else if(N_Square==Operation1(Operation1(O_Square, N), N))//2*90=180
        cout << "2" << endl;
    else if(N_Square==Operation1(Operation1(Operation1(O_Square, N), N), N))//3*90=270
        cout << "3" << endl;
    else if(N_Square==Operation3(O_Square, N))
        cout << "4" << endl;
    else if(N_Square==Operation3(Operation1(O_Square, N), N))
        cout << "5" << endl;
    else if(N_Square==Operation3(Operation1(Operation1(O_Square, N), N), N))
        cout << "5" << endl;
    else if(N_Square==Operation3(Operation1(Operation1(Operation1(O_Square, N), N), N), N))
        cout << "5" << endl;
    else if (N_Square == O_Square)
        cout << "6" << endl;
    else
        cout << "7" << endl;
}

vector<char> Operation1(vector<char>O_Square,int N)
{
    //123456789->741852963
    vector<char> N_Square;
    vector<int> Pos;
    for (int i = 1; i <= N;i++)
    {
        int P = N * N - N + i;
        while(P!=i)
        {
            Pos.push_back(P);
            P = P - N;
        }
        Pos.push_back(i);
    }
    for (int i = 0; i < Pos.size();i++)
        N_Square.push_back(O_Square[Pos[i]-1]);
    return N_Square;
}
vector<char> Operation3(vector<char>O_Square,int N)
{
    //123456789->741852963
    vector<char> N_Square;
    vector<int> Pos;
    for (int i = 0; i < N;i++)
    {
        int P = N * (i + 1);
        while(P>=N*i+1)
        {
            Pos.push_back(P);
            P = P - 1;
        }
    }
    for (int i = 0; i < Pos.size();i++)
        N_Square.push_back(O_Square[Pos[i]-1]);
    return N_Square;
}
```

## 7-7 1.2.3 删除排序数组中的重复项 (100 分)

给定一个排序数组(**n<=100**)，你需要在**原来数组上**删除重复出现的元素，使得每个元素只出现一次，返回移除后**数组的新长度**。

不要使用额外的数组空间，你必须在 **原数组** 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

### 输入格式:

第一行：数组长度n 第2到n+1行：数组的n项

### 输出格式:

数组的新长度。

### 输入样例:

在这里给出一组输入。例如：

```in
5
1
1
2
4
4
```

### 输出样例:

在这里给出相应的输出。例如：

```out
3
```

### 答案：

```C++
#include <iostream>

using namespace std;

int main()
{
    int N;
    cin >> N;
    int array[N];
    for (int i = 0; i < N;i++)
    {
        cin >> array[i];
    }
    int temp = array[0];
    int len = 1;
    for (int i = 0; i < N;i++)
    {
        if (temp!=array[i])
        {
            temp = array[i];
            len++;
        }
    }
    cout << len << endl;
}
```

## 7-8 1.2.4 回文平方数 (80 分)

回文数是指从左向右念和从右像做念都一样的数.如 12321 就是一个典型的回文数. 给定一个进制 B(2<=B<=20 十进制),输出所有的大于等于1，小于等于 300 且它的平方用 B 进制表示时是回文数的数.用’A’,’B’……表示 10,11 等等.

### 输入格式:

共一行,一个单独的整数 B(B 用十进制表示).

### 输出格式:

每行两个数字,第二个数是第一个数的平方,且第二个数是回文数.(注意:这两个数都应该在 B 那个进制下)

### 输入样例:

在这里给出一组输入。例如：

```in
10 
```

### 输出样例:

在这里给出相应的输出。例如：

```out
1 1
2 4
3 9
11 121
22 484
26 676
101 10201
111 12321
121 14641
202 40804
212 44944
264 69696
```

### 解题思路：

- **进制转换函数**
- **判断是否回文函数**

### 答案：

```C++
#include <iostream>
#include <string>
using namespace std;

string S_Scale(int, int);
bool B_Palindrome(string);

int main()
{
    int N;
    cin >> N;
    for (int i = 1; i <= 300;i++)
    {
        if(B_Palindrome(S_Scale(i*i,N)))
            cout << S_Scale(i,N) << ' ' << S_Scale(i*i, N) << endl;
    }

}

string S_Scale(int d,int N)
{
    int n = d;
    string scale = "";
    while(n>=N)
    {
        if (n%N<10)
            scale = to_string(n % N) + scale;
        else
            scale = char(n%N - 10 + 'A')+scale;
        n = n / N;
    }
    if (n%N<10)
            scale = to_string(n % N) + scale;
    else
        scale = char(n%N - 10 + 'A')+scale;
    return scale;
}

bool B_Palindrome(string S)
{
    for (int i = 0; i < S.size()/2;i++)
    {
        if (S[i]!=S[S.size()-i-1])
            return false;
    }
    return true;
}
```

## 7-9 1.2.5 双重回文数 (70 分)

如果一个数从左往右读和从右往左读都是一样,那么这个数就叫做“回文数”.例如,12321 就是一个回文数,而 77778 就不是.

当然,回文数的首和尾都应是非零的,因此 0220 就不是回文数. 事实上,有一些数（如 21）,在十进制时不是回文数,但在其它进制（如二进制时为 10101）时就是 回文数.

编一个程序,读入两个十进制数 N (1 <= N <= 15) S (0 < S < 10000)

然后找出前 N 个满足大于 S 且在两种以上进制（二进制至十进制）上是回文数的十进制数,输出到 文件上.

本问题的解决方案不需要使用大于 4 字节的整型变量.

### 输入格式:

只有一行,用空格隔开的两个数 N 和 S.

### 输出格式:

N 行, 每行一个满足上述要求的数,并按从小到大的顺序输出.

### 输入样例:

在这里给出一组输入。例如：

```in
3 25
```

### 输出样例:

在这里给出相应的输出。例如：

```out
26
27
28
```

### 解题思路：

- **使用前一个题构造的两个函数**
- **for循环遍历进制，记录满足次数**
- **每次记录次数需要清零**

### 答案

```C++
#include <iostream>
#include <string>
using namespace std;

string S_Scale(int, int);
bool B_Palindrome(string);

struct Num
{
    int N;//数字
    int P_N=0;//满足不同进制回文次数
};
int main()
{
    int N, S;
    cin >> N >> S;
    while(N>0)
    {
        S++;
        Num N_S;
        N_S.N = S;
        for (int i = 2; i <= 10;i++)
        {
            if(B_Palindrome(S_Scale(N_S.N,i)))
                N_S.P_N++;
            if (N_S.P_N == 2)
            {
                cout << N_S.N << endl;
                N--;
                break;
            }
        }
    }
}



string S_Scale(int d,int N)
{
    int n = d;
    string scale = "";
    while(n>=N)
    {
        if (n%N<10)
            scale = to_string(n % N) + scale;
        else
            scale = char(n%N - 10 + 'A')+scale;
        n = n / N;
    }
    if (n%N<10)
            scale = to_string(n % N) + scale;
    else
        scale = char(n%N - 10 + 'A')+scale;
    return scale;
}

bool B_Palindrome(string S)
{
    for (int i = 0; i < S.size()/2;i++)
    {
        if (S[i]!=S[S.size()-i-1])
            return false;
    }
    return true;
}
```

## 7-10 1.3.1 混合牛奶 (80 分)

牛奶包装是一个如此低利润的生意,所以尽可能低的控制初级产品(牛奶)的价格变的十分重要.

请帮助快乐的牛奶制造者(Merry Milk Makers)以可能的廉价的方式取得他们所需的牛奶.

快乐的牛奶制造公司从一些农民那购买牛奶,每个农民卖给牛奶制造公司的价格不一定相同.

而且,如一只母牛一天只能生产一定量的牛奶,农民每一天只有一定量的牛奶可以卖.

每天,快乐的牛奶制造者从每个农民那购买一定量的牛奶,少于或等于农民所能提供的大值.

给出快乐牛奶制造者的每日的牛奶需求,连同每个农民的可提供的牛奶量和每加仑的价格,请计算 快乐的牛奶制造者所要付出钱的小值.

注意: 每天农民生产的牛奶的总数对快乐的牛奶制造者来说足够的.

### 输入格式:

第 1 行:二个整数, N 和 M.

第一个数值,N,(0<= N<=2,000,000)是快乐的牛奶制造者的一天需要牛奶的数量.

第二个数值,M,(0<= M<=5,000)是农民的数目.

第 2 到 M+1 行:每行二个整数,Pi 和 Ai.

Pi(0<= Pi<=1,000) 是农民 i 牛奶的价格.

Ai(0 <= Ai <= 2,000,000)是农民 i 一天能卖给快乐的牛奶制造者的牛奶数量.

### 输出格式:

单独的一行包含单独的一个整数,表示快乐的牛奶制造者拿到所需的牛奶所要的最小费用

### 输入样例:

在这里给出一组输入。例如：

```in
100 5
5 20
9 40
3 10
8 80
6 30
```

### 输出样例:

在这里给出相应的输出。例如：

```out
630
```

### 解题思路：

- **排序后从最便宜开始买**

### 答案：

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Farmer
{
    int Pi = 0;
    int Ai = 0;
};
bool cmp(Farmer F1,Farmer F2);
int main()
{
    int N, M, All_Money = 0;
    cin >> N >> M;
    vector<Farmer> Vec(M);
    //输入数据
    for (int i = 0; i < M;i++)
    {
        cin >> Vec[i].Pi >> Vec[i].Ai;
    }
    //从小到大排序
    sort(Vec.begin(), Vec.end(), cmp);
    for (int i = 0; i < Vec.size();i++)
    {
        int Money = 0;
        if(N<=Vec[i].Ai)
        {
            Money = N * Vec[i].Pi;
            All_Money += Money;
            break;
        }
        else
        {
            Money = Vec[i].Pi * Vec[i].Ai;
            N = N - Vec[i].Ai;
        }
        All_Money += Money;
    }
    cout << All_Money << endl;
}
bool cmp(Farmer F1, Farmer F2)
{
    if (F1.Pi<F2.Pi)
        return true;
    else
        return false;
}
```

## 7-11 1.3.2 修理牛棚 (100 分)

在一个暴风雨的夜晚,农民约翰的牛棚的屋顶、门被吹飞了. 好在许多牛正在度假,所以牛棚没有住 满. 剩下的牛一个紧挨着另一个被排成一行来过夜. 有些牛棚里有牛,有些没有. 所有的牛棚有相 同的宽度. 自门遗失以后,农民约翰很快在牛棚之前竖立起新的木板. 他的新木材供应者将会供应 他任何他想要的长度,但是供应者只能提供有限数目的木板. 农民约翰想将他购买的木板总长度减 到少. 给出 M(1<= M<=50),可能买到的木板大的数目;S(1<= S<=200),牛棚的总数;C(1 <= C <=S) 牛棚里牛的数目,和牛所在的牛棚的编号 stall_number(1 <= stall_number <= S),计算拦住 所有有牛的牛棚所需木板的小总长度. 输出所需木板的小总长度作为的答案.

### 输入格式:

第 1 行: M , S 和 C(用空格分开)

第 2 到 C+1 行: 每行包含一个整数,表示牛所占的牛棚的编号

### 输出格式:

单独的一行包含一个整数表示所需木板的小总长度.

### 输入样例:

在这里给出一组输入。例如：

```in
4 50 18
3
4
6
8
14
15
16
17
21
25
26
27
30
31
40
41
42
43
```

### 输出样例:

在这里给出相应的输出。例如：

```out
25
```

### 解题思路：

- **贪心算法，每次修最短的**

### 答案：

```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int Count(vector<int>);
int main()
{
    int M, S, C;
    vector<int> Vec;
    //数据输入
    cin >> M >> S >> C;
    for (int i = 0; i < C;i++)
    {
        int p = 0;
        cin >> p;
        Vec.push_back(p);
    }
    sort(Vec.begin(), Vec.end());
    vector<int> distance;
    for (int i = 0; i < Vec.size()-1;i++)
        distance.push_back(Vec[i + 1] - Vec[i]-1);
    sort(distance.begin(), distance.end());
    //计算木板数
    while(Count(Vec)>M)
    {
        int P = 0;
        //查找当前最短补全
        for (int j = 0; j < distance.size();j++)
        {
            if (distance[j]!=0)
            {
                P = j;
                break;
            }
        }
        //补全操作
        for (int i = 0; i < Vec.size() - 1; i++)
        {
            if (Vec[i + 1] - Vec[i]-1 == distance[P])
            {
                for (int t = 0; t < distance[P];t++)
                    Vec.insert(Vec.begin() + i+t+1,Vec[i]+1+t );
                distance[P] = 0;
                break;
            }
        }
    }
    cout << Vec.size() << endl;
}
int Count(vector<int> Vec)
{
    int temp = Vec[0];
    int L = 1;
    for (int i = 1; i < Vec.size();i++)
    {
        if (temp == Vec[i]-1)
            temp = Vec[i];
        else
        {
            L++;
            temp = Vec[i];
        }
    }
    return L;
}
```

