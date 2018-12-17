#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
int ki[4] = {0,1,7,1};//{7,4,0,4};
bool stalemate=false,
     player1Win=false,
     player2Win=false,
     gameOver=false;
int n=8;
int m=8;
int deadindex=0;
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

char deadPieces[48]= {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '
                      ,' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '
                      ,' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',
                      ' ',' ',' ',' ',' ',' ',' ',' ',' ',' '
                     };

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

int resetMoves[12][12]= {{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1}
};
int blackMoves[12][12]= {{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1}
};
int whiteMoves[12][12]= {{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,0,0,0,0,0,0,0,0,1,1}
    ,{1,1,0,0,0,0,0,0,0,0,1,1},{1,1,1,1,1,1,1,1,1,1,1,1},{1,1,1,1,1,1,1,1,1,1,1,1}
};

void printboard()
{
    /*  ***************************************************** PLAYER 1 RELATED ******************************************************* */
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
                return;
            }
        }
        if(x1<6)
        {
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
                    return;
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
                return;
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
void stalemateblack()
{
    int i,j,c1,r1,r2,c2;
    gameOver=true;
    stalemate=true;
    for(r1=0; r1<8; r1++)
    {
        for(c1=0; c1<8; c1++)
        {
            if(board[r1][c1]=='K'||board[r1][c1]=='Q'||board[r1][c1]=='R'||board[r1][c1]=='B'||board[r1][c1]=='H'||board[r1][c1]=='P')
            {
                for(r2=0; r2<8; r2++)
                {
                    for(c2=0; c2<8; c2++)
                    {   if(r2==r1&&c2==c1)continue;

                        if((board[r1][c1]=='R'||board[r1][c1]=='Q')&&(r2==r1||c1==c2))
                            rook2incheck(c1,c2,r1,r2);
                        else if(board[r1][c1]=='B'||board[r1][c1]=='Q')
                            bishop2incheck(c1,c2,r1,r2);
                        else if(board[r1][c1]=='H')
                            horse2incheck(c1,c2,r1,r2);
                        else if(board[r1][c1]=='P')
                            pawnP1(c1,c2,r1,r2);
                        else if(board[r1][c1]=='K')
                        {
                            kingincheck(c1,c2,r1,r2);
                            checkWhiteMoves();
                            if(whiteMoves[r2+2][c2+2]==1)
                            {
                                kingincheck(c2,c1,r2,r1);
                            }
                        }
                        if(board[r1][c1]==boardlayout[r1][c1]&&(board[r2][c2]=='R'||board[r2][c2]=='K'||board[r2][c2]=='B'||board[r2][c2]=='H'||board[r2][c2]=='Q'||board[r2][c2]=='P'))
                        {

                            for(i=0; i<8; i++)
                            {
                                for(j=0; j<8; j++)
                                {
                                    board[i][j]=temparr[i][j];
                                }
                            }
                            stalemate=false;
                            gameOver=false;
                            return;
                        }
                    }
                }
            }
        }
    }
}
void stalematewhite()
{
    int i,j,c1,r1,r2,c2;
    gameOver=true;
    stalemate=true;
    for(r1=0; r1<8; r1++)
    {
        for(c1=0; c1<8; c1++)
        {
            if(board[r1][c1]=='k'||board[r1][c1]=='q'||board[r1][c1]=='r'||board[r1][c1]=='b'||board[r1][c1]=='h'||board[r1][c1]=='p')
            {
                for(r2=0; r2<8; r2++)
                {
                    for(c2=0; c2<8; c2++)
                    {   if(r2==r1&&c2==c1)continue;

                        if((board[r1][c1]=='r'||board[r1][c1]=='q')&&(r2==r1||c1==c2))
                            rook1incheck(c1,c2,r1,r2);
                        else if(board[r1][c1]=='b'||board[r1][c1]=='q')
                            bishop1incheck(c1,c2,r1,r2);
                        else if(board[r1][c1]=='h')
                            horse1incheck(c1,c2,r1,r2);
                        else if(board[r1][c1]=='p')
                            pawnP1(c1,c2,r1,r2);
                        else if(board[r1][c1]=='k')
                        {
                            kingincheck(c1,c2,r1,r2);
                            checkBlackMoves();
                            if(blackMoves[r2+2][c2+2]==1)
                            {
                                kingincheck(c2,c1,r2,r1);
                            }
                        }
                        if((board[r1][c1]==boardlayout[r1][c1])&&(board[r2][c2]=='r'||board[r2][c2]=='k'||board[r2][c2]=='b'||board[r2][c2]=='h'||board[r2][c2]=='q'||board[r2][c2]=='p'))
                        {

                            for(i=0; i<8; i++)
                            {
                                for(j=0; j<8; j++)
                                {
                                    board[i][j]=temparr[i][j];
                                }
                            }
                            stalemate=false;
                            gameOver=false;
                            return;
                        }
                    }
                }
            }
        }
    }
}

void checkmateblack()
{
    int i,j,c1,r1,r2,c2;
    for(i=0; i<8; i++)
    {
        for(j=0; j<8; j++)
        {
            temparr[i][j]=board[i][j];
        }
    }
    if(whiteMoves[ki[2]+2][ki[3]+2]==1)
    {
        gameOver=true;
        player1Win=true;
        for(r1=0; r1<8; r1++)
        {
            for(c1=0; c1<8; c1++)
            {
                if(board[r1][c1]=='K'||board[r1][c1]=='Q'||board[r1][c1]=='R'||board[r1][c1]=='B'||board[r1][c1]=='H'||board[r1][c1]=='P')
                {
                    for(r2=0; r2<8; r2++)
                    {
                        for(c2=0; c2<8; c2++)
                        {   if(r2==r1&&c2==c1)continue;
                            for(i=0; i<8; i++)
                            {
                                for(j=0; j<8; j++)
                                {
                                    board[i][j]=temparr[i][j];
                                }
                            }
                            if((board[r1][c1]=='R'||board[r1][c1]=='Q')&&(r2==r1||c1==c2))
                                rook1incheck(c1,c2,r1,r2);
                            else if(board[r1][c1]=='B'||board[r1][c1]=='Q')
                                bishop1incheck(c1,c2,r1,r2);
                            else if(board[r1][c1]=='H')
                                horse1incheck(c1,c2,r1,r2);
                            else if(board[r1][c1]=='P')
                                pawnP1(c1,c2,r1,r2);
                            else if(board[r1][c1]=='K')
                                kingincheck(c1,c2,r1,r2);
                            checkWhiteMoves();
                            if(whiteMoves[ki[2]+2][ki[3]+2]!=1)
                            {
                                gameOver=false;
                                player1Win=false;
                                for(i=0; i<8; i++)
                                {
                                    for(j=0; j<8; j++)
                                    {
                                        board[i][j]=temparr[i][j];

                                    }
                                }
                                return;
                            }
                        }
                    }
                }
            }
        }
    }
}
void checkmateWhite()
{
    int i,j,r1,r2,c1,c2;
    for(i=0; i<8; i++)
    {
        for(j=0; j<8; j++)
        {
            temparr[i][j]=board[i][j];
        }
    }
    if(blackMoves[ki[0]+2][ki[1]+2]==1)
    {
        gameOver=true;
        player2Win=true;
        for(r1=0; r1<8; r1++)
        {
            for(c1=0; c1<8; c1++)
            {
                if(board[r1][c1]=='k'||board[r1][c1]=='q'||board[r1][c1]=='r'||board[r1][c1]=='b'||board[r1][c1]=='h'||board[r1][c1]=='p')
                {
                    for(r2=0; r2<8; r2++)
                    {
                        for(c2=0; c2<8; c2++)
                        {   if(r2==r1&&c2==c1)continue;
                            for(i=0; i<8; i++)
                            {
                                for(j=0; j<8; j++)
                                {
                                    board[i][j]=temparr[i][j];
                                }
                            }
                            if((board[r1][c1]=='r'||board[r1][c1]=='q')&&(r2==r1||c1==c2))
                                rook2incheck(c1,c2,r1,r2);
                            else if(board[r1][c1]=='b'||board[r1][c1]=='q')
                                bishop2incheck(c1,c2,r1,r2);
                            else if(board[r1][c1]=='h')
                                horse2incheck(c1,c2,r1,r2);
                            else if(board[r1][c1]=='p')
                                pawnP2(c1,c2,r1,r2);
                            else if(board[r1][c1]=='k')
                                kingincheck(c1,c2,r1,r2);
                            checkBlackMoves();
                            if(blackMoves[ki[0]+2][ki[1]+2]!=1)
                            {
                                gameOver=false;
                                player2Win=false;
                                for(i=0; i<8; i++)
                                {
                                    for(j=0; j<8; j++)
                                    {
                                        board[i][j]=temparr[i][j];

                                    }
                                }
                                return;
                            }
                        }
                    }
                }
            }
        }
    }
}
void player1Move()
{
    int i,j;
    for(i=0; i<8; i++)
    {
        for(j=0; j<8; j++)
        {
            temparr[i][j]=board[i][j];
        }
    }
    int n1,n2;
    printboard();
    checkBlackMoves();
    stalematewhite();
    if(blackMoves[ki[0]+2][ki[1]+2]==1)
    {
        checkmateWhite();
        if(gameOver==false)
            printf("\n\nWhite Check\n\n");
    }
    if(gameOver==true)
    {
        return;
    }
    char c1,c2,c3;
// Positions
    printf("\n\nPlayer1:");
    scanf(" %c%d%c%d",&c1,&n1,&c2,&n2);
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
    if(board[x1][y1]=='p')
        pawnP1(y1,y2,x1,x2);
    else if((board[x1][y1]=='r'||board[x1][y1]=='q')&&((x1==x2)||(y2==y1)))
        rook1(y1,y2,x1,x2);
    else if((board[x1][y1]=='b'||board[x1][y1]=='q')&&(abs(x2-x1)==abs(y2-y1)))
        bishop1(y1,y2,x1,x2);
    else if(board[x1][y1]=='h')
        horse1(y1,y2,x1,x2);
    else if(board[x1][y1]=='k')
        king(y1,y2,x1,x2);
    else
    {
        system("cls");
        player1Move();
        return ;
    }
    system("cls");
    checkBlackMoves();
    while(blackMoves[ki[0]+2][ki[1]+2]==1)
    {
        for(i=0; i<8; i++)
        {
            for(j=0; j<8; j++)
            {
                board[i][j]=temparr[i][j];
            }
        }
        printf("\nCan't Make this Move As it results in Check.\n");
        player1Move();
        return;
    }
}

void pawnP2(int y1,int y2,int x1,int x2)
{
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
                    return;
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
                return;

            }
        }

        if(x1>1)
        {

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
                    return;
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
                return;

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
    int n1,n2,i,j;
    for(i=0; i<8; i++)
    {
        for(j=0; j<8; j++)
        {
            temparr[i][j]=board[i][j];
        }
    }
    char c1,c2;
    printboard();
    checkWhiteMoves();
    stalemateblack();
    if(whiteMoves[ki[2]+2][ki[3]+2]==1)
    {
        checkmateblack();
        if(gameOver==false)
            printf("\n\nBlack Check\n\n");
    }
    if(gameOver==true)
    {
        return;
    }
// Positions
    printf("\n\nPlayer2:");
    scanf(" %c%d%c%d",&c1,&n1,&c2,&n2);
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
    if(board[x1][y1]=='P')
        pawnP2(y1,y2,x1,x2);
    else if((board[x1][y1]=='R'||board[x1][y1]=='Q')&&((x2==x1)||(y2==y1)))
        rook2(y1,y2,x1,x2);
    else if((board[x1][y1]=='B'||board[x1][y1]=='Q')&&(abs(x2-x1)==abs(y2-y1)))
        bishop2(y1,y2,x1,x2);
    else if(board[x1][y1]=='H')
        horse2(y1,y2,x1,x2);
    else if(board[x1][y1]=='K')
        king(y1,y2,x1,x2);
    else
    {
        system("cls");
        player2Move();
        return ;
    }
    system("cls");
    checkWhiteMoves();
    while(whiteMoves[ki[2]+2][ki[3]+2]==1)
    {
        printf("Can't move as it results in check.\n");
        for(i=0; i<8; i++)
        {
            for(j=0; j<8; j++)
            {
                board[i][j]=temparr[i][j];
            }
        }
        player2Move();
        return;
    }
}


