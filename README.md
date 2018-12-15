#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
bool stalemate = false;
        gameOver=false,
        player1Win=false,
        player2Win=false,
        EnPassent1=false,
        EnPassent2=false;

int  PassentP2X,PassentP2Y;
int  PassentP1X,PassentP1Y;
int kingsindex[4] = {7,4,0,4};
char temparr[8][8]=
{
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'}
};
char board[8][8]=
{
    {'R','H','B','Q','K','B','H','R'},
    {'P','P','P','P','P','P','P','P'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'p','p','p','p','p','p','p','p'},
    {'r','h','b','q','k','b','h','r'}
};

char deadPieces[32]= {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '};
// char deadPieces[32]= {'p','r','h','b','q','P','R','H','B','Q','h','b','r','H','B','R','p','p','p','p','p','p','p','P','P','P','P','P','P','P',' ',' '}; // GRAVEYARD TEST

char boardlayout[8][8]=
{
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'},
    {'.','-','.','-','.','-','.','-'},
    {'-','.','-','.','-','.','-','.'}
};

int blackMoves[12][12]= {{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1}
};
int whiteMoves[12][12]= {{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1}
};
int n=8;
int m=8;
int deadindex=0;
void printboard()
{
    /*  ***************************************************** PLAYER 1 RELATED ******************************************************* */

    system("cls");
    int row,column;
    printf("Dead Pieces:");
//printf("\n");
    for(row=0; row<32; row++)
        printf("%c ",deadPieces[row]);
    printf("\n\n    A   B   C   D   E   F   G   H \n");
    printf("  ________________________________");

    for(row=0; row<n; row++)
    {
        printf("\n");
        printf("%d ",row+1);
        printf("|");
        for(column=0; column<m; column++)
        {
            printf(" %c |",board[row][column]);
        }
        printf("\n");
        printf("  |_______________________________|");
    }
}

void pawnP1(int y1,int y2,int x1,int x2)
{
    if(x1-x2 == 2)
    {
        EnPassent1=true;
        PassentP1X=x2;
        PassentP1Y=y2;
    }
    else
        EnPassent1=false;
    if(board[x1][y1] == '.' || board[x1][y1] == '-')
    {
        player1Move();
        return;
    }
    if(board[x1][y1] == 'p')
    {
        char upgrade1;
        if(y1!=y2)
        {
        if(board[x2][y2] == 'P')
            deadPieces[deadindex++] = 'P';
        else if(board[x2][y2] == 'R')
            deadPieces[deadindex++] = 'R';
        else if(board[x2][y2] == 'H')
            deadPieces[deadindex++] = 'H';
        else if(board[x2][y2] == 'B')
            deadPieces[deadindex++] = 'B';
        else if(board[x2][y2] == 'Q')
            deadPieces[deadindex++] = 'Q';
        }


        if ( x1 == 6 )
        {
            if(y1!=y2 && x2<x1 && board[x2][y2] != '-' && board[x2][y2] != '.')
            {
                if(board[x2][y2] != 'p' &&board[x2][y2] != 'r'&&board[x2][y2] != 'h'&&board[x2][y2] != 'b'&&board[x2][y2] != 'q' )
                {
                board[x2][y2] = 'p';
                board[x1][y1] = boardlayout[x1][y1];
                printboard(8,8,board,deadPieces);
                return;
                }
                else
                {
                 printf("INVALID MOVE");
                player1Move();
                 return ;
                }
            }
            else if(y1 == y2 && x2 >=4 && x2<x1)
            {
                board[x2][y2] = 'p';
                board[x1][y1] = boardlayout[x1][y1];
                printboard(8,8,board,deadPieces);
            }
            else
            {
                printf("INVALID MOVE");
                player1Move();
                return ;
            }
        }

        if(x1<6)
            {
                    if(  (board[x2][y2] =='-' || board[x2][y2] =='.')  &&   (board[x1][y1+1] == 'P' || board[x1][y1-1] == 'P') && x1==3 &&  EnPassent2==true && x2==PassentP2X-1 && y2==PassentP2Y )
                    {
                    board[x2][y2] = 'p';
                    if(board[x1][y1+1] == 'P')
                        {board[x1][y1] = boardlayout[x1][y1];
                        board[x1][y1+1] = boardlayout[x1][y1+1];}
                    else if(board[x1][y1-1] == 'P')
                        {board[x1][y1] = boardlayout[x1][y1];
                        board[x1][y1-1] = boardlayout[x1][y1-1];}
                    deadPieces[deadindex++]='P';
                    printboard(8,8,board,deadPieces);
                    return ;
                    }
                if(y1!=y2 && x2<x1 && board[x2][y2] != '-' && board[x2][y2] != '.')
                {
                    if(board[x2][y2] != 'p' &&board[x2][y2] != 'r'&&board[x2][y2] != 'h'&&board[x2][y2] != 'b'&&board[x2][y2] != 'q' )
                    {
                    board[x2][y2] = 'p';
                    board[x1][y1] = boardlayout[x1][y1];
                    printboard(8,8,board,deadPieces);
                    return ;
                    }

                    else
                    {
                     printf("INVALID MOVE");
                     player1Move();
                     return ;

                    }

                }
                else if(y1 == y2 && x1==x2+1 && board[x2][y2] == '-')
                {
                    board[x2][y2] = 'p';
                    board[x1][y1] = boardlayout[x1][y1];
                    printboard(8,8,board,deadPieces);
                }
                else if(y1 == y2 && x1==x2+1 && board[x2][y2] == '.')
                {
                    board[x2][y2] = 'p';
                    board[x1][y1] = boardlayout[x1][y1];
                    printboard(8,8,board,deadPieces);
                }
                else
                {
                    printf("INVALID MOVE");
                    player1Move();
                    return ;
                }
            }

            if(x2==0)
            {
                printf("\n");
                printf("Upgrade Your Pawn : ");
                scanf(" %c", &upgrade1);
                if(upgrade1=='h')
                    board[x2][y2] = 'h';
                else if(upgrade1=='b')
                    board[x2][y2] = 'b';
                else if(upgrade1=='r')
                    board[x2][y2] = 'r';
                else if(upgrade1=='q')
                    board[x2][y2] = 'q';
            }
    }


}

void player1Move()
{
    int n1,n2;
    int i,j;
    char c1,c2,c3;
    whiteDanger();
    if(blackMoves[kingsindex[0]+2][kingsindex[1]+2]==1)
    {
        if(blackMoves[kingsindex[0]+2][kingsindex[1]+2+1]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
            if(blackMoves[kingsindex[0]+2][kingsindex[1]+2-1]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
                if(blackMoves[kingsindex[0]+2+1][kingsindex[1]+2+1]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
                    if(blackMoves[kingsindex[0]+2+1][kingsindex[1]+2-1]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
                        if(blackMoves[kingsindex[0]+2+1][kingsindex[1]+2]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
                            if(blackMoves[kingsindex[0]+2-1][kingsindex[1]+2+1]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
                                if(blackMoves[kingsindex[0]+2-1][kingsindex[1]+2]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2)
                                    if(blackMoves[kingsindex[0]+2-1][kingsindex[1]+2]==1||blackMoves[kingsindex[0]+2][kingsindex[1]+2]==2);

    }
    if(blackMoves[kingsindex[0]+2][kingsindex[1]+2]==1)
    {
        for(i=0; i<8; i++)
        {
            for(j=0; j<8; j++)
            {
                temparr[i][j]=board[i][j];
            }
        }
        while(blackMoves[kingsindex[0]+2][kingsindex[1]+2]==1)
        {
            for(i=0; i<8; i++)
            {
                for(j=0; j<8; j++)
                {
                    board[i][j]=temparr[i][j];
                }
            }
            printf("\n\nCheck\n");
            player1Movetemp();
            whiteDanger();
        }
        return ;
    }
// Positions

    printboard();
    printf("\n\n");
    printf("PLAYER 1");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);

    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);

    n1--;
    n2--;
    int x1=n1;
    int x2=n2;
    c1=c1-'A';
    c2=c2-'A';
// To Solve Conflict Between Character And Integer
    int y1 = c1;
    int y2= c2;
if((x1>=0 || x1 <=8) && (x2>=0 || x2<=8) && ((c1>=0 || c1<=8) && (c2>=0 || c2<=8)) )
    /*        PAWN PAWN PAWN PAWN        */

// Checking If It Is Pawn And If True It Must Be At Row 7 That Means x1=6 Else Give Error
{

    pawnP1(y1,y2,x1,x2);
    rook1(y1,y2,x1,x2);
    bishop1(y1,y2,x1,x2);
    horse1(y1,y2,x1,x2);

    system("cls");
}
else
    return;



}
void player1Movetemp()
{
    printboard();
    int n1,n2;
    char c1,c2,c3;
    printf("\n\n");
    printf("PLAYER 1");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);
    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);
    n1--;
    n2--;
    int x1=n1;
    int x2=n2;
    c1=c1-'A';
    c2=c2-'A';
    int y1 = c1;
    int y2= c2;
    pawnP1(y1,y2,x1,x2);
    rook1(y1,y2,x1,x2);
    bishop1(y1,y2,x1,x2);
    horse1(y1,y2,x1,x2);
    system("cls");
}
/* ****************************************************** PLAYER 2 RELATED *************************************************** */
void pawnP2(int y1,int y2,int x1,int x2)
{
    if(x2-x1 == 2)
    {
        EnPassent2=true;
        PassentP2X=x2;
        PassentP2Y=y2;
    }
    else
        EnPassent2=false;
    if(board[x1][y1] == '.' || board[x1][y1] == '-')
    {
        player2Move();
        return;
    }
    if(board[x1][y1] == 'P')
    {
        char upgrade2;
        //Dead Conditions
        if(y1!=y2)
        {
        if(board[x2][y2] == 'p')
            deadPieces[deadindex++] = 'p';
        else if(board[x2][y2] == 'r')
            deadPieces[deadindex++] = 'r';
        else if(board[x2][y2] == 'h')
            deadPieces[deadindex++] = 'h';
        else if(board[x2][y2] == 'b')
            deadPieces[deadindex++] = 'b';
        else if(board[x2][y2] == 'q')
            deadPieces[deadindex++] = 'q';
        }


        if ( x1 == 1 )
        {
            if(y1!=y2 && x2>x1 && board[x2][y2] != '-' && board[x2][y2] != '.')
            {
                    if(board[x2][y2] != 'P' &&board[x2][y2] != 'R'&&board[x2][y2] != 'H'&&board[x2][y2] != 'B'&&board[x2][y2] != 'Q' )
                    {
                    board[x2][y2] = 'P';
                    board[x1][y1] = boardlayout[x1][y1];
                    printboard(8,8,board,deadPieces);
                    return;
                    }
                    else
                    {
                     printf("INVALID MOVE");
                     player2Move();
                     return ;
                    }
            }
           else if(y1 == y2 && x2 <=3 && x2>x1 && board[x2][y2] == '-')
            {
                board[x2][y2] = 'P';
                board[x1][y1] = boardlayout[x1][y1];
                printboard(8,8,board,deadPieces);
            }
           else if(y1 == y2 && x2 <=3 && x2>x1 && board[x2][y2] == '.')
            {
                board[x2][y2] = 'P';
                board[x1][y1] = boardlayout[x1][y1];
                printboard(8,8,board,deadPieces);
            }
            else
            {
                 printf("INVALID MOVE");
                 player2Move();
                 return ;
            }
        }

        if(x1>1)
        {
                    if(  (board[x2][y2] =='-' || board[x2][y2] =='.')  &&   (board[x1][y1+1] == 'p' || board[x1][y1-1] == 'p') && x1==4 &&  EnPassent1==true && x2==PassentP1X+1 && y2==PassentP1Y )
                    {
                    board[x2][y2] = 'P';
                    if(board[x1][y1+1] == 'p')
                        {board[x1][y1] = boardlayout[x1][y1];
                        board[x1][y1+1] = boardlayout[x1][y1+1];}
                    else if(board[x1][y1-1] == 'p')
                        {board[x1][y1] = boardlayout[x1][y1];
                        board[x1][y1-1] = boardlayout[x1][y1-1];}
                    deadPieces[deadindex++]='p';
                    printboard(8,8,board,deadPieces);
                    return ;
                    }
           if(y1!=y2 && x2>x1 && board[x2][y2] != '-' && board[x2][y2] != '.')
            {
                    if(board[x2][y2] != 'P' &&board[x2][y2] != 'R'&&board[x2][y2] != 'H'&&board[x2][y2] != 'B'&&board[x2][y2] != 'Q' )
                    {
                    board[x2][y2] = 'P';
                    board[x1][y1] = boardlayout[x1][y1];
                    printboard(8,8,board,deadPieces);
                    }
                    else
                    {
                     printf("INVALID MOVE");
                     player2Move();
                     return ;
                    }
            }
           else if(y1 == y2 && x2==x1+1 && board[x2][y2] == '-')
            {
                board[x2][y2] = 'P';
                board[x1][y1] = boardlayout[x1][y1];
                printboard(8,8,board,deadPieces);
            }
           else if(y1 == y2 && x2==x1+1 && board[x2][y2] == '.')
            {
                board[x2][y2] = 'P';
                board[x1][y1] = boardlayout[x1][y1];
                printboard(8,8,board,deadPieces);
            }
            else
            {
                 printf("INVALID MOVE");
                 player2Move();
                 return ;
            }
        }

        if(x2==7)
        {
            printf("\n");
            printf("Upgrade Your Pawn : ");
            scanf(" %c", &upgrade2);
            if(upgrade2=='H')
                board[x2][y2] = 'H';
            else if(upgrade2=='B')
                board[x2][y2] = 'B';
            else if(upgrade2=='R')
                board[x2][y2] = 'R';
            else if(upgrade2=='Q')
                board[x2][y2] = 'Q';
        }
    }
}

void player2Move()
{
    int i,j;
    int n1,n2;
    char c1,c2;
    blackDanger();
    /*if(whiteMoves[kingsindex[2]+2][kingsindex[3]+2]==1)
    {
       for(i=0; i<8; i++)
       {
           for(j=0; j<8; j++)
           {
               temparr[i][j]=board[i][j];
           }
       }
       while(whiteMoves[kingsindex[2]+2][kingsindex[3]+2]==1)
       {
           for(i=0; i<8; i++)
           {
               for(j=0; j<8; j++)
               {
                   board[i][j]=temparr[i][j];
               }
           }
           printf("\n\nCheck\n");
           player2Movetemp();
           blackDanger();
       }
       return ;
    }*/
    printboard();
// Positions
    printf("PLAYER 2:");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);

    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);

//  Important To Solve Index Problem
    n1--;
    n2--;
    int x1=n1;
    int x2=n2;
    c1=c1-'A';
    c2=c2-'A';
// To Solve Conflict Between Character And Integer
    int y1 = c1;
    int y2= c2;

    /*        PAWN PAWN PAWN PAWN        */

// Checking If It Is Pawn And If True It Must Be At Row 7 That Means x1=6 Else Give Error

    pawnP2(y1,y2,x1,x2);
    rook2(y1,y2,x1,x2);
    bishop2(y1,y2,x1,x2);
    horse2(y1,y2,x1,x2);
    system("cls");



    /*
    while(y1>7||y1<0||y2>7||y2<0||x1>7||x1<0||x2>7||x2<0)
    {
        printf("Player 1 :");
        scanf("%c%d%c%d",&y1,&x1,&y2,&x2);
        x1--;
        x2--;
        y1=y1-'A';
        y2=y2-'A';
    }
    */


}
void player2Movetemp()
{

    int n1,n2;
    char c1,c2;
    printboard();
// Positions
    printf("PLAYER 2:");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);
    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);

