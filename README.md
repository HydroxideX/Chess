# Chess
This is a Chess game made by using C programming language
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
char board[8][8]={{'R','H','B','Q','K','B','H','R'},{'P','P','P','P','P','P','P','P'},
{'.','-','.','-','.','-','.','-'},{'-','.','-','.','-','.','-','.'},
{'.','-','.','-','.','-','.','-'},{'-','.','-','.','-','.','-','.'},
{'p','p','p','p','p','p','p','p'},{'r','h','b','q','k','b','h','r'}};
char deadPieces[32]={' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',
' ',' ',' ',' ',' ',' ',' '};
char boardlayout[8][8]={{'.','-','.','-','.','-','.','-'},{'-','.','-','.','-','.','-','.'},
{'.','-','.','-','.','-','.','-'},{'-','.','-','.','-','.','-','.'},
{'.','-','.','-','.','-','.','-'},{'-','.','-','.','-','.','-','.'},
{'.','-','.','-','.','-','.','-'},{'-','.','-','.','-','.','-','.'}};
int n=8,m=8;
/**********************************************************************************

***********************************************************************************/
/**********************************************************************************
*      clears output and prints the full board after each move                    *
***********************************************************************************/
void printboard()
{
system("cls");
int row,column;
for(row=0;row<32;row++)
    printf("%c ",deadPieces[row]);
printf("\n    A   B   C   D   E   F   G   H \n");
printf("    _   _   _   _   _   _   _   _ ");

for(row=0;row<n;row++)
{
printf("\n\n");
printf("%d ",row+1);
printf("|");
    for(column=0;column<m;column++)
    {

        printf(" %c |",board[row][column]);
    }
    printf("\n");
    printf("    _   _   _   _   _   _   _   _ ");
}
}
/***********************************************************************************************
 *c1 is first character input,c2 is second ,n1 is first number input, n2 is second number input*
 ***********************************************************************************************/