void kingincheck(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='k')
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            if(blackMoves[x2+2][y2+2]!=5
                    &&blackMoves[x2+2][y2+2]!=3&&whiteMoves[x2+2][y2+2]!=1)    //checks if black would threaten white king in that space
            {

                board[x2][y2]='k';
                board[x1][y1]=boardlayout[x1][y1];
                ki[0]=x2;
                ki[1]=y2;
            }
        }
    }
    else if(board[x1][y1]=='K')                   //checks if white pieces would threaten black king in that space
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            if(whiteMoves[x2+2][y2+2]!=3&&whiteMoves[x2+2][y2+2]!=5
                    &&whiteMoves[x2+2][y2+2]!=1)    //checks if white would threaten black king in that space
            {
                board[x2][y2]='K';
                board[x1][y1]=boardlayout[x1][y1];
                ki[2]=x2;
                ki[3]=y2;
            }
        }
    }
}
void king(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='k')
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            if(!((board[x2][y2]=='r'||
               board[x2][y2]=='h'||board[x2][y2]=='p'||
               board[x2][y2]=='q'||board[x2][y2]=='b')
               ||blackMoves[x2+2][y2+2]==3||blackMoves[x2+2][y2+2]==1))    //checks if white would threaten black king in that space
            {
                if(board[x2][y2]!='-'&&board[x2][y2]!='.')
                    deadPieces[deadindex++]=board[x2][y2];
                board[x2][y2]='k';
                board[x1][y1]=boardlayout[x1][y1];
                ki[0]=x2;
                ki[1]=y2;
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
    }
    else if(board[x1][y1]=='K')                   //checks if white pieces would threaten black king in that space
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            if(!((board[x2][y2]=='R'||
               board[x2][y2]=='H'||board[x2][y2]=='P'||
               board[x2][y2]=='Q'||board[x2][y2]=='B')
               ||whiteMoves[x2+2][y2+2]==3||whiteMoves[x2+2][y2+2]==1))    //checks if white would threaten black king in that space
            {
                if(board[x2][y2]!='-'&&board[x2][y2]!='.')
                    deadPieces[deadindex++]=board[x2][y2];
                board[x2][y2]='K';
                board[x1][y1]=boardlayout[x1][y1];
                ki[2]=x2;
                ki[3]=y2;
            }
            else
            {
                player2Move();
                return;
            }

        }
        else
        {
            player2Move();
            return;
        }

    }
}
/*****************************************************************************************************
*this function detects all possible moves that could be made by the enemy team and put  in the       *
*array which is blackMoves so king wont be able to move if the nearby square is 1 which is danger    *
*or 2 which is the current player piece                                                              *
*****************************************************************************************************/
void checkBlackMoves()
{
    int i,j;
    for(i=0; i<12; i++)
    {
        for(j=0; j<12; j++)
        {
            blackMoves[i][j]=resetMoves[i][j];
        }
    }
    for(i=0; i<8; i++)
        for(j=0; j<8; j++)
        {
            if(board[i][j]=='h'||board[i][j]=='q'||board[i][j]=='p'||board[i][j]=='b'||board[i][j]=='r'||board[i][j]=='k')
                blackMoves[i+2][j+2]=5;
            if(board[i][j]=='H')
                blackMoves[i+2][j+2]=3;
        }
    j=ki[1];
    for(i=ki[0]+1; i<8; i++)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='R'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    j=ki[1];
    for(i=ki[0]-1; i>=0; i--)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='R'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    i=ki[0];
    for(j=ki[1]+1; j<8; j++)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='R'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    i=ki[0];
    for(j=ki[1]-1; j>=0; j--)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='R'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    if(ki[0]>1)
    {
        if(board[ki[0]-1][ki[1]-1]=='P'||board[ki[0]-1][ki[1]+1]=='P')
        {
            blackMoves[ki[0]+2][ki[1]+2]=1;
            return;
        }
    }
    if(blackMoves[ki[0]+1+2][ki[1]+2-2]==3||blackMoves[ki[0]+1+2][ki[1]+1+2]==3||blackMoves[ki[0]-1+2][ki[1]+2+2]==3
            ||blackMoves[ki[0]-1+2][ki[1]+2-2]==3||blackMoves[ki[0]+2+2][ki[1]+2+1]==3
            ||blackMoves[ki[0]+2+2][ki[1]+2-1]==3||blackMoves[ki[0]+2-2][ki[1]+2-1]==3||blackMoves[ki[0]+2-2][ki[1]+2+1]==3)
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    j=ki[1]-1;
    i=ki[0]-1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j--;
        i--;
    }
    if(board[i][j]=='B'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    j=ki[1]+1;
    i=ki[0]+1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j++;
        i++;
    }
    if(board[i][j]=='B'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    j=ki[1]-1;
    i=ki[0]+1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j--;
        i++;
    }
    if(board[i][j]=='B'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
    j=ki[1]+1;
    i=ki[0]-1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j++;
        i--;
    }
    if(board[i][j]=='B'||board[i][j]=='Q')
    {
        blackMoves[ki[0]+2][ki[1]+2]=1;
        return;
    }
}
void checkWhiteMoves()
{
    int i,j;
    for(i=0; i<12; i++)
    {
        for(j=0; j<12; j++)
        {
            whiteMoves[i][j]=resetMoves[i][j];
        }
    }
    for(i=0; i<8; i++)
        for(j=0; j<8; j++)
        {

            if(board[i][j]=='H'||board[i][j]=='Q'||board[i][j]=='P'||board[i][j]=='B'||board[i][j]=='R')
                whiteMoves[i+2][j+2]=5;
            if(board[i][j]=='h')
                whiteMoves[i+2][j+2]=3;
        }
    j=ki[3];
    for(i=ki[2]+1; i<8; i++)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='r'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    j=ki[3];
    for(i=ki[2]-1; i>-1; i--)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='r'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    i=ki[2];
    for(j=ki[3]+1; j<8; j++)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='r'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    i=ki[2];
    for(j=ki[3]-1; j>=0; j--)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
    }
    if(board[i][j]=='r'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    if(ki[2]<7)
    {
        if(board[ki[2]+1][ki[3]-1]=='p'||board[ki[2]+1][ki[3]+1]=='p')
        {
            whiteMoves[ki[2]+2][ki[3]+2]=1;
            return;
        }
    }
    if(whiteMoves[ki[2]+1+2][ki[3]+2-2]==3||whiteMoves[ki[2]+1+2][ki[3]+1+2]==3||whiteMoves[ki[2]-1+2][ki[3]+2+2]==3
            ||whiteMoves[ki[2]-1+2][ki[3]+2-2]==3||whiteMoves[ki[2]+2+2][ki[3]+2+1]==3
            ||whiteMoves[ki[2]+2+2][ki[3]+2-1]==3||whiteMoves[ki[2]+2-2][ki[3]+2-1]==3||whiteMoves[ki[2]+2-2][ki[3]+2+1]==3)
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    j=ki[3]-1;
    i=ki[2]-1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j--;
        i--;
    }
    if(board[i][j]=='b'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    j=ki[3]+1;
    i=ki[2]+1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j++;
        i++;
    }
    if(board[i][j]=='b'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    j=ki[3]-1;
    i=ki[2]+1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j--;
        i++;
    }
    if(board[i][j]=='b'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
    j=ki[3]+1;
    i=ki[2]-1;
    while(i>=0&&j>=0)
    {
        if(board[i][j]!='.'&&board[i][j]!='-')
            break;
        j++;
        i--;
    }
    if(board[i][j]=='b'||board[i][j]=='q')
    {
        whiteMoves[ki[2]+2][ki[3]+2]=1;
        return;
    }
}
/**********************************************************************************
*The Horse Has Eight Available Moves At Max if the Square is blocked by one of    *
*  your pieces then it takes input again if not it captures it                    *
***********************************************************************************/
void horse1incheck(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='h')
    {
        if(x2-x1==1&&abs(y2-y1)==2||x2-x1==2&&abs(y2-y1)==1||x2-x1==-1&&abs(y2-y1)==2||x2-x1==-2&&abs(y2-y1)==1)       //its rows and columns change by 1 or 2 (always different)
        {
            if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='b'||board[x2][y2]=='h'||board[x2][y2]=='p'||board[x2][y2]=='r')
            {
                return ;
            }
            board[x2][y2]=board[x1][y1];
            board[x1][y1]=boardlayout[x1][y1];
        }
        else
        {
            return ;
        }
    }
}
void horse2incheck(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='H')
    {
        if(x2-x1==1&&abs(y2-y1)==2||x2-x1==2&&abs(y2-y1)==1||x2-x1==-1&&abs(y2-y1)==2||x2-x1==-2&&abs(y2-y1)==1)       //its rows and columns change by 1 or 2 (always different)
        {
            if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='B'||board[x2][y2]=='H'||board[x2][y2]=='P'||board[x2][y2]=='R')
            {

                return ;
            }
            board[x2][y2]=board[x1][y1];
            board[x1][y1]=boardlayout[x1][y1];
        }
        else
        {

            return ;
        }
    }


}
void bishop1incheck(int y1,int y2,int x1,int x2)
{
    if(abs(x2-x1)==abs(y2-y1))
    {
        if(board[x1][y1]=='b'||board[x1][y1]=='q')
        {
            if(abs(x2-x1)==abs(y2-y1))            //if diff between rows=diff between columns then it is correct move else it restarts
            {
                diagonalincheck(y1,y2,x1,x2);
            }
            else
            {
                return ;
            }
        }
    }
}
void bishop2incheck(int y1,int y2,int x1,int x2)
{
    if(abs(x2-x1)==abs(y2-y1))
    {

        if(board[x1][y1]=='B'||board[x1][y1]=='Q')   //if diff between rows=diff between columns then it is correct move else it restarts
        {
            if(abs(x2-x1)==abs(y2-y1))
            {
                diagonalincheck(y1,y2,x1,x2);
            }
            else
            {
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
void diagonalincheck(int y1,int y2,int x1,int x2)
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

                        return ;
                    }
                    i++;
                    j++;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {

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

                        return ;
                    }
                    i++;
                    j++;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {

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

                        return ;
                    }
                    i++;
                    j--;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {

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

                        return ;
                    }
                    i++;
                    j--;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {

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
                        return ;
                    }
                    i--;
                    j++;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {
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
                        return ;
                    }
                    i--;
                    j++;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
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
                        return ;
                    }
                    i--;
                    j--;
                }
                if(board[x2][y2]=='k'||board[x2][y2]=='q'||board[x2][y2]=='p'||board[x2][y2]=='b'||board[x2][y2]=='r')
                {

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

                        return ;
                    }
                    i--;
                    j--;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    return ;              //checks if piece of the same color is on destination
                }
            }
        }
    }
    if (board[x1][y1]=='b')
    {
        board[x2][y2]='b';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if (board[x1][y1]=='B')
    {
        board[x2][y2]='B';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if (board[x1][y1]=='q')
    {
        board[x2][y2]='q';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if (board[x1][y1]=='Q')
    {
        board[x2][y2]='Q';
        board[x1][y1]=boardlayout[x1][y1];
    }
}
void rook2incheck(int y1,int y2,int x1,int x2)
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
                return ;
            }
        }
    }
}
void rook1incheck(int y1,int y2,int x1,int x2)
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
void straightincheck(int y1,int y2,int x1,int x2)
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
                            return ;
                        }
                    }
                }
            }
            if(board[x2][y2]=='R'||board[x2][y2]=='B'||board[x2][y2]=='P'||board[x2][y2]=='K'||board[x2][y2]=='H'||board[x2][y2]=='Q')
            {
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

                            return ;
                        }

                    }
                }
            }

        }
        if(board[x2][y2]=='r'||board[x2][y2]=='b'||board[x2][y2]=='p'||board[x2][y2]=='k'||board[x2][y2]=='h'||board[x2][y2]=='q')
        {
            return ;                             //checks if destination has same color of the piece if so it restarts as cannot capture
        }
    }

    if(board[x1][y1]=='R')   //checks if piece is rook then replaces with whatever on the other location
    {
        board[x2][y2]='R';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if(board[x1][y1]=='r')
    {
        board[x2][y2]='r';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if(board[x1][y1]=='Q')   //checks if piece is Queen then replaces with whatever on the other location
    {
        board[x2][y2]='Q';
        board[x1][y1]=boardlayout[x1][y1];
    }
    else if(board[x1][y1]=='q')
    {
        board[x2][y2]='q';
        board[x1][y1]=boardlayout[x1][y1];
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

int main()
{
    // Functions Call
    while(gameOver==false)
    {
        player1Move();
        if(gameOver==true)
            break;
        player2Move();
    }
    if (player1Win==true)
    {
        printf("\nCheckmate.\n");
        printf("Player 1 Wins.");
    }
    else if (player2Win==true)
    {
        printf("\n\nCheckmate.\n");
        printf("Player 2 Wins.");
    }
    else
    {
        printf("Stalemate");
    }

    return 0;
}
