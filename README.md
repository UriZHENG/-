
c语言实验课作业8
#include <stdio.h>
#include <stdlib.h>
#include<string.h>
#define N 30//人数最高限制
#define M 6//科目最高限制
typedef struct student//定义结构体数组student
{
    char name[N];
    long number;
    int total;
    int score[M];
    float ave;
}STUDENT;

void ReadScore(STUDENT stu[],int n,int m);
void TotalCourse(STUDENT stu[],int n,int m);
void SelectionSort1(STUDENT stu[],int n,int m,int(*compare)(int a,int b));
void SelectionSort2(STUDENT stu[],int n,int m);
void SelectionSort3(STUDENT stu[],int n,int m);
void Swap(int *x,int *y);
void PrintScore(STUDENT stu[],int n,int m);
void SearchNumber(STUDENT stu[],int n,int m);
void SearchName(STUDENT stu[],int n,int m);
void Statistics(STUDENT stu[],int n,int m);
void PrintList(STUDENT stu[],int n,int m);
int Ascending(int a,int b);
int Descending(int a,int b);
int main()
{
    int choice,n,m,i,j,k,find=0;
    STUDENT stu[N];
    POINT1:
    printf("How many course(max is 6):");
    scanf("%d",&m);
        if(m<0||m>6)//防止科目数不符合要求
    {
        printf("Too many courses!Please cut down the number and input again!\n");
        goto POINT1;
    }
    POINT7:
    printf("How many students(max is 30):");
    scanf("%d",&n);
        if(n>30)//防止学生人数过多
    {
        printf("Too many students!Please cut down the number and input again!\n");
        goto POINT7;
    }
    //打印选项
     printf("\n");
     printf("*1．Input record\n");
     printf("*2. Calculate total and average score of every course\n");
     printf("*3. Calculate total and average score of every student\n");
     printf("*4. Sort in descending order by total score of every student\n");
     printf("*5. Sort in ascending order by total score of every student\n");
     printf("*6. Sort in ascending order by number\n");
     printf("*7. Sort in dictionary order by name\n");
     printf("*8. Search by number\n");
     printf("*9. Search by name\n");
     printf("*10.Statistic analysis for every course\n");
     printf("*11.List record\n");
     printf("*0. Exit\n");
     printf("**********Attention please:The scores are all int!\n");
     printf("**********Attention please:The number could not start with 0:");
    do{
    POINT2://确保用户输入了数据
    if(find==0)
    {
        printf("\nPlease input your choice:");
        scanf("%d",&choice);
        switch(choice)
        {
        case 1:
            ReadScore(stu,n,m);
            //计算出每个学生的总分和平均分
           for(i=0;i<n;i++)
             {
                 stu[i].total=0;
           for(j=0;j<m;j++)
               {
            stu[i].total+=stu[i].score[j];
               }
           stu[i].ave=(float)stu[i].total/m;
             }
             find=1;
        break;
        case 0:
            printf("Exit!");
            return 0;//输入0时退出
        default:
            printf("Please input choice 1");
        goto POINT2;
        }
    }
    else
        {
    POINT3://多次进行成绩系统的运行
        printf("Please input your choice:");
        scanf("%d",&choice);
    switch(choice)
    {
        case 2:
             TotalCourse(stu,n,m);
             goto POINT3;
        case 3:
            for(k=0;k<n;k++)
            {
            printf("Student %d's total score is %d,average score is %f\n",k+1,stu[k].total,stu[k].ave);
            }
            goto POINT3;
        case 4:
            SelectionSort1(stu,n,m,Ascending);
            printf("Score items in ascending order:\n");
            PrintScore(stu,n,m);
            goto POINT3;
        case 5:
            SelectionSort1(stu,n,m,Descending);
            printf("Score items in decending order:\n");
            PrintScore(stu,n,m);
            goto POINT3;
        case 6:
            SelectionSort2(stu,n,m);
            printf("Number items in Ascending order:\n");
            PrintScore(stu,n,m);
            goto POINT3;
        case 7:
            SelectionSort3(stu,n,m);
            printf("Name items in Dictionary order:\n");
            PrintScore(stu,n,m);
            goto POINT3;
        case 8:
            SelectionSort1(stu,n,m,Descending);
            SearchNumber(stu,n,m);
           goto POINT3;
        case 9:
            SelectionSort1(stu,n,m,Descending);
            SearchName(stu,n,m);
           goto POINT3;
        case 10:
           Statistics(stu,n,m);
           goto POINT3;
        case 11:
            printf("\n");
            printf("List record:\n");
            PrintList(stu,n,m);
            goto POINT3;
        case 0:
            printf("Exit!");
            break;
        default:
            printf("Wrong input!Please input again!\n");
    }
    }
    }while(choice!=0);//当choice=0时结束
    return 0;
}

