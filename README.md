#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
int ki[4] = {7,4,0,4};
bool stalemate=false,
     player1Win=false,
     player2Win=false,
     gameOver=false;
int pawnpossiblemoves[8][8]={{0,0,0,0,0,0,0,0},{0,0,0,0,0,0,0,0},{0,0,0,0,0,0,0,0},{0,0,0,0,0,0,0,0},
{0,0,0,0,0,0,0,0},{0,0,0,0,0,0,0,0},{0,0,0,0,0,0,0,0},{0,0,0,0,0,0,0,0}};
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

char deadPieces[32]= {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '
                      ,' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '
                      ,' ',' ',' ',' ',' ',' ',' '
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
char cb[12][12]= {{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},
    {' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '},{' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '}
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
int n=8;
int m=8;
int deadindex=0;
void pawnmoves()
{   int i,j;
    for(i=0;i<8;i++)
    {
        for(j=0;j<8;j++)
        {
            pawnpossiblemoves[i][j]=0;
        }
    }

    for(i=0;i<8;i++)
    {
        for(j=0;j<8;j++)
        {
            if(board[i][j]=='p'&&(board[i-1][j]=='.'||board[i-1][j]=='-'))
            {
                pawnpossiblemoves[i-1][j]=2;
            }
            if(board[i][j]=='p'&&i==6&&(board[i-2][j]=='.'||board[i-2][j]=='-'))
            {
                pawnpossiblemoves[i-2][j]=2;
            }
            if(board[i][j]=='P'&&(board[i+1][j]=='.'||board[i+1][j]=='-'))
            {
                pawnpossiblemoves[i+1][j]=1;
            }
            if(board[i][j]=='P'&&i==2&&(board[i+2][j]=='.'||board[i+2][j]=='-'))
            {
                pawnpossiblemoves[i+2][j]=1;
            }
        }
    }
}
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
bool ifcondblack(int i,int j)
{
    bool z=false;
    if(pawnpossiblemoves[ki[0]+i][ki[1]+j]==1||blackMoves[ki[0]+2+i][ki[1]+2+j]==6||
            blackMoves[ki[0]+2+i][ki[1]+2+j]==7||blackMoves[ki[0]+2+i][ki[1]+2+j]==8||
            blackMoves[ki[0]+2+i][ki[1]+2+j]==9)
        z=true;
    return z;

}
bool ifcondblackkill(int i,int j)
{
    bool z=false;
    if(blackMoves[ki[0]+2+i][ki[1]+2+j]==5||blackMoves[ki[0]+2+i][ki[1]+2+j]==6||
            blackMoves[ki[0]+2+i][ki[1]+2+j]==7||blackMoves[ki[0]+2+i][ki[1]+2+j]==8||
            blackMoves[ki[0]+2+i][ki[1]+2+j]==9)
        z=true;
    return z;

}
bool ifcondwhite(int i,int j)
{
    bool z=false;
    if(pawnpossiblemoves[ki[2]+i][ki[3]+j]==2||whiteMoves[ki[2]+2+i][ki[3]+2+j]==6||
            whiteMoves[ki[2]+2+i][ki[3]+2+j]==7||whiteMoves[ki[2]+2+i][ki[3]+2+j]==8||
            whiteMoves[ki[2]+2+i][ki[3]+2+j]==9)
        z=true;
    return z;

}
bool ifcondwhitekill(int i,int j)
{
    bool z=false;
    if(whiteMoves[ki[2]+2+i][ki[3]+2+j]==5||whiteMoves[ki[2]+2+i][ki[3]+2+j]==6||
            whiteMoves[ki[2]+2+i][ki[3]+2+j]==7||whiteMoves[ki[2]+2+i][ki[3]+2+j]==8||
            whiteMoves[ki[2]+2+i][ki[3]+2+j]==9)
        z=true;
    return z;

}
bool king1Possiblemoves()
{
    int i,j;
    bool possible=true;
    if (ifcondblackkill(0,0))
    {
        for(i=-1; i<2; i++)
        {
            for(j=-1; j<2; j++)
            {
                if(i==0&&j==0)
                    continue;

                if(blackMoves[ki[0]+2+i][ki[1]+2+j]!=6||blackMoves[ki[0]+2+i][ki[1]+2+j]!=7
                        ||blackMoves[ki[0]+2+i][ki[1]+2+j]!=8||blackMoves[ki[0]+2+i][ki[1]+2+j]!=5
                        ||blackMoves[ki[0]+2+i][ki[1]+2+j]!=9||blackMoves[ki[0]+2+i][ki[1]+2+j]!=1
                        ||blackMoves[ki[0]+2+i][ki[1]+2+j]!=2||blackMoves[ki[0]+2+i][ki[1]+2+j]!=4)
                    possible=false;
            }
        }
    }
    if(!possible)
    {
        for(i=2; i<10; i++)
        {
            for(j=2; j<10; j++)
            {
                cb[i][j]=board[i-2][j-2];
            }
        }
        if(blackMoves[ki[0]+2][ki[1]+2]==5)
        {
            if(cb[ki[0]+2-1][ki[1]+2+1]=='P')
            {
                if(ifcondwhite(-1,1))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2-1][ki[1]+2-1]=='P')
            {
                if(ifcondwhite(-1,-1))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
        }
        else if(blackMoves[ki[0]+2][ki[1]+2]==6)
        {
            if(cb[ki[0]+2-1][ki[1]+2+2]=='H')
            {
                if(ifcondwhite(-1,2))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2-2][ki[1]+2+1]=='H')
            {
                if(ifcondwhite(-2,1))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2-1][ki[1]+2-2]=='H')
            {
                if(ifcondwhite(-1,-2))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2-2][ki[1]+2-1]=='H')
            {
                if(ifcondwhite(-2,-1))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2+1][ki[1]+2-2]=='H')
            {
                if(ifcondwhite(1,-2))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2+1][ki[1]+2+2]=='H')
            {
                if(ifcondwhite(1,2))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2+2][ki[1]+2-1]=='H')
            {
                if(ifcondwhite(2,-1))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
            else if(cb[ki[0]+2+2][ki[1]+2+1]=='H')
            {
                if(ifcondwhite(2,1))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player2Win=true;
                }
            }
        }
        else if(blackMoves[ki[0]+2][ki[1]+2]==7)
        {
            j=ki[1]-1;
            for(i=ki[0]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i--;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=-1)&&board[i][j]=='B')
            {
                for(; i<ki[0]; i++)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]+1;
            for(i=ki[0]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i--;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=-1)&&board[i][j]=='B')
            {
                for(; i<ki[0]; i++)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]+1;
            for(i=ki[0]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i++;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=8)&&board[i][j]=='B')
            {
                for(; i>ki[0]; i--)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]-1;
            for(i=ki[0]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i++;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=8)&&board[i][j]=='B')
            {
                for(; i>ki[0]; i--)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
        }
        else if(blackMoves[ki[0]+2][ki[1]+2]==8)
        {
            for(i=ki[0]+1; i<8; i++)
            {
                if(board[i][ki[1]]!='.'||board[i][ki[1]]!='-')
                    break;
            }
            if(i<8&&board[i][ki[1]]=='R')
            {
                for(; i>ki[0]; i--)
                {
                    if(ifcondwhite(i-ki[0],0))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            for(i=ki[0]-1; i>=0; i--)
            {

                if(board[i][ki[1]]!='.'||board[i][ki[1]]!='-')
                    break;
            }
            if(i>=0&&board[i][ki[1]]=='R')
            {
                for(; i<ki[0]; i++)
                {
                    if(ifcondwhite(i-ki[0],0))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            for(i=ki[1]+1; i<8; i++)
            {
                if(board[ki[0]][i]!='.'||board[ki[0]][i]!='-')
                    break;
            }
            if(i<8&&board[ki[0]][i]=='R')
            {
                for(; i>ki[1]; i--)
                {
                    if(ifcondwhite(0,i-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }

            for(i=ki[1]-1; i>=0; i--)
            {

                if(board[ki[0]][i]!='.'||board[ki[0]][i]!='-')
                    break;
            }
            if(i>=0&&board[ki[0]][i]=='R')
            {
                for(; i<ki[1]; i++)
                {
                    if(ifcondwhite(0,i-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }
        }
        else if(blackMoves[ki[0]+2][ki[1]+2]==9)
        {
            for(i=ki[0]+1; i<8; i++)
            {
                if(board[i][ki[1]]!='.'||board[i][ki[1]]!='-')
                    break;
            }
            if(i<8&&board[i][ki[1]]=='Q')
            {
                for(; i>ki[0]; i--)
                {
                    if(ifcondwhite(i-ki[0],0))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            for(i=ki[0]-1; i>=0; i--)
            {

                if(board[i][ki[1]]!='.'||board[i][ki[1]]!='-')
                    break;
            }
            if(i>=0&&board[i][ki[1]]=='Q')
            {
                for(; i<ki[0]; i++)
                {
                    if(ifcondwhite(i-ki[0],0))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            for(i=ki[1]+1; i<8; i++)
            {
                if(board[ki[0]][i]!='.'||board[ki[0]][i]!='-')
                    break;
            }
            if(i<8&&board[ki[0]][i]=='Q')
            {
                for(; i>ki[1]; i--)
                {
                    if(ifcondwhite(0,i-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }

            for(i=ki[1]-1; i>=0; i--)
            {

                if(board[ki[0]][i]!='.'||board[ki[0]][i]!='-')
                    break;
            }
            if(i>=0&&board[ki[0]][i]=='Q')
            {
                for(; i<ki[1]; i++)
                {
                    if(ifcondwhite(0,i-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]-1;
            for(i=ki[0]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i--;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=-1)&&board[i][j]=='Q')
            {
                for(; i<ki[0]; i++)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]+1;
            for(i=ki[0]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i--;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=-1)&&board[i][j]=='Q')
            {
                for(; i<ki[0]; i++)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]+1;
            for(i=ki[0]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i++;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=8)&&board[i][j]=='Q')
            {
                for(; i>ki[0]; i--)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player2Win=true;
                return;
            }
            j=ki[1]-1;
            for(i=ki[0]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i++;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=8)&&board[i][j]=='Q')
            {
                for(; i>ki[0]; i--)
                {
                    if(ifcondwhite(i-ki[0],j-ki[1]))
                    {
                        printf("WhiteCheck\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player2Win=true;
                return;
            }

        }
    }
}

bool king2Possiblemoves()
{
    int i,j;
    bool possible=true;
    if (ifcondwhite(0,0))  //checks if king is in danger
    {
        for(i=-1; i<2; i++)
        {
            for(j=-1; j<2; j++)
            {
                if(i==0&&j==0)
                    continue;

                if(whiteMoves[ki[2]+2+i][ki[3]+2+j]!=6||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=7    //if king cannot move to any adjacent square
                        ||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=8||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=5
                        ||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=9||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=1
                        ||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=2||whiteMoves[ki[2]+2+i][ki[3]+2+j]!=4)
                    possible=false;
            }
        }
    }
    if(!possible)
    {
        for(i=2; i<10; i++)
        {
            for(j=2; j<10; j++)
            {
                cb[i][j]=board[i-2][j-2];   //reads board in a 12 by 12 array to facilitate code for horse and king
            }
        }
        if(whiteMoves[ki[2]+2][ki[3]+2]==5)
        {
            if(cb[ki[2]+2-1][ki[3]+2+1]=='p')       //if king is threatened by pawn checks if it can be eaten
            {
                if(ifcondblack(-1,1))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2-1][ki[3]+2-1]=='p')
            {
                if(ifcondblack(-1,-1))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
        }
        else if(whiteMoves[ki[2]+2][ki[3]+2]==6)
        {
            if(cb[ki[2]+2-1][ki[3]+2+2]=='h')
            {
                if(ifcondblack(-1,2))
                   {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2-2][ki[3]+2+1]=='h')
            {
                if(ifcondblack(-2,1))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2-1][ki[3]+2-2]=='h')
            {
                if(ifcondblack(-1,-2))
                   {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2-2][ki[3]+2-1]=='h')
            {
                if(ifcondblack(-2,-1))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2+1][ki[3]+2-2]=='h')
            {
                if(ifcondblack(1,-2))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2+1][ki[3]+2+2]=='h')
            {
                if(ifcondblack(1,2))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2+2][
                        ki[3]+2-1]=='h')
            {
                if(ifcondblack(2,-1))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
            else if(cb[ki[2]+2+2][
                        ki[3]+2+1]=='h')
            {
                if(ifcondblack(2,1))
                    {
                        printf("Black Check\n");
                        return;
                    }
                else
                {
                    gameOver=true;
                    player1Win=true;
                }
            }
        }
        else if(whiteMoves[ki[2]+2][
                    ki[3]+2]==7)
        {
            j=
                ki[3]-1;
            for(i=ki[2]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i--;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=-1)&&board[i][j]=='b')
            {
                for(; i<ki[2]; i++)
                {
                    if(ifcondblack(i-ki[2],j-
                                   ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=
                ki[3]+1;
            for(i=ki[2]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i--;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=-1)&&board[i][j]=='b')
            {
                for(; i<ki[2]; i++)
                {
                    if(ifcondblack(i-ki[2],j-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=
                ki[3]+1;
            for(i=ki[2]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i++;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=8)&&board[i][j]=='b')
            {
                for(; i>ki[2]; i--)
                {
                    if(ifcondblack(i-ki[2],j-

                                   ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=
                ki[3]-1;
            for(i=ki[2]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i++;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=8)&&board[i][j]=='b')
            {
                for(; i>ki[2]; i--)
                {
                    if(ifcondblack(i-ki[2],j-


                                   ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
        }
        else if(whiteMoves[ki[2]+2][
                    ki[3]+2]==8)
        {
            for(i=ki[2]+1; i<8; i++)
            {
                if(board[i][
                            ki[3]]!='.'||board[i][ki[3]]!='-')
                    break;
            }
            if(i<8&&board[i][ki[3]]=='r')
            {
                for(; i>ki[2]; i--)
                {
                    if(ifcondblack(i-ki[2],0))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            for(i=ki[2]-1; i>=0; i--)
            {

                if(board[i][
                            ki[3]]!='.'||board[i][

                            ki[3]]!='-')
                    break;
            }
            if(i>=0&&board[i][
                        ki[3]]=='r')
            {
                for(; i<ki[2]; i++)
                {
                    if(ifcondblack(i-ki[2],0))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            for(i=
                        ki[3]+1; i<8; i++)
            {
                if(board[ki[2]][i]!='.'||board[ki[2]][i]!='-')
                    break;
            }
            if(i<8&&board[ki[2]][i]=='r')
            {
                for(; i>
                        ki[3]; i--)
                {
                    if(ifcondblack(0,i-
                                   ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }

            for(i=ki[3]-1; i>=0; i--)
            {

                if(board[ki[2]][i]!='.'||board[ki[2]][i]!='-')
                    break;
            }
            if(i>=0&&board[ki[2]][i]=='r')
            {
                for(; i<ki[3]; i++)
                {
                    if(ifcondblack(0,i-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }
        }
        else if(whiteMoves[ki[2]+2][ki[3]+2]==9)
        {
            for(i=ki[2]+1; i<8; i++)
            {
                if(board[i][ki[3]]!='.'||board[i][ki[3]]!='-')
                    break;
            }
            if(i<8&&board[i][ki[3]]=='q')
            {
                for(; i>ki[2]; i--)
                {
                    if(ifcondblack(i-ki[2],0))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            for(i=ki[2]-1; i>=0; i--)
            {

                if(board[i][ki[3]]!='.'||board[i][ki[3]]!='-')
                    break;
            }
            if(i>=0&&board[i][ki[3]]=='q')
            {
                for(; i<ki[2]; i++)
                {
                    if(ifcondblack(i-ki[2],0))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            for(i=ki[3]+1; i<8; i++)
            {
                if(board[ki[2]][i]!='.'||board[ki[2]][i]!='-')
                    break;
            }
            if(i<8&&board[ki[2]][i]=='q')
            {
                for(; i>ki[3]; i--)
                {
                    if(ifcondblack(0,i-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }

            for(i=ki[3]-1; i>=0; i--)
            {

                if(board[ki[2]][i]!='.'||board[ki[2]][i]!='-')
                    break;
            }
            if(i>=0&&board[ki[2]][i]=='q')
            {
                for(; i<ki[3]; i++)
                {
                    if(ifcondblack(0,i-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=ki[3]-1;
            for(i=ki[2]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i--;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=-1)&&board[i][j]=='q')
            {
                for(; i<ki[2]; i++)
                {
                    if(ifcondblack(i-ki[2],j-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=ki[3]+1;
            for(i=ki[2]-1; i>0; i--)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i--;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=-1)&&board[i][j]=='q')
            {
                for(; i<ki[2]; i++)
                {
                    if(ifcondblack(i-ki[2],j-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=ki[3]+1;
            for(i=ki[2]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==7)
                {
                    j++;
                    i++;
                    break;
                }
                j++;

            }
            if((j!=8&&i!=8)&&board[i][j]=='q')
            {
                for(; i>ki[2]; i--)
                {
                    if(ifcondblack(i-ki[2],j-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j--;
                }
                gameOver=true;
                player1Win=true;
                return;
            }
            j=ki[3]-1;
            for(i=ki[2]+1; i<8; i++)
            {
                if(board[i][j]!='.'||board[i][j]!='-')
                    break;
                if(j==0)
                {
                    j--;
                    i++;
                    break;
                }
                j--;

            }
            if((j!=-1&&i!=8)&&board[i][j]=='q')
            {
                for(; i>ki[2]; i--)
                {
                    if(ifcondblack(i-ki[2],j-ki[3]))
                    {
                        printf("Black Check\n");
                        return;
                    }
                    j++;
                }
                gameOver=true;
                player1Win=true;
                return;
            }

        }
    }
}
void player1Move()
{

    int n1,n2;
    int i,j;
    for(i=0; i<8; i++)
    {
            for(j=0; j<8; j++)
            {
                    temparr[i][j]=board[i][j];
            }
    }
    char c1,c2,c3;
    whiteDanger();
    blackDanger();
    king1Possiblemoves();
    if(gameOver==true)
    {
        return;
    }
    if(ifcondblackkill(0,0))
    {while(ifcondblackkill(0,0))
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
    return;
    }
// Positions

    printboard();
    /*printf("\n\n");
    printf("PLAYER 1");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);

    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);*/
    printf("Player1");
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

    pawnP1(y1,y2,x1,x2);
    rook1(y1,y2,x1,x2);
    bishop1(y1,y2,x1,x2);
    horse1(y1,y2,x1,x2);
    king(y1,y2,x1,x2);
    system("cls");




}
void player1Movetemp()
{
    printboard();
    int n1,n2;
    char c1,c2,c3;
   /* printf("\n\n");
    printf("PLAYER 1");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);
    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);*/
    printf("Player1");
    scanf(" %c%d%c%d",&c1,&n1,&c2,&n2);
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
    king(y1,y2,x1,x2);
    system("cls");
}
/* ****************************************************** PLAYER 2 RELATED *************************************************** */
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
    int i,j;
    for(i=0; i<8; i++)
    {
            for(j=0; j<8; j++)
            {
                    temparr[i][j]=board[i][j];
            }
    }
    int n1,n2;
    char c1,c2;
    blackDanger();
    whiteDanger();
    king2Possiblemoves();
    if(gameOver==true)
    {
        return;
    }
    if(ifcondwhitekill(0,0))
    {
    while(ifcondwhitekill(0,0))
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
    return;
    }
    printboard();
// Positions
   /* printf("PLAYER 2:");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);

    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);*/
    printf("Player2");
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

    /*        PAWN PAWN PAWN PAWN        */

// Checking If It Is Pawn And If True It Must Be At Row 7 That Means x1=6 Else Give Error

    pawnP2(y1,y2,x1,x2);
    rook2(y1,y2,x1,x2);
    bishop2(y1,y2,x1,x2);
    horse2(y1,y2,x1,x2);
    king(y1,y2,x1,x2);
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
    /*printf("PLAYER 2:");
    printf("\n");
    printf("Enter x1 (Current Piece Row) : ");
    scanf("%d",&n1);
    printf("Enter y1 (Current Piece Column) : ");
    scanf(" %c",&c1);
    printf("Enter x2 ( Row To Move ) : ");
    scanf("%d",&n2);
    printf("Enter y2 ( Column To Move ) : ");
    scanf(" %c",&c2);*/
    printf("Player2");
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

    /*        PAWN PAWN PAWN PAWN        */

// Checking If It Is Pawn And If True It Must Be At Row 7 That Means x1=6 Else Give Error

    pawnP2(y1,y2,x1,x2);                   //#3rft el deadindex ka static 3shan keda sheloh
    rook2(y1,y2,x1,x2);
    bishop2(y1,y2,x1,x2);
    horse2(y1,y2,x1,x2);
    king(y1,y2,x1,x2);
    system("cls");

}
void king(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='k')
    {
        if((abs(x2-x1)==1&&abs(y2-y1)==1)||(abs(x2-x1)==0&&abs(y2-y1)==1)||(abs(x2-x1)==1&&abs(y2-y1)==0))
        {
            if(blackMoves[x2+2][y2+2]!=2&&blackMoves[x2+2][y2+2]!=4
                    &&blackMoves[x2+2][y2+2]!=6&&blackMoves[x2+2][y2+2]!=5
                    &&blackMoves[x2+2][y2+2]!=7&&blackMoves[x2+2][y2+2]!=8
                    &&blackMoves[x2+2][y2+2]!=9&&whiteMoves[x2+2][y2+2]!=1)    //checks if black would threaten white king in that space
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
                return ;
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
            if(whiteMoves[x2+2][y2+2]!=2&&whiteMoves[x2+2][y2+2]!=4
                    &&whiteMoves[x2+2][y2+2]!=6&&whiteMoves[x2+2][y2+2]!=5
                    &&whiteMoves[x2+2][y2+2]!=7&&whiteMoves[x2+2][y2+2]!=8
                    &&whiteMoves[x2+2][y2+2]!=9&&whiteMoves[x2+2][y2+2]!=1)    //checks if white would threaten black king in that space
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
void whiteDanger()
{
    int i,j,r,c;
    for(i=0; i<12; i++)
    {
        for(j=0; j<12; j++)
        {
            blackMoves[i][j]=resetMoves[i][j];
        }
    }
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
                blackMoves[i][j]=3;
            if(board[i-2][j-2]=='K')
                blackMoves[i][j]=4;
            if(board[i-2][j-2]=='P')
                blackMoves[i][j]=10;
            if(board[i-2][j-2]=='B')
                blackMoves[i][j]=12;
            if(board[i-2][j-2]=='H')
                blackMoves[i][j]=11;
            if(board[i-2][j-2]=='R')
                blackMoves[i][j]=13;
            if(board[i-2][j-2]=='Q')
                blackMoves[i][j]=14;
        }
    }
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)
        {
            if(board[i-2][j-2]=='K')
            {
                blackMoves[i+1][j-1]=4;
                blackMoves[i-1][j-1]=4;                 //fills spaces around king with 1
                blackMoves[i][j-1]=4;
                blackMoves[i+1][j+1]=4;
                blackMoves[i-1][j+1]=4;
                blackMoves[i][j+1]=4;
                blackMoves[i][j+1]=4;
                blackMoves[i-1][j]=4;
                blackMoves[i+1][j]=4;
            }
            if(board[i-2][j-2]=='P')
            {
                //fills possible eating moves for pawn with 1
                blackMoves[i+1][j-1]=5;
                blackMoves[i+1][j+1]=5;
            }
            if(board[i-2][j-2]=='H')
            {
                //fills possible eating moves for horse with 1
                blackMoves[i+2][j+1]=6;
                blackMoves[i+2][j-1]=6;
                blackMoves[i+1][j-2]=6;
                blackMoves[i+1][j+2]=6;
                blackMoves[i-1][j+2]=6;
                blackMoves[i-2][j+1]=6;
                blackMoves[i-2][j-1]=6;
                blackMoves[i-1][j-2]=6;
            }
            if(board[i-2][j-2]=='R')
            {
                //fills possible eating moves for rook with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king
                for(r=i-1; r>1; r--)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=8;
                        break;
                    }
                    blackMoves[r][j]=8;
                }
                for(r=i+1; r<10; r++)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=8;
                        break;
                    }
                    blackMoves[r][j]=8;
                }
                for(c=j-1; c>1; c--)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=8;
                        break;
                    }
                    blackMoves[i][c]=8;
                }
                for(c=j+1; c<10; c++)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=8;
                        break;
                    }
                    blackMoves[i][c]=8;
                }
            }
            if(board[i-2][j-2]=='B')
            {
                //fills possible eating moves for bishop with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                c=j-1;
                for(r=i-1; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=7;
                        break;
                    }
                    blackMoves[r][c]=7;
                    c--;
                }
                c=j+1;
                for(r=i+1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=7;
                        break;
                    }
                    blackMoves[r][c]=7;
                    c++;
                }
                c=j+1;
                for(r=i-1; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=7;
                        break;
                    }
                    blackMoves[r][c]=7;
                    c++;
                }
                c=j-1;
                for(r=i+1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=7;
                        break;
                    }
                    blackMoves[r][c]=7;
                    c--;
                }
            }
            if(board[i-2][j-2]=='Q')
            {
                //just copied the code for both rook and bishop
                for(r=i-1; r>1; r--)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=9;
                        break;
                    }
                    blackMoves[r][j]=9;
                }
                for(r=i+1; r<10; r++)
                {
                    if(blackMoves[r][j]==2)
                        break;
                    if(blackMoves[r][j]==3)
                    {
                        blackMoves[r][j]=9;
                        break;
                    }
                    blackMoves[r][j]=9;
                }
                for(c=j-1; c>1; c--)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=9;
                        break;
                    }
                    blackMoves[i][c]=9;
                }
                for(c=j+1; c<10; c++)
                {
                    if(blackMoves[i][c]==2)
                        break;
                    if(blackMoves[i][c]==3)
                    {
                        blackMoves[i][c]=9;
                        break;
                    }
                    blackMoves[i][c]=9;
                }
                c=j-1;
                for(r=i-1; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=9;
                        break;
                    }
                    blackMoves[r][c]=9;
                    c--;
                }
                c=j+1;
                for(r=i+1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=9;
                        break;
                    }
                    blackMoves[r][c]=9;
                    c++;
                }
                c=j+1;
                for(r=i-1; r>1; r--)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=9;
                        break;
                    }
                    blackMoves[r][c]=9;
                    c++;
                }
                c=j-1;
                for(r=i+1; r<10; r++)
                {
                    if(blackMoves[r][c]==2)
                        break;
                    if(blackMoves[r][c]==3)
                    {
                        blackMoves[r][c]=9;
                        break;
                    }
                    blackMoves[r][c]=9;
                    c--;
                }
            }
        }
    }
}
void blackDanger()
{
    int i,j,r,c;
    for(i=0; i<12; i++)
    {
        for(j=0; j<12; j++)
        {
            whiteMoves[i][j]=resetMoves[i][j];
        }
    }
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)                 //fills array with positions of all pieces the friendly will be 2 enemy pieces will be 3 current king is 4
        {
            if(board[i-2][j-2]=='R')
                whiteMoves[i][j]=2;
            if(board[i-2][j-2]=='Q')
                whiteMoves[i][j]=2;
            if(board[i-2][j-2]=='B')
                whiteMoves[i][j]=2;
            if(board[i-2][j-2]=='P')
                whiteMoves[i][j]=2;
            if(board[i-2][j-2]=='H')
                whiteMoves[i][j]=2;
            if(board[i-2][j-2]=='K')
                whiteMoves[i][j]=3;
            if(board[i-2][j-2]=='k')
                whiteMoves[i][j]=4;
            if(board[i-2][j-2]=='p')
                whiteMoves[i][j]=10;
            if(board[i-2][j-2]=='b')
                whiteMoves[i][j]=12;
            if(board[i-2][j-2]=='h')
                whiteMoves[i][j]=11;
            if(board[i-2][j-2]=='r')
                whiteMoves[i][j]=13;
            if(board[i-2][j-2]=='q')
                whiteMoves[i][j]=14;
        }
    }
    for(i=2; i<10; i++)
    {
        for(j=2; j<10; j++)
        {
            if(board[i-2][j-2]=='k')
            {

                whiteMoves[i+1][j-1]=4;
                whiteMoves[i-1][j-1]=4;                 //fills spaces around king with 1
                whiteMoves[i][j-1]=4;
                whiteMoves[i+1][j+1]=4;

                whiteMoves[i-1][j+1]=4;


                whiteMoves[i][j+1]=4;


                whiteMoves[i][j+1]=4;

                whiteMoves[i-1][j]=4;

                whiteMoves[i+1][j]=4;
            }
            if(board[i-2][j-2]=='p')
            {
                //fills possible eating moves for pawn with 1


                whiteMoves[i+1][j-1]=5;


                whiteMoves[i+1][j+1]=5;
            }
            if(board[i-2][j-2]=='h')
            {
                //fills possible eating moves for horse with 1

                whiteMoves[i+2][j+1]=6;

                whiteMoves[i+2][j-1]=6;


                whiteMoves[i+1][j-2]=6;

                whiteMoves[i+1][j+2]=6;
                whiteMoves[i-1][j+2]=6;
                whiteMoves[i-1][j+1]=6;
                whiteMoves[i-1][j-1]=6;

                whiteMoves[i-1][j-2]=6;
            }

            if(board[i-2][j-2]=='r')
            {
                //fills possible eating moves for rook with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                for(r=i-1; r>1; r--)
                {
                    if(


                        whiteMoves[r][j]==2)
                        break;
                    if(
                        whiteMoves[r][j]==3)
                    {


                        whiteMoves[r][j]=8;
                        break;
                    }

                    whiteMoves[r][j]=8;
                }
                for(r=i+1; r<10; r++)
                {
                    if(
                        whiteMoves[r][j]==2)
                        break;
                    if(

                        whiteMoves[r][j]==3)
                    {

                        whiteMoves[r][j]=8;
                        break;
                    }

                    whiteMoves[r][j]=8;
                }
                for(c=j-1; c>1; c--)
                {
                    if(

                        whiteMoves[i][c]==2)
                        break;
                    if(whiteMoves[i][c]==3)
                    {
                        whiteMoves[i][c]=8;
                        break;
                    }

                    whiteMoves[i][c]=8;
                }
                for(c=j+1; c<10; c++)
                {
                    if(

                        whiteMoves[i][c]==2)
                        break;
                    if(
                        whiteMoves[i][c]==3)
                    {

                        whiteMoves[i][c]=8;
                        break;
                    }

                    whiteMoves[i][c]=8;
                }
            }
            if(board[i-2][j-2]=='b')
            {
                //fills possible eating moves for bishop with 1 if it finds an enemy piece it stops as it wont pose danger afterwards if it finds friendly piece it makes it 1 so it cant be eaten by king

                c=j-1;
                for(r=i-1; r>1; r--)
                {
                    if(

                        whiteMoves[r][c]==2)
                        break;
                    if(
                        whiteMoves[r][c]==3)
                    {
                        whiteMoves[r][c]=7;
                        break;
                    }
                    whiteMoves[r][c]=7;
                    c--;
                }
                c=j+1;
                for(r=i+1; r<10; r++)
                {
                    if(
                        whiteMoves[r][c]==2)
                        break;
                    if(


                        whiteMoves[r][c]==3)
                    {

                        whiteMoves[r][c]=7;
                        break;
                    }

                    whiteMoves[r][c]=7;
                    c++;
                }
                c=j+1;
                for(r=i+1; r>1; r--)
                {
                    if(whiteMoves[r][c]==2)
                        break;
                    if(whiteMoves[r][c]==3)
                    {

                        whiteMoves[r][c]=7;
                        break;
                    }

                    whiteMoves[r][c]=7;
                    c++;
                }
                c=j-1;
                for(r=i+1; r<10; r++)
                {
                    if(
                        whiteMoves[r][c]==2)
                        break;
                    if(
                        whiteMoves[r][c]==3)
                    {
                        whiteMoves[r][c]=7;
                        break;
                    }
                    whiteMoves[r][c]=7;
                    c--;
                }
            }
            if(board[i-2][j-2]=='q')
            {
                //just copied the code for both rook and bishop
                for(r=i-1; r>1; r--)
                {
                    if(
                        whiteMoves[r][j]==2)
                        break;
                    if(

                        whiteMoves[r][j]==3)
                    {
                        whiteMoves[r][j]=9;
                        break;
                    }
                    whiteMoves[r][j]=9;
                }
                for(r=i+1; r<10; r++)
                {
                    if(whiteMoves[r][j]==2)
                        break;
                    if(whiteMoves[r][j]==3)
                    {
                        whiteMoves[r][j]=9;
                        break;
                    }
                    whiteMoves[r][j]=9;
                }
                for(c=j-1; c>1; c--)
                {
                    if(whiteMoves[i][c]==2)
                        break;
                    if(whiteMoves[i][c]==3)
                    {
                        whiteMoves[i][c]=9;
                        break;
                    }
                    whiteMoves[i][c]=9;
                }
                for(c=j+1; c<10; c++)
                {
                    if(whiteMoves[i][c]==2)
                        break;
                    if(whiteMoves[i][c]==3)
                    {
                        whiteMoves[i][c]=9;
                        break;
                    }
                    whiteMoves[i][c]=9;
                }
                c=j-1;
                for(r=i-1; r>1; r--)
                {
                    if(whiteMoves[r][c]==2)
                        break;
                    if(whiteMoves[r][c]==3)
                    {
                        whiteMoves[r][c]=9;
                        break;
                    }
                    whiteMoves[r][c]=9;
                    c--;
                }
                c=j+1;
                for(r=i+1; r<10; r++)
                {
                    if(whiteMoves[r][c]==2)
                        break;
                    if(whiteMoves[r][c]==3)
                    {
                        whiteMoves[r][c]=9;
                        break;
                    }
                    whiteMoves[r][c]=9;
                    c++;
                }
                c=j+1;
                for(r=i-1; r>1; r--)
                {
                    if(whiteMoves[r][c]==2)
                        break;
                    if(whiteMoves[r][c]==3)
                    {
                        whiteMoves[r][c]=9;
                        break;
                    }
                    whiteMoves[r][c]=9;
                    c++;
                }
                c=j-1;
                for(r=i+1; r<10; r++)
                {
                    if(whiteMoves[r][c]==2)
                        break;
                    if(whiteMoves[r][c]==3)
                    {
                        whiteMoves[r][c]=9;
                        break;
                    }
                    whiteMoves[r][c]=9;
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
    {   printf("Checkmate\n");
        printf("Player 1 Wins");
    }
    else if (player2Win==true)
    {
        printf("Checkmate\n");
        printf("Player 2 Wins");
    }
    else
    {
        printf("Stalemate");
    }

    return 0;
}
