# task3
#include <iostream >
#include <conio.h>
#include <mmsystem.h> 
#pragma comment(lib, "winmm.lib") 
using namespace std ;

bool gameover;

const int height = 30 ;
const int width = 20 ;

int x , y , fruitX , fruitY , score ;

int tailX[100] , tailY[100] ;
int nTail ;

enum eDirection {STOP = 0 , LEFT , RIGHT , UP , DOWN} ;
eDirection dir ;

void setup(){

    gameover = false ;
    dir = STOP ;
    x = width / 2 ;
    y = height / 2 ;
    fruitX = rand() % width ;
    fruitY = rand() % height ;
    score = 0 ;

}
void draw(){
    system("cls");

    for(int i = 0 ; i < width + 1 ; i++){
        cout << "|" ;
    }
    cout << endl ;

    for(int i = 0 ; i < height ; i++){
        for (int j = 0 ; j < width ; j++){
            if( j == 0){
                cout << "|" ;
            }
            if( i == y && j == x ){
                cout << "O" ;
            }
            else if (i == fruitY && j == fruitX){
                cout<< " F " ;
            }
            else{
                bool print = false ;
                for(int k = 0 ; k < nTail ; k++){
                   
                    if(tailX[k] == j && tailY[k] == i){
                        cout << "o" ;
                        print = true ;
                    }
                }
                if(!print){
                    cout << " " ;
                }
            }
            if(j == width - 1){
                cout << "|";
            }
        }
        cout << endl ;
    }
    for(int i = 0 ; i < height + 1 ; i++){
        cout << "|" ;
    }
    cout << endl ;

    cout << "SCORE : " << score << endl ;
}

void input(){
    if(_kbhit()){
        switch(_getch()){
            case 'a':
                dir = LEFT ;
                break;
            case 'd':
                dir = RIGHT ;
                break;
            case 'w' :
                dir = UP;
                break;
            case 's' :
                dir = DOWN ;
                break;
            case 'q' :
                gameover = true ;
                break;
        }
    }
}
void logic(){
    int preX = tailX[0] ;
    int preY = tailY[0] ;
    int prev2X , prev2Y ;

    for (int i = 1 ; i < nTail ; i++){
        prev2X = tailX[i] ;
        prev2Y = tailY[i] ;

        tailX[i] = preX ;
        tailY[i] = preY ;

        preX = prev2X ;
        preY = prev2Y ;

        tailX[0] = x ;
        tailY[0] = y ;
    }
    switch (dir)
    {
    case LEFT:
        x-- ;
        break;
    case RIGHT :
        x++ ;
        break;
    case UP :
        y-- ;
        break;
    case DOWN :
        y++ ;
        break;
    default:
        break;
    }

    if( x>= width) x = 0 ;
        else if (x < 0) x = width -1 ;
    if( y>= height) y = 0 ;
        else if (y < 0) y = height -1 ;  
    for(int i =0 ; i < nTail ; i++){
        if(tailX[i] == x && tailY[i] == y){
            gameover = true ;
        }
    }
    if(x == fruitX && y == fruitY){

        score += 10 ;
        fruitX = rand() % width ;
        fruitY = rand() % height ;
        nTail ++ ;

        PlaySound(TEXT("eat.wav"), NULL, SND_ASYNC);
    }
    do {
        FruitX = rand() % Width;
        FruitY = rand() % Height;
    } while (tailX[0] == FruitX && tailY[0] == FruitY);
    
}
int main(){
    setup();
    while(!gameover){
        draw();
        input();
        logic();
    }
    if (gameOver) {
        system("cls"); // Clear screen
        cout << "\n\n\n";
        cout << "\t====================\n";
        cout << "\t     GAME OVER     \n";
        cout << "\t====================\n";
        cout << "\n\tYour Score: " << Score << endl;
        cout << "\n\tPress any key to exit...\n";
        _getch();
    }
    return 0 ;
}
