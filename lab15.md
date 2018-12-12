---
layout: default
title:# 智能蛇
---

# 智能蛇

## 智能蛇
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <conio.h>  
#pragma warning(disable:4996)

#define SNAKE_MAX_LENGTH 20  
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'
#define MAP_SIZE 12

int read(void);//输入移动方向，返回游戏是否结束
int snakeMove(int);//根据输入的方向先判断是否撞墙、吃到食物或咬到自己，都没有则移动
void print(void);//打印当前游戏状态
void put_food(void);//给出一个新的食物

//表示移动方向
int direction[4][2] = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } };
int direct = 3;
//游戏初始状态
char map[MAP_SIZE][MAP_SIZE] = { "************",
"*XXXXH     *",
"*          *",
"*          *",
"*          *",
"*          *",
"*          *",
"*          *",
"*          *",
"*          *",
"*          *",
"************" };

int snakeX[SNAKE_MAX_LENGTH] = { 0, 1, 1, 1, 1, 1 };
int snakeY[SNAKE_MAX_LENGTH] = { 0, 1, 2, 3, 4, 5 };
int snakeLength = 5;
int food;
//主函数
int main()
{
    srand(time(NULL));
    food = 0;
    put_food();
    //打印初始游戏状态
    print();
    //游戏开始
    while (!read());
    //游戏结束
    printf("Game over!\n");
    getchar();
    return 0;
}
//输入移动方向，返回游戏是否结束
int read(void){
    switch (getch()){
    case 224:                    //方向键区的ASCII码
        switch (getch()){
        case 72:
            direct = 0;
            return snakeMove(0);
            break;
        case 80:
            direct = 1;
            return snakeMove(1);
            break;
        case 75:
            direct = 2;
            return snakeMove(2);
            break;
        case 77:
            direct = 3;
            return snakeMove(3);
            break;
        }
    default:
        return snakeMove(direct);
        break;
    }
}

int snakeMove(int dir){
    int i, x, y;
    x = snakeX[snakeLength] + direction[dir][0];
    y = snakeY[snakeLength] + direction[dir][1];
    //如果吃到食物，则蛇的长度加1，返回0，游戏继续
    if (map[x][y] == '$'){
        map[snakeX[snakeLength]][snakeY[snakeLength]] = 'X';
        snakeLength++;
        snakeX[snakeLength] = x;
        snakeY[snakeLength] = y;
        map[x][y] = 'H';
        put_food();
        print();
        return 0;
    }
    //如果撞到墙或者咬到自己直接返回1，游戏结束
    if (map[x][y] != ' ')
        return 1;
    //将蛇尾位置还原为空格
    map[snakeX[1]][snakeY[1]] = ' ';
    //蛇身向前移动
    for (i = 1; i < snakeLength; i++){
        snakeX[i] = snakeX[i + 1];
        snakeY[i] = snakeY[i + 1];
    }
    //原来蛇头的位置变为蛇身
    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = 'X';
    //蛇头位置的移动
    snakeX[snakeLength] += direction[dir][0];
    snakeY[snakeLength] += direction[dir][1];
    map[snakeX[snakeLength]][snakeY[snakeLength]] = 'H';
    //打印当前游戏状态
    print();
    //返回0，表示游戏继续
    return 0;
}

void print(void){
    //清屏
    system("cls");
    //打印当前游戏状态
    int i, j;
    for (i = 0; i < MAP_SIZE; i++){
        for (j = 0; j < MAP_SIZE; j++){
            printf("%c", map[i][j]);
        }
        printf("\n");
    }
}
//放置食物
void put_food(void){
    int x = 0, y = 0;
    while (map[x][y] != ' '){
        x = rand() % 10 + 1;
        y = rand() % 10 + 1;
    }
    map[x][y] = '$';
}