# C

## 银行管理系统


#include <stdio.h>
#include <stdlib.h>
#include<ctype.h>//isdigit函数的头文件
#include <conio.h>//getch()的头文件
#define NumMax 1000
int N;
void OpenAccount();//开户
void SaveMoney();//存款
void DrawMoney();//取款
void Inquire();//查询
void TransferAccounts();//转账
void ReportTheLoss();//挂失
void SolutionToHang();//解挂
void ClosingAccount();//销户
void ChangeThePassword();//改密
int search(int num);//判断账号的位置
int InputPassword();//输入密码
int judge(char*s);
typedef struct
{
    int num;//账号
    char name[20];//用户名
    int password;//密码
    double balance;//余额
    int status;//状态：0正常 1挂失 2销户
} Account;
Account user[NumMax];
int main()
{
    int i=0,a;
    FILE *fp;
    if ((fp=fopen("D:\\account.txt", "r"))==NULL)
    {
        printf("打不开文件！");
        exit(0);
    }
    while(fscanf(fp,"%d %s %d %lf %d",&user[i].num, user[i].name, &user[i].password, &user[i].balance,&user
 
                 [i].status ) != EOF)
    {
        i++;
    }
    fclose(fp);
    N=i;
    printf("---------\n");
    printf("欢迎光临！\n");
    printf("---------\n");
    while(a!=10)
    {
        printf("*************************\n");
        printf("请选择您的操作：\n");
        printf("1.开户  2.存款  3.取款  4.查询  5.转账\n");
        printf("6.挂失  7.解挂  8.销户  9.改密  10.欢迎再次使用\n");
        printf("*************************\n");
        scanf("%d",&a);
        if(a>0&&a<=10)
        {
            switch(a)
            {
            case 1:
                OpenAccount();
                break;
            case 2:
                SaveMoney();
                break;
            case 3:
                DrawMoney();
                break;
            case 4:
                Inquire();
                break;
            case 5:
                TransferAccounts();
                break;
            case 6:
                ReportTheLoss();
                break;
            case 7:
                SolutionToHang();
                break;
            case 8:
                ClosingAccount();
                break;
            case 9:
                ChangeThePassword();
                break;
            case 10:
                printf("欢迎再次使用！\n");
                break;
            }
        }
        else
            printf("输入错误。请重新输入\n");
    }
    if ((fp=fopen("account.txt", "w"))==NULL)
    {
        printf("打不开文件！");
        exit(0);
    }
    for(i=0; i<N; i++)
        fprintf(fp,"%d %s %d %lf %d\n",user[i].num, user[i].name, user[i].password, user[i].balance,user[i].status
 
               );
    fclose(fp);
    return 0;
}
void OpenAccount()
{
	int mima1,mima2;
    if(N==NumMax)
    {
        printf("银行用户数已达上限，不能开户");
        return;
    }
    printf("正在开户\n");
    user[N].num=10001+N;
    printf("账号：%d\n", user[N].num);
    printf("户主姓名：");
    scanf("%s", user[N].name);
 
    printf("密码：");
    mima1=InputPassword();
    printf("确认密码：");
    mima2=InputPassword();
    if(mima1==mima2)
    {
        user[N].password=mima1;
        user[N].status=0;
        N++;
        printf("成功开户！\n");
    }
    else
    {
        printf("两次密码不一致，未成功开户！\n");
    }
}
void SaveMoney()
{
    int a;
    int num;//账号
    int who;//查找到该账号在数组中对应的下标
    int mima;
    double money;
    char str[100];
    printf("请输入您的账号：\n");
    scanf("%d",&num);
    who = search(num);
    if(who<0)
        printf("该用户不存在，存款失败！\n");
    else
    {
        if(user[who].status==0)
        {
            printf("请输入密码：");
            mima=InputPassword();
            if(mima!=user[who].password)
                printf("密码错误，请重新输入：");
            else
            {
                printf("户主姓名：%s\n", user[who].name);
                printf("输入存款额：");
                scanf("%s", str);
                a=judge(str);
                while(a!=1)
                {
                    printf("输入错误，请重新输入存款额\n");
                    scanf("%s", str);
                    a=judge(str);
                }
                if(a==1)
                {
                    money=atoi(str);
                    if(money<0||money>1000000)
                        printf("输入金额太大，自动退出\n");
                    else
                    {
                        user[who].balance+=money;
                        printf("您的当前余额为%.2f元. \n",user[who].balance);
                    }
                }
            }
        }
        else if(user[who].status==1)
            printf("该用户处于挂失状态，存款失败！\n");
        else  if(user[who].status==2)
            printf("该用户已经销户，存款失败！\n");
    }
}
void DrawMoney()
{
    char str[100];
    int a;
    int num, who,mima;
    double money;
    printf("账号：");
    scanf("%d", &num);
    who = search(num);
    if(who<0)
    {
        printf("该用户不存在，取款失败！\n");
    }
    else
    {
        if(user[who].status==0)
        {
            printf("户主姓名：%s\n", user[who].name);
            printf("密码：");
            mima=InputPassword();
            if(mima!=user[who].password)
            {
                printf("输入密码错误，取款失败！\n");
            }
            else
            {
                printf("输入取款额：");
                scanf("%s", str);
                a=judge(str);
                while(a!=1)
                {
                    printf("输入错误，请重新输入取款额\n");
                    scanf("%s", str);
                    a=judge(str);
                }
                if(a==1)
                {
                    money=atoi(str);
                    if(money>user[who].balance)
                        printf("余额不足，取款失败！\n");
                    else
                    {
                        user[who].balance-=money;
                        printf("取款成功，您的余额为%.2f元. \n",user[who].balance);
                    }
                }
            }
        }
        else if(user[who].status==1)
        {
            printf("该用户处于挂失状态，取款失败！\n");
        }
        else
        {
            printf("该用户已经销户，取款失败！\n");
        }
    }
    printf("欢迎下次光临\n");
}
void Inquire()
{
    int num, who;
    int mima;
    printf("账号：");
    scanf("%d", &num);
    who = search(num);
    if(who<0)
    {
        printf("该用户不存在，查询完毕！\n");
    }
    else
    {
        printf("户主姓名：%s\n", user[who].name);
        printf("密码：");
        mima=InputPassword();
        if(mima!=user[who].password)
        {
            printf("输入密码错误，不能继续查询其他信息！\n");
        }
        else
        {
            printf("余额：%.2f元. \n",user[who].balance);
            printf("状态：");
            if(user[who].status==0)
            {
                printf("正常\n");
            }
            else if(user[who].status==1)
            {
                printf("挂失\n");
            }
            else
            {
                printf("已经销户\n");
            }
        }
    }
    printf("欢迎下次光临\n");
}
void TransferAccounts()
{
    char str[100];
    int a;
    int num, whoout, whoin;
    int mima;
    double money;
    printf("输入转出账号：");
    scanf("%d", &num);
    whoout = search(num);
    if(whoout<0)
    {
        printf("该用户不存在，转账失败！\n");
    }
    else
    {
        if(user[whoout].status==0)
        {
            printf("户主姓名：%s\n", user[whoout].name);
            printf("密码：");
            mima=InputPassword();
            if(mima!=user[whoout].password)
            {
                printf("输入密码错误，转账失败！\n");
            }
            else
            {
                printf("输入转账金额：");
                scanf("%s", str);
                a=judge(str);
                while(a!=1)
                {
                    printf("输入错误，请重新输入转账金额\n");
                    scanf("%s", str);
                    a=judge(str);
                }
                if(a==1)
                {
                    money=atoi(str);
                    if(money>user[whoout].balance)
                    {
                        printf("余额不足，转账失败！\n");
                    }
                    else
                    {
                        printf("输入转入账号：");
                        scanf("%d", &num);
                        whoin = search(num);
                        if(whoin<0)
                        {
                            printf("转入账户不存在，转账失败！\n");
                        }
                        else
                        {
                            if(user[whoin].status>0)
                            {
                                printf("转入账户异常，转账失败！\n");
                            }
                            else
                            {
 
 
                                user[whoout].balance-=money;
                                user[whoin].balance+=money;
                                printf("转账后，您还有%.2f元. \n",user[whoout].balance);
                            }
                        }
                    }
                }
            }
        }
        else
            printf("该账户异常，转账失败！\n");
    }
    printf("欢迎下次光临\n");
}
void ReportTheLoss()
{
    int num, who;
    int mima;
    printf("账号：");
    scanf("%d", &num);
    who = search(num);
    if(who<0)
    {
        printf("该用户不存在，不能挂失！\n");
    }
    else
    {
        printf("户主姓名：%s\n", user[who].name);
        printf("密码：");
        mima=InputPassword();
        if(mima!=user[who].password)
        {
            printf("输入密码错误，不能继续操作！\n");
        }
        else
        {
            if(user[who].status==0)
            {
                user[who].status=1;
                printf("挂失成功\n");
            }
            else if(user[who].status==1)
            {
                printf("该账户已经处于挂失状态\n");
            }
            else
            {
                printf("该账户已销户，不能挂失\n");
            }
        }
    }
}
void SolutionToHang()
{
    int num, who;
    int mima;
    printf("账号：");
    scanf("%d", &num);
    who = search(num);
    if(who<0)
    {
        printf("该用户不存在，解除挂失失败！\n");
    }
    else
    {
        printf("户主姓名：%s\n", user[who].name);
        printf("密码：");
        mima=InputPassword();
        if(mima!=user[who].password)
        {
            printf("输入密码错误，不能继续操作！\n");
        }
        else
        {
            if(user[who].status==0)
            {
                printf("该账户处于正常状态，不需要解除挂失\n");
            }
            else if(user[who].status==1)
            {
                user[who].status=0;
                printf("解除挂失成功\n");
            }
            else
            {
                printf("该账户已销户，操作无效\n");
            }
        }
    }
}
void ClosingAccount()
{
    int num;
    int who;
    int mima;
    printf("待销户账号：");
    scanf("%d", &num);
    who = search(num);
    if(who<0)
    {
        printf("该用户不存在，销户失败！\n");
    }
    else
    {
        printf("户主姓名：%s\n", user[who].name);
        printf("密码：");
        mima=InputPassword();
        if(mima==user[who].password)
        {
            printf("余额：%.2f 元\n", user[who].balance);
            user[who].balance=0;
            user[who].status=2;
            printf("取款 %.2f 元，销户成功！\n", user[who].balance);
        }
        else
        {
            printf("输入的密码错误，销户失败！\n");
        }
    }
}
void ChangeThePassword()
{
    int num, who;
    int mima, mima1, mima2;
    printf("账号：");
    scanf("%d", &num);
    who = search(num);
    if(who<0)
    {
        printf("该用户不存在，修改密码失败！\n");
    }
    else
    {
        printf("户主姓名：%s\n", user[who].name);
        printf("密码：");
        mima=InputPassword();
        if(mima!=user[who].password)
        {
            printf("输入密码错误，不能继续操作！\n");
        }
        else
        {
            printf("新密码：");
            mima1=InputPassword();
            printf("确认密码：");
            mima2=InputPassword();
            if(mima1==mima2)
            {
                user[who].password=mima1;
                printf("修改成功！\n");
            }
            else
            {
                printf("两次输入不同，修改失败！\n");
            }
        }
    }
}
int search(int num)
{
    int index=-1;
    int low=0, high=N-1, mid;
    while(low<=high)
    {
        mid = (low+high)/2;
        if(user[mid].num==num)
        {
            index=mid;
            break;
        }
        else if (user[mid].num>num)
            high=mid-1;
        else
            low=mid+1;
    }
    return index;
}
int InputPassword()
{
    char c;  //接收字符形式密码
    int mima=0;   //要转换为数字
    int i;
    while(1)
    {
        for(i=0; i<6; i++)
        {
            c=getch();//输入但不显示
            putchar('*');
            if(isdigit(c))//isdight主要用于检查参数c是否为阿拉伯数字0到9 头文件为#include<ctype.h>
                mima=mima*10+(c-'0');
            else
            {
                mima=0;
                break;
            }
        }
        fflush(stdin); //清除键盘缓存区中已经有的输入
        printf("\n");
        if(mima==0)
        {
            printf("密码要求全为数字，且不能全0！\n");
            printf("请重新输入密码: ");
        }
        else
            break;
    }
    return mima;
}
int judge(char*s)//判断输入的金额是否符合要求
{
    int a,i=0;
    while(s[i]!='\0')
    {
        if(s[i]<'0'||s[i]>'9')
        {
            a=0;
            break;
        }
        i++;
    }
    if(a!=0)
        a=1;
    return a;
}
