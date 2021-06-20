# atari__breakout
игра на prossesing3
//передаём информацыю с классов в основную програму
platform Platforma; 
ball Ball; 
bricks Bricks; 

PFont DancingScript; //шрифт для score и record

//переменные score и record
int score=0;
int record=0;
 
void draw(){
  background(0);
  fill(225);
  textAlign(CENTER);
  textSize(20);
  text("SCORE:  " + score, 600, 650);
  text(" BEST SCORE:  " + record, 600, 600);
 //выводим функции с классов 
  Platforma.display();
  Bricks.resetBall(Ball);
  int res = Ball.balln(Platforma);
  
  //счётчик для score и record
  if (res==0){
    if(record < score){
      record = score;
      score=0;
    }
    else {
      score=0;
    }
  }
  if(frameCount % 60 == 0){
    score++;
  }
}

void setup(){
  //создаём наследника классов
  Platforma=new platform((width/2)-100,height-100);
  Ball=new ball();
  Bricks = new bricks();
  size(750,750);
  frameRate(120);
  Bricks.displayBricks();
}
//клас для мяча
class ball{ //сщздаём класс для шарика
  boolean die = false;
  
float ball_x=random(10,657);
float ball_y=random(10,657);
float ball_move_y=2;
float ball_move_x=3;

int r = 255;
int g = 255;
int b = 255; //создаём переменные для цвета
 

int balln(platform d){
  ball_x+=ball_move_x;
  ball_y+=ball_move_y;
  
  if(this.ball_x > d.platform_x && this.ball_x < d.platform_x+200 && this.ball_y+8 > d.platform_y){ //програма проверки на столкновение с платформой и смена цвета при касании с платформой
    color ballColor = color(random(255),random(255),random(255));
    ball_move_y=-ball_move_y;
    fill(ballColor);
  }
  
  if(ball_x>700-15)
  {
    ball_move_x=-ball_move_x;
  }
  else if(ball_x<15)
  {
    ball_move_x=-ball_move_x;
  }
  else if(ball_y>700-15){
    
    die = true;
    ball_move_y=-ball_move_y;
    return 0;
  }
  else if(ball_y<15)
  {
    ball_move_y=-ball_move_y;
  }
  
  if(ball_y > Platforma.platform_y - 15 && ball_x > Platforma.platform_y && ball_x < Platforma.platform_x + 200){ 
    ball_move_y =  -ball_move_y;
    r = (int)random(50,255);
    g = (int)random(50,255);
    b = (int)random(50,255);
  }
  
  fill(r,g,b);
  
  ellipse(ball_x,ball_y,16,16);
  return 1;
}
}
//класс для платформы
class platform { //создаём класс для платформы
  public  float platform_x=300;
  public  float platform_y=600;

platform (float platform_x,float platform_y){
  this.platform_x = platform_x;
  this.platform_y = platform_y;
}
void display() { //програма для управления и рисовки платформы
  fill(225, 5, 197);
  rect(platform_x, platform_y, 200, 30, 20);
  if (keyCode == LEFT && platform_x > 0)
  {
    platform_x-=3;
  }
  else if (keyCode == RIGHT && platform_x < width-200) {
    platform_x+=3;
  }
}
}
//класс для кирпичей
class bricks{ //создаём класс для кирпичей
  int BRICKS_ACROSS = 10, BRICKS_HIGH = 4;
  boolean bricks[][] = new boolean[BRICKS_ACROSS][BRICKS_HIGH];

void displayBricks(){ //рисовка кирпичей
  for (int i = 0; i < BRICKS_ACROSS; i++) {
    for (int j = 0; j < BRICKS_HIGH; j++) {
      bricks[i][j] = true;
    }
  }
}

void resetBall(ball balln){ //програма проверки на столкновение с кирпичями 
  for (int i = 0; i < BRICKS_ACROSS; i++) {
    for (int j = 0; j < BRICKS_HIGH; j++) {
      
      if(balln.ball_x > i*75 && balln.ball_x < i*75+75 && balln.ball_y-8 < j*25+25){
        bricks[i][j] = false;
      }
      
      if (bricks[i][j]) {
        fill(20*(i+j) + 51, 204 - 5*(i+j), 255 - 7*(i+j));
      }
      else fill (0, 0, 0, 0);
      rect(i*75, j*25, 75, 25);
    }
  }
  if(balln.die){ //востановление кирпичей при падении мяча
    for (int i = 0; i < BRICKS_ACROSS; i++) {
      for (int j = 0; j < BRICKS_HIGH; j++) {
        bricks[i][j] = true;
        balln.die = false;
      }
    }
  }
}
}  