//函数功能：输入学生姓名，学生学号和成绩
void ReadScore(STUDENT stu[],int n,int m)
{
    int i,j;
    for(i=0;i<n;i++)
    {
        printf("No.%d Student name:",i+1);
        getchar();
        gets(stu[i].name);
        printf("His/Her ID:");
        scanf("%d",&stu[i].number);
        for(j=0;j<m;j++)
        {
            printf("His/Her score %d:",j+1);
            scanf("%d",&stu[i].score[j]);
        }
    }
}
//函数功能：计算每门科目的总得分和平均分
void TotalCourse(STUDENT stu[],int n,int m)
{
    int i,j,sum[M]={0};
    float pingjun[M];
    for(i=0;i<m;i++)
    {
        for(j=0;j<n;j++)
        {
        sum[i]=sum[i]+stu[j].score[i];
        }
        pingjun[i]=(float)sum[i]/n;
    }
    for(i=0;i<m;i++)
    {
        printf("The total score of course%d is %d,average score is %f\n",i+1,sum[i],pingjun[i]);
    }
}
//函数功能：对学生总成绩进行排序
void SelectionSort1(STUDENT stu[],int n,int m,int(*compare)(int a,int b))
{
    int i,j,k,x;
    char temp[30];
    for(i=0;i<n-1;i++)
    {
        k=i;
        for(j=i+1;j<n;j++)
        {
            if((*compare)(stu[j].total,stu[k].total))
                k=j;
        }
        if (k!=i)
            {
            Swap(&stu[k].total,&stu[i].total);
            Swap(&stu[k].number,&stu[i].number);
            for(x=0;x<m;x++)
            {
                Swap(&stu[k].score[x],&stu[i].score[x]);
            }
            strcpy(temp,stu[k].name);
            strcpy(stu[k].name,stu[i].name);
            strcpy(stu[i].name,temp);
            }
    }
}
//函数功能：对学生学号按从小到大进行排序
void SelectionSort2(STUDENT stu[],int n,int m)
{
    int i,j,k,x;
    char temp[30];
    for(i=0;i<n-1;i++)
    {
        k=i;
        for(j=i+1;j<n;j++)
        {
            if(stu[j].number<stu[k].number)
                k=j;
        }
        if (k!=i)
            {
            Swap(&stu[k].number,&stu[i].number);
            Swap(&stu[k].total,&stu[i].total);
           for(x=0;x<m;x++)
            {
                Swap(&stu[k].score[x],&stu[k].score[x]);
            }
            strcpy(temp,stu[k].name);
            strcpy(stu[k].name,stu[i].name);
            strcpy(stu[i].name,temp);
    }
}
}
//函数功能：对学生按照姓名首字母从小到大排序
void SelectionSort3(STUDENT stu[],int n,int m)
{
    int i,j,x;
    char temp[30];
    for(i=0;i<n-1;i++)
    {
        for(j=i+1;j<n;j++)
        {
            if(strcmp(stu[j].name,stu[i].name)<0)
            {
                strcpy(temp,stu[i].name);
                strcpy(stu[i].name,stu[j].name);
                strcpy(stu[j].name,temp);
                Swap(&stu[j].number,&stu[j].number);
                Swap(&stu[j].score,&stu[i].score);
                            for(x=0;x<m;x++)
            {
                Swap(&stu[j].score[x],&stu[i].score[i]);
            }
            }
        }
    }
}
//升序排序
int Ascending(int a,int b)
{
    return a<b;
}
//降序排序
int Descending(int a,int b)
{
    return a>b;
}
//函数功能：交换两个数的值
void Swap(int *x,int *y)
{
    int temp;
    temp=*x;
    *x=*y;
    *y=temp;
}
//函数功能：输出成绩表
void PrintScore(STUDENT stu[],int n,int m)
{
    int i,j;
        printf("\t\tName\tID\tTotal");
        for(j=0;j<m;j++)
        {
            printf("\tscore %d",j+1);
        }
        printf("\n");
     for(i=0;i<n;i++)
    {
        printf("student %d.\t%s\t%ld\t%d",i+1,stu[i].name,stu[i].number,stu[i].total);
        for(j=0;j<m;j++)
        {
            printf("\t%d",stu[i].score[j]);
        }
        printf("\n");
    }
}