//  Important To Solve Index Problem
    n1--;
    n2--;
    int x1=n1;
    int x2=n2;
    c1=c1-'A';
    c2=c2-'A';
// To Solve Conflict Between Character And Integer
    int y1 = c1;
    int y2= c2;

    /*        PAWN PAWN PAWN PAWN        */

// Checking If It Is Pawn And If True It Must Be At Row 7 That Means x1=6 Else Give Error

    pawnP2(y1,y2,x1,x2);                   //#3rft el deadindex ka static 3shan keda sheloh
    rook2(y1,y2,x1,x2);
    bishop2(y1,y2,x1,x2);
    horse2(y1,y2,x1,x2);
    system("cls");

}
void king(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='k')
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            whiteDanger();
            if(blackMoves[x2+2][y2+2]!=2&&blackMoves[x2+2][y2+2]!=1)    //checks if black would threaten white king in that space
            {
                if(board[x2][y2]!='-'&&board[x2][y2]!='.')
                    deadPieces[deadindex++]=board[x2][y2];
                board[x2][y2]='k';
                board[x1][y1]=boardlayout[x1][y1];
                kingsindex[0]=x2;
                kingsindex[1]=y2;
            }
        }
        else
        {
            player1Move();
            return;
        }
    }
    else
    {
        player1Move();
        return;
    }
    if(board[x1][y1]=='K')                   //checks if white pieces would threaten black king in that space
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            blackDanger();

            if(blackMoves[x2+2][y2+2]!=2&&blackMoves[x2+2][y2+2]!=1)    //checks if black would threaten white king in that space
            {
                if(board[x2][y2]!='-'&&board[x2][y2]!='.')
                    deadPieces[deadindex++]=board[x2][y2];
                board[x2][y2]='K';
                board[x1][y1]=boardlayout[x1][y1];
                kingsindex[3]=x2;
                kingsindex[4]=y2;
            }
            else
            {
                player1Move();
                return;
            }
        }
    }
    else
    {
        player2Move();
        return;
    }
}
/*****************************************************************************************************
*this function detects all possible moves that could be made by the enemy team and put  in the       *
*array which is blackMoves so king wont be able to move if the nearby square is 1 which is danger    *
*or 2 which is the current player piece                                                              *
*****************************************************************************************************/
void whiteDanger()
{
    int i,j,r,c;
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)                 //fills array with positions of all pieces the friendly will be 2 enemy pieces will be 3 current king is 4
        {
            if(board[i-2][j-2]=='r')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='q')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='b')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='p')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='h')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='k')
                blackMoves[i][j]=4;
            if(board[i-2][j-2]=='K')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='P')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='B')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='H')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='R')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='Q')
                blackMoves[i][j]=3;
        }
    }
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)
        {
            if(board[i-2][j-2]=='K')
            {
                blackMoves[i+1][j-1]=1;
                blackMoves[i-1][j-1]=1;                 //fills spaces around king with 1
                blackMoves[i][j-1]=1;
                blackMoves[i+1][j+1]=1;
                blackMoves[i-1][j+1]=1;
                blackMoves[i][j+1]=1;
                blackMoves[i][j+1]=1;
                blackMoves[i-1][j]=1;
                blackMoves[i+1][j]=1;
            }
            if(board[i-2][j-2]=='P')
            {
                //fills possible eating moves for pawn with 1
                blackMoves[i+1][j-1]=1;
                blackMoves[i+1][j+1]=1;
            }
            if(board[i-2][j-2]=='H')
            {
                //fills possible eating moves for horse with 1
                blackMoves[i+2][j+1]=1;
                blackMoves[i+2][j-1]=1;
                blackMoves[i+1][j-2]=1;
                blackMoves[i+1][j+2]=1;
                blackMoves[i-1][j+2]=1;
                blackMoves[i-1][j+1]=1;
                blackMoves[i-1][j-1]=1;
                blackMoves[i-1][j-2]=1;
            }

            if(board[i-2][j-2]=='R')
            {
                //fills possible eating moves for rook with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(c=j-3; c>1; c--)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
                for(c=i-1; c<10; c++)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
            }
            if(board[i-2][j-2]=='B')
            {
                //fills possible eating moves for bishop with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                c=j-3;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
                c=j-1;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-1;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-3;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
            }
            if(board[i-2][j-2]=='Q')
            {
                //just copied the code for both rook and bishop
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(c=j-3; c>1; c--)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
                for(c=i-1; c<10; c++)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
                c=j-3;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
                c=j-1;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-1;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-3;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
            }
        }
    }
}
void blackDanger()
{
    int i,j,r,c;
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)                 //fills array with positions of all pieces the friendly will be 2 enemy pieces will be 3 current king is 4
        {
            if(board[i-2][j-2]=='R')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='Q')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='B')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='P')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='H')
                blackMoves[i][j]=2;
            if(board[i-2][j-2]=='K')
                blackMoves[i][j]=4;
            if(board[i-2][j-2]=='k')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='P')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='b')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='h')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='r')
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='q')
                blackMoves[i][j]=3;
        }
    }
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)
        {
            if(board[i-2][j-2]=='k')
            {
                blackMoves[i+1][j-1]=1;
                blackMoves[i-1][j-1]=1;                 //fills spaces around king with 1
                blackMoves[i][j-1]=1;
                blackMoves[i+1][j+1]=1;
                blackMoves[i-1][j+1]=1;
                blackMoves[i][j+1]=1;
                blackMoves[i][j+1]=1;
                blackMoves[i-1][j]=1;
                blackMoves[i+1][j]=1;
            }
            if(board[i-2][j-2]=='p')
            {
                //fills possible eating moves for pawn with 1
                blackMoves[i+1][j-1]=1;
                blackMoves[i+1][j+1]=1;
            }
            if(board[i-2][j-2]=='h')
            {
                //fills possible eating moves for horse with 1
                blackMoves[i+2][j+1]=1;
                blackMoves[i+2][j-1]=1;
                blackMoves[i+1][j-2]=1;
                blackMoves[i+1][j+2]=1;
                blackMoves[i-1][j+2]=1;
                blackMoves[i-1][j+1]=1;
                blackMoves[i-1][j-1]=1;
                blackMoves[i-1][j-2]=1;
            }

            if(board[i-2][j-2]=='r')
            {
                //fills possible eating moves for rook with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(c=j-3; c>1; c--)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
                for(c=i-1; c<10; c++)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
            }
            if(board[i-2][j-2]=='b')
            {
                //fills possible eating moves for bishop with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                c=j-3;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
                c=j-1;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-1;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-3;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
            }
            if(board[i-2][j-2]=='q')
            {
                //just copied the code for both rook and bishop
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=1;
                        break;
                    }
                    blackMoves[r][j]=1;
                }
                for(c=j-3; c>1; c--)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
                for(c=i-1; c<10; c++)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=1;
                        break;
                    }
                    blackMoves[i][c]=1;
                }
                c=j-3;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
                c=j-1;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-1;
                for(r=i-3; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c++;
                }
                c=j-3;
                for(r=i-1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=1;
                        break;
                    }
                    blackMoves[r][c]=1;
                    c--;
                }
            }
        }
    }

}

