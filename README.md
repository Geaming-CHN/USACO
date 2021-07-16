# USACO刷题笔记

[TOC]

## 前言

笔者为重庆大学CS专业学生Geaming，小学期刚好刷了一下USACO的题，因为之前并没有接触过，写的代码~~绝对~~可能不是最优的，但个人来说比较好理解，欢迎互相讨论，。

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

- 暴力求解

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