//函数功能：寻找对应学号的学生的成绩和排名
void SearchNumber(STUDENT stu[],int n,int m)
{
    int time,k,i,flag=0;
    printf("Input the ID you are searching for：");
    scanf("%d",&time);
    for(k=0;k<n&&flag==0;k++)
    {
        if(time==stu[k].number)
        {
            flag=1;
            break;
        }
    }
    if(flag==1)//如果寻找到该学生
    {
    printf("*Each score of the student is:\n");
    for(i=0;i<m;i++)
    {
        printf("score%d\t",i+1);
    }
    printf("\n");
    for(i=0;i<m;i++)
    {
        printf("%d\t",stu[k].score[i]);
    }
    printf("\n");
    printf("And his/her list is NO.%d\n",k+1);
    }
    if(flag==0)//防止非法输入
        {
            printf("Wrong ID!\n");
        }
}
//函数功能：寻找对应姓名的学生的成绩和排名
void SearchName(STUDENT stu[],int n,int m)
{
    char time[30];
    int i,j,e,count,flag=0;
    printf("Input the name you are searching for：");
    scanf("%s",&time);
    for(i=0;i<n;i++)
    {
        if(strcmp(time,stu[i].name)==0)
      {
          flag=1;
      }
      if(flag==1)//如果寻找到该学生
      {
        for(e=0;e<m;e++)
        {
        printf("score%d\t",e+1);
        }
        printf("\n");
        for(e=0;e<m;e++)
        {
        printf("%d\t",stu[i].score[e]);
        }
        printf("\n");
        printf("And his/her list is NO.%d\n",i+1);
        break;
      }
    }
    //防止错误输入
    if(flag==0)
    {
        printf("Wrong name!\n");
    }
}

//函数功能：对每门课成绩数据进行分析
void Statistics(STUDENT stu[],int n,int m)
{
    int i,j,k;
    i=0;
    do{
            int a[5]={0};
            float b[5]={0.000};
    for(j=0;j<n;j++)
    {
        if(stu[j].score[i]<60)
            a[0]++;
        if(stu[j].score[i]>=60&&stu[j].score[i]<=69)
            a[1]++;
        if(stu[j].score[i]<=79&&stu[j].score[i]>=70)
            a[2]++;
        if(stu[j].score[i]>=80&&stu[j].score[i]<=89)
            a[3]++;
        if(stu[j].score[i]>=90)
            a[4]++;
    }
    for(k=0;k<5;k++)
    {
    b[k]=(a[k]*100.00)/n;
    }
    //将最终结果制成表格
    printf("\n");
    printf("Analysis of score%d.\n",i+1);
    printf("\t优秀\t\t良好\t\t一般\t\t及格\t\t不及格\n");
    printf("人数\t%3d\t\t%3d\t\t%3d\t\t%3d\t\t%3d\n",a[4],a[3],a[2],a[1],a[0]);
    printf("百分比\t%f%%\t%f%%\t%f%%\t%f%%\t%f%%",b[4],b[3],b[2],b[1],b[0]);
    printf("\n");
    i++;
    }while(i<m);
}
//函数功能：制表输出每位学生的姓名，学号，总分，每门课的得分，平均分
void   PrintList(STUDENT stu[],int n,int m)
{
    int i,j;
        printf("\t\tname\tID\tTotal");
        for(j=0;j<m;j++)
        {
            printf("\tscore%d",j+1);
        }
        printf("\taverage");
        printf("\n");
     for(i=0;i<n;i++)
    {
        printf("student %d.\t%s\t%ld\t%d",i+1,stu[i].name,stu[i].number,stu[i].total);
        for(j=0;j<m;j++)
        {
            printf("\t%d",stu[i].score[j]);
        }
        printf("\t%f",stu[i].ave);
        printf("\n");
    }
}