void player1Move()
{
int n1,n2;
char c1,c2;
printf("Player 1 :");
scanf("%c",&c1);
if(c1=='u')
{   //****************
    return ;
}
scanf("%d%c%d",&n1,&c2,&n2);
n1--;
n2--;
c1=c1-'A';
c2=c2-'A';
while(c1>7||c1<0||c2>7||c2<0||n1>7||n1<0||n2>7||n2<0)
{
    printf("Player 1 :");
    scanf("%c%d%c%d",&c1,&n1,&c2,&n2);
    n1--;
    n2--;
    c1=c1-'A';
    c2=c2-'A';
}
}
void player2Move()
{
printf("\n");
int n1,n2;
char c1,c2;
printf("Player 2 :");
scanf("%c%d%c%d",&c1,&n1,&c2,&n2);
n1--;
n2--;
c1=c1-'A';
c2=c2-'A';
while(c1>7||c1<0||c2>7||c2<0||n1>7||n1<0||n2>7||n2<0)
{
    printf("Player 2 :");
    scanf("%c%d%c%d",&c1,&n1,&c2,&n2);
    n1--;
    n2--;
    c1=c1-'A';
    c2=c2-'A';
}
}
void rook(char c1,char c2,int n1,int n2)
{
    int dif;
    if(c1==c2)
    {
    dif=n2-n1;
    diff( dif, c1, c2, n1, n2);
    revdiff( dif, c1, c2, n1, n2);
    }
    if(n1==n2)
    {
    dif=c2-c1;
    diff( dif, c1, c2, n1, n2);
    revdiff( dif, c1, c2, n1, n2);
    }
}
void diff(int dif,char c1,char c2,int n1,int n2)
{
   if(dif%2==0&&(board[n1][c1]=='R'||board[n1][c1]=='r'))
    {
       if((board[n2][c2]=='-'||board[n2][c2]=='p'||board[n2][c2]=='q'||board[n2][c2]=='b'
                ||board[n2][c2]=='h'||board[n2][c2]=='r')&&board[n1][c1]=='R')
       {
           board[n2][c2]='R';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if((board[n2][c2]=='.'||board[n2][c2]=='p'||board[n2][c2]=='q'||board[n2][c2]=='b'
                ||board[n2][c2]=='h'||board[n2][c2]=='r')&&board[n1][c1]=='R')
       {
           board[n2][c2]='R';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if((board[n2][c2]=='-'||board[n2][c2]=='P'||board[n2][c2]=='Q'||board[n2][c2]=='B'
                ||board[n2][c2]=='H'||board[n2][c2]=='R')&&board[n1][c1]=='r')
       {
           board[n2][c2]='r';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if((board[n2][c2]=='.'||board[n2][c2]=='P'||board[n2][c2]=='Q'||board[n2][c2]=='B'
                ||board[n2][c2]=='H'||board[n2][c2]=='R')&&board[n1][c1]=='r')
       {
           board[n2][c2]='r';
           board[n1][c1]=boardlayout[n1][c1];
       }
    }
    else if(dif%2==0&&(board[n1][c1]=='q'||board[n1][c1]=='Q'))
    {
        if((board[n2][c2]=='-'||board[n2][c2]=='p'||board[n2][c2]=='q'||board[n2][c2]=='b'
                ||board[n2][c2]=='h'||board[n2][c2]=='q')&&board[n1][c1]=='Q')
       {
           board[n2][c2]='Q';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if((board[n2][c2]=='.'||board[n2][c2]=='p'||board[n2][c2]=='q'||board[n2][c2]=='b'
                ||board[n2][c2]=='h'||board[n2][c2]=='q')&&board[n1][c1]=='Q')
       {
           board[n2][c2]='Q';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if((board[n2][c2]=='-'||board[n2][c2]=='P'||board[n2][c2]=='Q'||board[n2][c2]=='B'
                ||board[n2][c2]=='H'||board[n2][c2]=='R')&&board[n1][c1]=='q')
       {
           board[n2][c2]='q';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if((board[n2][c2]=='.'||board[n2][c2]=='P'||board[n2][c2]=='Q'||board[n2][c2]=='B'
                ||board[n2][c2]=='H'||board[n2][c2]=='R')&&board[n1][c1]=='q')
       {
           board[n2][c2]='q';
           board[n1][c1]=boardlayout[n1][c1];
       }
    }
}
void revdiff(int dif,char c1,char c2,int n1,int n2)
{
     if(dif%2==1&&(board[n1][c1]=='R'||board[n1][c1]=='r'))
    {
       if(board[n2][c2]=='-'&&board[n1][c1]=='R')
       {
           board[n2][c2]='R';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if(board[n2][c2]=='.'&&board[n1][c1]=='R')
       {
           board[n2][c2]='R';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if(board[n2][c2]=='-'&&board[n1][c1]=='r')
       {
           board[n2][c2]='r';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if(board[n2][c2]=='.'&&board[n1][c1]=='r')
       {
           board[n2][c2]='r';
           board[n1][c1]=boardlayout[n1][c1];
       }
    }
    else if(dif%2==1&&(board[n1][c1]=='Q'||board[n1][c1]=='q'))
    {
       if(board[n2][c2]=='-'&&board[n1][c1]=='Q')
       {
           board[n2][c2]='Q';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if(board[n2][c2]=='.'&&board[n1][c1]=='Q')
       {
           board[n2][c2]='Q';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if(board[n2][c2]=='-'&&board[n1][c1]=='q')
       {
           board[n2][c2]='q';
           board[n1][c1]=boardlayout[n1][c1];
       }
       else if(board[n2][c2]=='.'&&board[n1][c1]=='q')
       {
           board[n2][c2]='q';
           board[n1][c1]=boardlayout[n1][c1];
       }
    }
}
int main()
{   bool gameOver=false,player1Win=false,player2Win=false,stalemate=false;

    printboard(8,8,board,deadPieces);
    while(gameOver==false)
    {   printf("\n\n");
        player1Move();
        if(gameOver==true)
            break;
        printf("\n\n");
        player2Move();
    }
    if (player1Win==true)
    {
        printf("Player 1 Wins");
    }
    else if (player2Win==true)
    {
        printf("Player 2 Wins");
    }
    else
    {
        printf("Stalemate");
    }
    return 0;
}
