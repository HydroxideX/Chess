#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
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
void pawnP1(int y1,int y2,int x1,int x2,int deadindex)   //#3rft el deadindex ka static 3shan keda sheloh
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
    else if(board[x2][y2] == 'K')                  //#El king mbytshalsh
        deadPieces[deadindex++] = 'K';
    if ( x1 == 6 )
    {
        if(y1 == y2 && x2 >=4)
        {
            board[x2][y2] = 'p';
            board[x1][y1] = boardlayout[x1][y1];
            printboard(8,8,board,deadPieces);
        }
        else
            printf("INVALID MOVE");
    }
    else if(x1<6)
    {
        if(y1 == y2 && x1==x2+1)
        {
            board[x2][y2] = 'p';
            board[x1][y1] = boardlayout[x1][y1];
            printboard(8,8,board,deadPieces);
        }
        else
            printf("INVALID MOVE");
    }

    /*   if ( x1 == 6 )
       {
          /* if(board[x2][y2] = 'p' && y1 == y2 && x2 <=4 && x1)
           {
               deadPieces[n++]= 'p';
               board[x2][y2] = 'p';
               board[x1][y1] = boardlayout[x1][y1];
               printboard(8,8,board,deadPieces);
           }

            if(y1 == y2 && x2 <=4)
           {
               if(board[x2++][y2++] = 'p')
               {
                   deadPieces[n++]= 'p';
               }
               board[x2][y2] = 'p';
               board[x1][y1] = boardlayout[x1][y1];
               printboard(8,8,board,deadPieces);
           }
           else
               printf("Error");
       }
       */

    printboard(8,8,board,deadPieces);

}
void CheckDead(int y1,int y2,int x1,int x2)
{
    int n=0;
    if(board[x2][y2] == board[x2][y2] == 'p')
    {
        deadPieces[n++] ='p';
    }
}
void player1Move()
{
    printboard();
    int n1,n2;
    char c1,c2,c3;
// Positions
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
    /*printf("Player 1:");
    scanf("%c%d%c%d",&c1,&n1,&c2,&n2);*/
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

    pawnP1(y1,y2,x1,x2,0);                   //#3rft el deadindex ka static 3shan keda sheloh
    rook1(y1,y2,x1,x2);
    bishop1(y1,y2,x1,x2);
    horse1(y1,y2,x1,x2);
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
/* ****************************************************** PLAYER 2 RELATED *************************************************** */
void pawnP2(int y1,int y2,int x1,int x2,int deadindex)
{
    //Dead Conditions
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
    else if(board[x2][y2] == 'k')
        deadPieces[deadindex++] = 'k';  // el king mbytshalsh
    if ( x1 == 1 )
    {
        if(y1 == y2 && x2 <=3)
        {
            board[x2][y2] = 'P';
            board[x1][y1] = boardlayout[x1][y1];
            printboard(8,8,board,deadPieces);
        }
        else
            printf("Error");
    }
    else if(x1>1)
    {
        if(y1 == y2 && x2==x1+1)
        {
            board[x2][y2] = 'P';
            board[x1][y1] = boardlayout[x1][y1];
            printboard(8,8,board,deadPieces);
        }
        else
            printf("Error");
    }


}
void player2Move()
{
    printboard();
    int n1,n2;
    char c1,c2;
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

    pawnP1(y1,y2,x1,x2,0);                   //#3rft el deadindex ka static 3shan keda sheloh
    rook1(y1,y2,x1,x2);
    bishop1(y1,y2,x1,x2);
    horse1(y1,y2,x1,x2);
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
    if(board[x1][y1]=='b'||board[x1][y1]=='q')
    {
        if((x2-x1)==(y2-y1))            //if diff between rows=diff between columns then it is correct move else it restarts
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
void bishop2(int y1,int y2,int x1,int x2)
{
    if(board[x1][y1]=='B'||board[x1][y1]=='Q')   //if diff between rows=diff between columns then it is correct move else it restarts
    {
        if((x2-x1)==(y2-y1))
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
                        player1Move();
                        return ;
                    }
                    i++;
                    j++;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    player1Move();
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
                        player1Move();
                        return ;
                    }
                    i++;
                    j--;
                }
                if(board[x2][y2]=='K'||board[x2][y2]=='Q'||board[x2][y2]=='P'||board[x2][y2]=='B'||board[x2][y2]=='R')
                {
                    player1Move();
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
                if(board[x1][y1]=='k'||board[x1][y1]=='q'||board[x1][y1]=='p'||board[x1][y1]=='b'||board[x1][y1]=='r')
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
                if(board[x1][y1]=='K'||board[x1][y1]=='Q'||board[x1][y1]=='P'||board[x1][y1]=='B'||board[x1][y1]=='R')
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
                if(board[x1][y1]=='k'||board[x1][y1]=='q'||board[x1][y1]=='p'||board[x1][y1]=='b'||board[x1][y1]=='r')
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
                        player1Move();
                        return ;
                    }
                    i--;
                    j--;
                }
                if(board[x1][y1]=='K'||board[x1][y1]=='Q'||board[x1][y1]=='P'||board[x1][y1]=='B'||board[x1][y1]=='R')
                {
                    player1Move();
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
void rook1(int y1,int y2,int x1,int x2)
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
    // Variables
    int i;
    bool gameOver=false,
         player1Win=false,
         player2Win=false,
         stalemate=false;
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
