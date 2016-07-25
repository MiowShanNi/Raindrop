import ddf.minim.*;
import ddf.minim.signals.*;
import ddf.minim.analysis.*;
import ddf.minim.effects.*;
AudioPlayer player;
Minim minim;

boolean right = false;
boolean left = false;
boolean menu = true;
    //Create the bucket, and array of raindrops
Bucket buck = new Bucket();
RainDrop[] rain = new RainDrop[30];


void setup() {
    size (400,500);
     minim = new Minim(this);
  player = minim.loadFile("Smile.mp3.mp3");
  player.play();
   
     //create the raindrops in the array
  for (int i = 0; i<rain.length;i++) {
    rain[i] = new RainDrop(); 
    
  }
    
}
//counters to keep track of the game's status
int dropsMissed = 0;
int dropsCaught = 0;
int gameState = 1;
void draw() {
  
     if(menu == true){   
     PImage img;
     img = loadImage("mainsky..jpg");
     smooth();
     background(155); 
     
     
    
    image(img,0,0);                                                    
    textSize(18);
    fill(183,30,76);                                                      
    text("START GAME",40,90);
    text("INSTRUCTIONS",140,187);
    text("EXIT",298,270); 
      
      noStroke();
      fill(255);                                                        
      ellipse(mouseX-5,mouseY+10,20,10); //bottom
      ellipse(mouseX,mouseY-8,20,10);  //top
      ellipse (mouseX-10,mouseY,25,15); //left
      ellipse(mouseX+10,mouseY,25,15); //right
      ellipse(mouseX+5,mouseY+10,20,10); //btm
      ellipse(mouseX,mouseY+5,10,10); //mid
   
    
    if(mousePressed  && (mouseButton==LEFT) && (mouseX>40 && mouseX<150 && mouseY>75 && mouseY<95)){                  
       menu = false;
       
      }
      
    else if(mousePressed  && (mouseButton == LEFT) && (mouseX>130 && mouseX<270 && mouseY>150 && mouseY<220)){
      
          background(0);                                               
          fill(255);         
          textSize(30);
          text("HOLD TO READ",90,60);          
          textSize(20);
          fill(random(255),random(255),random(255));
          text("Catch the raindrop!",100,160);
          textSize(25);
          fill(175);
          text("To control:",128,200);
          textSize(20);
          text("Slide your cursor on the bottom bar",30,230);
          text("or",188,260);
          text("Use left and right keypad",80,290);
          text("Catch > Miss = WIN",105,320);
          
          
    }
    else if(mousePressed  && (mouseButton == LEFT) && (mouseX>260 && mouseX<350 && mouseY>220 && mouseY<290)){
      exit();
      }
    
   }
 if (menu == false){
   
   
  PImage photo;
  photo = loadImage("sky-01.jpg");
  textAlign(CENTER);
  smooth();
  size(400, 500);
  
  

  background (photo);
  if (gameState==1) {
    fill(183,30,76);
    rect(0, 401, width, height-401);
    stroke(0);
    strokeWeight(5);
    line((width/2.8), 401, (width/2.8), height);
    line((2*width/3.15), 401, (2*width/3.15), height); //right
    line(0,400,400,400);
    line(2.5,0,2.5,500);
    line(397,0,397,500);
    line(0,497,500,497);
    line(0,2.5,500,2.5);
    fill(255);
    textSize (20);
    text("LEFT", (width/4)-30, 450);
    text("STAY", (width/2), 450);
    text("RIGHT", (3*width/4) + 25, 450);
 
    if (mouseY>400) {
      checkMouse();
    }
    buck.display();
 
    //check to see if the raindrops should display
    for (int i = 0; i<rain.length;i++) {
      if ((int)(millis()/1500) ==rain[i].timeToDisplay) {
        rain[i].makeRain();
      }
    }
    //keep drawing the raindrops and making them fall
    for (int i=0; i<rain.length;i++) {
 
      if (rain[i].falling) {
        rain[i].display();
        rain[i].fall();
      }
      //if the bucket is under the raindrop, call it a catch, otherwise, call a miss
      if (abs(rain[i].locationY - buck.locationY+35)<=5 && abs(rain[i].locationX - buck.locationX)<=35) {
        rain[i].caught();
        dropsCaught ++;
      }
      if (rain[i].locationY>400) {
        rain[i].miss();
        dropsMissed++;
      }
    }
    //allows for smooth movement of the bucket
    if (right) {
      buck.moveRight();
    }
    if (left) {
      buck.moveLeft();
    }
 
     textSize(15);
    fill(0);
    strokeWeight(1);
    text("Rain drops caught: " + dropsCaught, 200, 30);
    text("Rain drops missed: " + dropsMissed, 200, 60);
    //is the game over?
    if (dropsMissed + dropsCaught == 30) {
      gameState = 2;
    }
  }
  //display end of game text
  if (gameState == 2) {
     fill(0);
     textSize(20);
     text("You caught " +dropsCaught+ " rain drops", 200, 100);
    if (dropsCaught > dropsMissed) {
      text("CONGRATULATIONS, YOU WIN!", 200, 200);
    }
    else text("YOU LOSE!", 200, 200);
    noStroke();
      fill(255);                                                        
      ellipse(mouseX-5,mouseY+10,20,10); //bottom
      ellipse(mouseX,mouseY-8,20,10);  //top
      ellipse (mouseX-10,mouseY,25,15); //left
      ellipse(mouseX+10,mouseY,25,15); //right
      ellipse(mouseX+5,mouseY+10,20,10); //btm
      ellipse(mouseX,mouseY+5,10,10); //mid
    
    fill(255);
    textSize(30);
    text("EXIT",200,350);
    
   
   if(mousePressed && (mouseButton == LEFT) && (mouseX>180 && mouseX<230 && mouseY>320 && mouseY<380)){
         exit();
  }
}}
}

 
void checkMouse() {
    if ((int)mouseX> (int)(2*width/3.15)) {
    right = true;
    left = false;
  }
  if ((int)mouseX< (int)(width/2.8)) {
    left = true;
    right = false;
  }
  if ((int)mouseX<(int)(2*width/3.15) && (int)mouseX> (int)(width/2.8)) {
    left = false;
    right = false;
  }
}
 
 //again, allows for smooth left and right movement