/**********************************************************************************
*The Horse Has Eight Available Moves At Max if the Square is blocked by one of    *
*  your pieces then it takes input again if not it captures it                    *
***********************************************************************************/
void horse1(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='h')
    {
        if(x2-x1==1&&abs(y2-y1)==2||x2-x1==2&&abs(y2-y1)==1||x2-x1==-1&&abs(y2-y1)==2||x2-x1==-2&&abs(y2-y1)==1)       //its rows and columns change by 1 or 2 (always different)
        {
            if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='b'||board[x2][y2]=='h'||board[x2][y2]=='p'||board[x2][y2]=='r')
            {
                player1Move();
                return ;
            }
            if(board[x2][y2]!='-'&&board[x2][y2]!='.')
                deadPieces[deadindex++]=board[x2][y2];
            board[x2][y2]=board[x1][y1];
            board[x1][y1]=boardlayout[x1][y1];
        }
        else
        {
            player1Move();
            return ;
        }
    }


}
void horse2(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='H')
    {
        if(x2-x1==1&&abs(y2-y1)==2||x2-x1==2&&abs(y2-y1)==1||x2-x1==-1&&abs(y2-y1)==2||x2-x1==-2&&abs(y2-y1)==1)       //its rows and columns change by 1 or 2 (always different)
        {
            if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='B'||board[x2][y2]=='H'||board[x2][y2]=='P'||board[x2][y2]=='R')
            {
                player2Move();
                return ;
            }
            if(board[x2][y2]!='-'&&board[x2][y2]!='.')
                deadPieces[deadindex++]=board[x2][y2];
            board[x2][y2]=board[x1][y1];
            board[x1][y1]=boardlayout[x1][y1];
        }
        else
        {
            player2Move();
            return ;
        }
    }


}
void bishop1(int y1,int y2,int x1,int x2)
{
    if(abs(x2-x1)==abs(y2-y1))
    {
        if(board[x1][y1]=='b'||board[x1][y1]=='q')
        {
            if(abs(x2-x1)==abs(y2-y1))            //if diff between rows=diff between columns then it is correct move else it restarts
            {
                diagonal(y1,y2,x1,x2);
            }
            else
            {
                player1Move();
                return ;
            }
        }
    }
}
void bishop2(int y1,int y2,int x1,int x2)
{
    if(abs(x2-x1)==abs(y2-y1))
    {

        if(board[x1][y1]=='B'||board[x1][y1]=='Q')   //if diff between rows=diff between columns then it is correct move else it restarts
        {
            if(abs(x2-x1)==abs(y2-y1))
            {
                diagonal(y1,y2,x1,x2);
            }
            else
            {
                player2Move();
                return ;
            }
        }
    }
}
/**********************************************************************************
*This Function Moves Both The bishop or The Queen Diagonally for                  *
*both Players if its an empty space it takes it if it is enemy piece it captures  *
*if its this player's piece it restarts and take the player Move Again            *
*if it there are pieces in between the original and destination it restarts too   *
***********************************************************************************/
void diagonal(int y1,int y2,int x1,int x2)
{
    int i,j;
    if(x2>x1)   //checks if  rows inc(moves diagonally downwards)
    {
        if(y2>y1)           //checks if columns inc (right)
        {
            i=x1+1;
            j=y1+1;
            if(board[x1][y1]=='b'||board[x1][y1]=='q')
            {
                //keeps checking if there is a piece in the way to destination
                while(i<x2)
                {
                    if(board[i][j]=='k'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player1Move();
                        return ;
                    }
                    i++;
                    j++;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {
                    player1Move();
                    return ;          //checks if piece of the same color is on destination
                }
            }
            else if(board[x1][y1]=='B'||board[x1][y1]=='Q')
            {
                while(i<x2)  //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='K'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player2Move();
                        return ;
                    }
                    i++;
                    j++;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    player2Move();
                    return ;
                }
            }
        }
        else if(y1>y2) //checks if columns dec (left)
        {
            i=x1+1;
            j=y1-1;
            if(board[x1][y1]=='b'||board[x1][y1]=='q')
            {
                while(i<x2)    //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='k'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player1Move();
                        return ;
                    }
                    i++;
                    j--;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {
                    player1Move();
                    return ;              //checks if piece of the same color is on destination
                }
            }
            else if(board[x1][y1]=='B'||board[x1][y1]=='Q')
            {
                while(i<x2)   //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='K'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player2Move();
                        return ;
                    }
                    i++;
                    j--;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    player2Move();
                    return ;            //checks if piece of the same color is on destination
                }
            }
        }
    }
    else if(x1>x2)  //if rows dec (moves up)
    {
        if(y2>y1)   //if columns  inc (moves right)
        {
            i=x1-1;
            j=y1+1;
            if(board[x1][y1]=='b'||board[x1][y1]=='q')
            {
                while(i>x2)   //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='k'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player1Move();
                        return ;
                    }
                    i--;
                    j++;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {
                    player1Move();
                    return ;             //checks if piece of the same color is on destination
                }
            }
            else if(board[x1][y1]=='B'||board[x1][y1]=='Q')
            {
                while(i>x2)     //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='K'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player1Move();
                        return ;
                    }
                    i--;
                    j++;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    player1Move();
                    return ;             //checks if piece of the same color is on destination
                }
            }
        }
        else if(y1>y2)  //if columns  dec (moves left)
        {
            i=x1-1;
            j=y1-1;
            if(board[x1][y1]=='b'||board[x1][y1]=='q')
            {
                while(i>x2)    //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='k'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player1Move();
                        return ;
                    }
                    i--;
                    j--;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {
                    player1Move();
                    return ;          //checks if piece of the same color is on destination
                }
            }
            else if(board[x1][y1]=='B'||board[x1][y1]=='Q')
            {
                while(i>x2)   //keeps checking if there is a piece in the way to destination
                {
                    if(board[i][j]=='K'||board[i][j]=='Q'||board[i][j]=='q'
                            ||board[i][j]=='r'||board[i][j]=='b'||board[i][j]=='B'
                            ||board[i][j]=='R'||board[i][j]=='p'||board[i][j]=='P'
                            ||board[i][j]=='h'||board[i][j]=='H')
                    {
                        player2Move();
                        return ;
                    }
                    i--;
                    j--;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    player2Move();
                    return ;              //checks if piece of the same color is on destination
                }
            }
        }
    }
    if (board[x1][y1]=='b')
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='b';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if (board[x1][y1]=='B')
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='B';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if (board[x1][y1]=='q')
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='q';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if (board[x1][y1]=='Q')
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='Q';
        board[x1][y1]=boardlayout[x1][y1];
    }
}
void rook2(int y1,int y2,int x1,int x2)
{
    if(x1==x2||y1==y2)
    {
        if(board[x1][y1]=='R'||board[x1][y1]=='Q')              //if black rook
        {
            if(y1==y2||x1==x2)                             //if it will move vertically or horizontally
            {
                straight( y1, y2, x1, x2);
            }

            else                    //if wrong input restarts player 1
            {
                player2Move();
                return ;
            }
        }
    }
}
void rook1(int y1,int y2,int x1,int x2)
{
    if(x1==x2||y1==y2)
    {
        if(board[x1][y1]=='r'||board[x1][y1]=='q')             //if white rook
        {
            if(y1==y2||x1==x2)                             //if it will move vertically or horizontally
            {
                straight( y1, y2, x1, x2);
            }
            else                    //if wrong input restarts player 1
            {
                player1Move();
                return ;
            }
        }
    }

}
/**********************************************************************************
*This Function Moves Both The Rook or The Queen Vertically or Horizontally for    *
*both Players if its an empty space it takes it if it is enemy piece it captures  *
*if its this player's piece it restarts and take the player Move Again            *
*if it there are pieces in between the original and destination it restarts too   *
***********************************************************************************/
void straight(int y1,int y2,int x1,int x2)
{
    int i;
    if(board[x1][y1]=='R'||board[x1][y1]=='Q')
    {
        if(x1==x2)
        {
            if(y2>y1)       //checks if there are pieces between start and end
            {
                i=y1+1;
                for(; i<y2 ; i++)
                {
                    if(board[x1][y1]=='R'||board[x1][y1]=='Q')
                    {
                        if (board[x2][i]=='Q'||board[x2][i]=='R'||board[x2][i]=='B'
                                ||board[x2][i]=='P'||board[x2][i]=='H'||board[x2][i]=='p'
                                ||board[x2][i]=='r'||board[x2][i]=='h'||board[x2][i]=='q'
                                ||board[x2][i]=='b'||board[x2][i]=='K')
                        {
                            player2Move();
                            return ;
                        }
                    }

                }
            }
            if(y1>y2)       //checks if there are pieces between start and end
            {
                i=y2+1;
                for(; i<y1; i++)
                {
                    if(board[x1][y1]=='R'||board[x1][y1]=='Q')
                    {
                        if (board[x2][i]=='Q'||board[x2][i]=='R'||board[x2][i]=='B'
                                ||board[x2][i]=='P'||board[x2][i]=='H'||board[x2][i]=='p'
                                ||board[x2][i]=='r'||board[x2][i]=='h'||board[x2][i]=='q'
                                ||board[x2][i]=='b'||board[x2][i]=='K')
                        {
                            player2Move();
                            return ;
                        }
                    }

                }
            }
        }
        else if (y1==y2)
        {
            if(x2>x1)
            {
                i=x1+1;
                for(; i<x2; i++)                                           //checks if there are pieces between start and end
                {
                    if(board[x1][y1]=='R'||board[x1][y1]=='Q')
                    {
                        if (board[i][y2]=='Q'||board[i][y2]=='R'||board[i][y2]=='B'
                                ||board[i][y2]=='P'||board[i][y2]=='H'||board[i][y2]=='p'
                                ||board[i][y2]=='r'||board[i][y2]=='h'||board[i][y2]=='q'
                                ||board[i][y2]=='b'||board[i][y2]=='K')
                        {
                            player2Move();
                            return ;
                        }
                    }
                }
            }
            else if(x1>x2)
            {
                i=x2+1;
                for(; i<x1; i++)                                           //checks if there are pieces between start and end
                {
                    if(board[x1][y1]=='R'||board[x1][y1]=='Q')
                    {
                        if (board[i][y2]=='Q'||board[i][y2]=='R'||board[i][y2]=='B'
                                ||board[i][y2]=='P'||board[i][y2]=='H'||board[i][y2]=='p'
                                ||board[i][y2]=='r'||board[i][y2]=='h'||board[i][y2]=='q'
                                ||board[i][y2]=='b'||board[i][y2]=='K')
                        {
                            player2Move();
                            return ;
                        }
                    }
                }
            }
            if(board[x2][y2]=='R'||board[x2][y2]=='B'||board[x2][y2]=='P'||board[x2][y2]=='K'||board[x2][y2]=='H'||board[x2][y2]=='Q')
            {
                player2Move();
                return ;                //checks if destination has same color of the piece if so it restarts as cannot capture
            }
        }
    }
    if (board[x1][y1]=='r'||board[x1][y1]=='q')
    {
        if(x1==x2)
        {
            if(y2>y1)
            {
                i=y1+1;
                for(; i<y2; i++)                       //checks if there are pieces between start and end
                {
                    if(board[x1][y1]=='r'||board[x1][y1]=='q')
                    {
                        if (board[x2][i]=='Q'||board[x2][i]=='R'||board[x2][i]=='B'
                                ||board[x2][i]=='P'||board[x2][i]=='H'||board[x2][i]=='p'
                                ||board[x2][i]=='r'||board[x2][i]=='h'||board[x2][i]=='q'
                                ||board[x2][i]=='b'||board[x2][i]=='k')
                        {
                            player1Move();
                            return ;
                        }
                    }
                }
            }
            else if(y1>y2)
            {
                i=y2+1;
                for(; i<y1; i++)            //checks if there are pieces between start and end if so restart
                {
                    if(board[x1][y1]=='r'||board[x1][y1]=='q')
                    {
                        if (board[x2][i]=='Q'||board[x2][i]=='R'||board[x2][i]=='B'
                                ||board[x2][i]=='P'||board[x2][i]=='H'||board[x2][i]=='p'
                                ||board[x2][i]=='r'||board[x2][i]=='h'||board[x2][i]=='q'
                                ||board[x2][i]=='b'||board[x2][i]=='k')
                        {
                            player1Move();
                            return ;
                        }
                    }
                }
            }
        }
        else if (y1==y2)
        {
            if(x2>x1)
            {
                i=x1+1;
                for(; i<x2; i++)
                {
                    if(board[x1][y1]=='r'||board[x1][y1]=='q')          //checks if there are pieces between start and end
                    {
                        if (board[i][y2]=='Q'||board[i][y2]=='R'||board[i][y2]=='B'
                                ||board[i][y2]=='P'||board[i][y2]=='H'||board[i][y2]=='p'
                                ||board[i][y2]=='r'||board[i][y2]=='h'||board[i][y2]=='q'
                                ||board[i][y2]=='b'||board[i][y2]=='k')
                        {
                            player1Move();
                            return ;
                        }

                    }
                }
            }
            else  if(x1>x2)
            {
                i=x2+1;
                for(; i<x1; i++)
                {
                    if(board[x1][y1]=='r'||board[x1][y1]=='q')          //checks if there are pieces between start and end
                    {
                        if (board[i][y2]=='Q'||board[i][y2]=='R'||board[i][y2]=='B'
                                ||board[i][y2]=='P'||board[i][y2]=='H'||board[i][y2]=='p'
                                ||board[i][y2]=='r'||board[i][y2]=='h'||board[i][y2]=='q'
                                ||board[i][y2]=='b'||board[i][y2]=='k')
                        {
                            player1Move();
                            return ;
                        }

                    }
                }
            }

        }
        if(board[x2][y2]=='r'||board[x2][y2]=='b'||board[x2][y2]=='p'||board[x2][y2]=='k'||board[x2][y2]=='h'||board[x2][y2]=='q')
        {
            player1Move();
            return ;                             //checks if destination has same color of the piece if so it restarts as cannot capture
        }
    }

    if(board[x1][y1]=='R')   //checks if piece is rook then replaces with whatever on the other location
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='R';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if(board[x1][y1]=='r')
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='r';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if(board[x1][y1]=='Q')   //checks if piece is Queen then replaces with whatever on the other location
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='Q';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if(board[x1][y1]=='q')
    {
        if(board[x2][y2]!='-'&&board[x2][y2]!='.')
            deadPieces[deadindex++]=board[x2][y2];
        board[x2][y2]='q';
        board[x1][y1]=boardlayout[x1][y1];
    }
}

