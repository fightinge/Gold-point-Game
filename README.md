# Gold-point-Game
多人进行的小游戏
#include "stdafx.h"
#include "stdlib.h"
#include "windows.h"
#include "math.h"
typedef struct gameplayer //定义玩家结构体
{
int name;
int score;
float number;
}PLAYER; //含有玩家的姓名，分数，编号，分数信息

      每个玩家涉及信息量较多，我们很自然选用了结构体。

void startgame(FILE *fp); //游戏的初始化
void gamerule(); //游戏规则
void suan_score(PLAYER *p,float ave,int num,FILE *fp);//计算比赛得分的函数

      这里我们主要用到这几个函数。

int main()
{
system("color 3B");
int choice;
FILE *fp;
while(1)
{
fp=fopen("goldgame.txt","w+");
printf("\n 黄金点游戏 \n\n");
printf(" 1. 游戏规则\n");
printf(" 2. 开始游戏\n");
printf(" 3. 退出游戏\n");
scanf("%d",&choice);
switch(choice)
{
case 1: gamerule(); break;
case 2: startgame(fp); break;
case 3: exit(0); break;
default:
{
printf(" 您的输入有误，请重新输入\n");
break;
}
}
}
return 0;
}

      主函数，我们设计了初始界面：包括开始游戏，游戏规则，退出游戏简单的功能。用SWITCH函数读取玩家的操作。

void startgame(FILE *fp)
{
PLAYER *p;
int i,playernum,gamenum,j;
int flag=1;
float sum,ave;
char choice;
p=(PLAYER *)malloc(10*sizeof(PLAYER)); //动态分配结构体数组
printf("请输入参与的玩家数：");
scanf("%d",&playernum);
if(playernum>10)
{
p=(PLAYER *)realloc(p,playernum*sizeof(PLAYER));//空间不足而需要增加空间
}
printf("请输入比赛轮次：");
scanf("%d",&gamenum);
printf("\n");
for(j=0;j<gamenum;j++)
{
printf("第%d轮比赛：\n",j+1);
for(i=0,sum=0 ;i<playernum;i++)//sum设为总数和
{
if(flag==1)
{
p[i].name=i+1;
p[i].score=0;
p[i].win=0;
p[i].fail=0;
} //初始化初值为0
printf(" 玩家%d: ",p[i].name);
scanf("%f",&p[i].number);
sum+=p[i].number;
}
ave=sum/playernum; //计算平均值
ave=(float)(ave*0.618); //计算黄金点的值
printf(" 黄金点G的值为：%f\n",ave);
suan_score(p,ave,playernum,fp);
flag=0;
}

}

     这是开始化游戏函数，动态分配内存空间，如果不够，需要补充空间。主要进行玩家人数和游戏轮次的设定，然后读取玩家各个数值，计算G点值，进入suan_score函数。

void suan_score(PLAYER *p,float ave,int num,FILE *fp)
{
int i;
char ch;
float max=(float)fabs(p[0].number-ave);
float min=(float)fabs(p[0].number-ave);

for(i=0;i<num;i++) //统计出本轮最大值，最小值
{
p[i].b=(float)fabs(p[i].number-ave);
if(max<p[i].b)
max=p[i].b;
if(min>p[i].b)
min=p[i].b;
}

for(i=0;i<num;i++) //挨个玩家赋予成绩
{
if(p[i].b==max)
{
p[i].score-=2;
}
if(p[i].b==min)
{
p[i].score+=num;
}
}
printf("累计比赛的得分：\n");
for(i=0;i<num;i++) //挨个玩家输出成绩
printf(" 玩家%d: %d\n",p[i].name,p[i].score);
while(1)
{
printf("是否继续 Y or N : ");
scanf("%c",&ch);
if(ch=='y'||ch=='Y')
{
break;
}
else if(ch=='n'||ch=='N')
{
exit(0);
}

}
}