void keyPressed() {
  if (keyCode==RIGHT) {
    right = true;
  }
 
  if (keyCode==LEFT) {
    left = true;
  }
}
 
void keyReleased() {
  if (keyCode==RIGHT) {
    right = false;
  }
 
  if (keyCode==LEFT) {
    left = false;
  }
}
 
class Bucket{
 //centers of the bucket(kinda)
  int locationX;
  int locationY;
  int speedChange;
   
  Bucket(){
    //puts the bucket at the center of the screen
   locationX = 200;
   locationY = 385;
   speedChange = 7;
    
  }
   
  void moveRight(){
    locationX += speedChange;
    //keep the bucket on the screen
    if (locationX> 358){
      locationX = 358;
    }
  }
   
  void moveLeft(){
    locationX -= speedChange;
    //keep the bucket on the screen
    if (locationX< 42){
      locationX = 42;
    }
     
  }
   
  void display(){
    fill(116,80,27);
    quad(locationX-35,locationY-35,
        locationX-30,locationY + 10,
        locationX + 30,locationY + 10,
        locationX + 35,locationY - 35);
    
     
    
  }
   
}
class RainDrop{
  int locationX;
  int locationY;
  boolean falling;
  int timeToDisplay;
  int fallingSpeed;
   
   
  RainDrop(){
    locationY = 5;
    locationX = (int)random(10,380);
    //bucket should not be falling
    falling = false;
    //when the bucket should fall
    timeToDisplay = (int)random(2,40);
    fallingSpeed = (int)random(2,5);
  }
   
  void display(){
    noStroke();
    fill(random(255),random(255),random(255));
    ellipse(locationX,locationY, 15,15);
    triangle(locationX-8,locationY,
             locationX,locationY-15,
             locationX + 8, locationY);              
  }
   
  void fall(){
    locationY +=fallingSpeed;
  }
   
  void makeRain(){
    falling = true;
  }
 //these two methods put the drop off the screen
  void caught(){
     
    fallingSpeed = 0;
    locationX = 999;
  }
  void miss(){
   fallingSpeed = 0;
  locationY = -100;
  }}