void CheckStalemate()
{
    // 1 - LACK OF CHECK MATING MATERIAL FOR PLAYER 1
        // 1 KING VS 1 KING
    int i,count=0;
    for(i=0;i<32;i++)
    {
        if(deadPieces[i] == 'p' || deadPieces[i] == 'r' || deadPieces[i] == 'h' || deadPieces[i] == 'b' ||deadPieces[i] == 'q' )
        {
            count++;
        }
        if(deadPieces[i] == 'P' || deadPieces[i] == 'R' || deadPieces[i] == 'H' || deadPieces[i] == 'B' ||deadPieces[i] == 'Q' )
        {
            count++;
        }

    }

    if(count==30)
    {
            stalemate=true;
            gameOver=true;
    }


        // 1 KING 1 BISHOP VS 1 KING
    i=0,count=0;
    for(i=0;i<32;i++)
    {
        if(deadPieces[i] == 'p' || deadPieces[i] == 'r' || deadPieces[i] == 'h'  ||deadPieces[i] == 'q' )
        {
            count++;
        }
        if(deadPieces[i] == 'P' || deadPieces[i] == 'R' || deadPieces[i] == 'H'  ||deadPieces[i] == 'Q' )
        {
            count++;
        }

        if(deadPieces[i] == 'B' || deadPieces[i] == 'b')
        {
            count++ ;// if 3 there is one bishop on the board and 2 kings
        }
    }

    if(count==29)
    {

            stalemate=true;
            gameOver=true;
    }

        // 1 KINGHT AND 1 KING VS 1 KING
    i=0,count=0;
    for(i=0;i<32;i++)
    {
        if(deadPieces[i] == 'p' || deadPieces[i] == 'r' || deadPieces[i] == 'b' ||deadPieces[i] == 'q' )
        {
            count++;
        }
        if(deadPieces[i] == 'P' || deadPieces[i] == 'R' ||  deadPieces[i] == 'B' ||deadPieces[i] == 'Q' )
        {
            count++;
        }
        if(deadPieces[i] == 'H' || deadPieces[i] == 'h')
        {
            count++ ;// if 3 there is one horse on the board and 2 kings
        }
    }

    if(count==29)
    {
            stalemate=true;
            gameOver=true;
    }

        // 2  KNIGHTS AND 1 KING VS 1 KING
    i=0,count=0;
    int countCapital=0,countSmall=0;
    for(i=0;i<32;i++)
    {
        if(deadPieces[i] == 'p' || deadPieces[i] == 'r' || deadPieces[i] == 'b' ||deadPieces[i] == 'q' )
        {
            count++;
        }
        if(deadPieces[i] == 'P' || deadPieces[i] == 'R' ||  deadPieces[i] == 'B' ||deadPieces[i] == 'Q' )
        {
            count++;
        }
        if(deadPieces[i] == 'H')
        {
            countCapital++ ;// if 2 there is 2 While Knights on the board
        }
        if(deadPieces[i] == 'h')
        {
            countSmall++;
        }
    }

    if(count==26 && countCapital==2 && countSmall==0)
    {
            stalemate=true;
            gameOver=true;
    }
    else if(count==26 && countCapital==0 && countSmall==2)
    {
            stalemate=true;
            gameOver=true;
    }
    else
    {
        return ;
    }

}
int main()
{

    // Functions Call
    while(gameOver==false)
    {
        CheckStalemate();
        player1Move();
        if(gameOver==true)
            break;
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
