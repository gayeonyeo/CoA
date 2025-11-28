// Sprint 2

Snake s;
GrowItem g;
ShrinkItem sh;
ColorItem c;

void setup() {
  size(600, 600);

  s = new Snake(300, 300);
  g = new GrowItem(100, 100);
  sh = new ShrinkItem(200, 200);
  c = new ColorItem(400, 300);
}

void draw() {
  background(30);

  s.move();
  s.show();
  
  g.show();
  sh.show();
  c.show();

  // 아이템 충돌 체크
  if (dist(s.headX, s.headY, g.x, g.y) < (s.headSize/2 + g.size/2)) {
    g.applyEffect(s);
    g.ran();
  }
  if (dist(s.headX, s.headY, sh.x, sh.y) < (s.headSize/2 + sh.size/2)) {
    sh.applyEffect(s);
    sh.ran();
  }
  if (dist(s.headX, s.headY, c.x, c.y) < (s.headSize/2 + c.size/2)) {
    c.applyEffect(s);
    c.ran();
  }

  // 벽 충돌
  if (s.headX < 0 || s.headX > width || s.headY < 0 || s.headY > height) {
    gameOver();
  }

  // 자기 몸 충돌
  if (s.hitbody()) {
    gameOver();
  }
}

void gameOver() {
  background(0);
  fill(255, 0, 0);
  textSize(40);
  text("GAME OVER", 180, 300);
  noLoop();
}

//뱀
class Snake {
  float headX, headY;
  float headSize = 20;

  float[] bodyX = new float[50]; 
  float[] bodyY = new float[50];
  float bodySize = 18;

  int bodyLength = 10; 

  float speed = 2;
  float vx = 2, vy = 0;

  int colorValue = 255; 

  Snake(float x, float y) {
    headX = x;
    headY = y;

    for (int i = 0; i < bodyLength; i++) {
      bodyX[i] = x - (i + 1) * 12;
      bodyY[i] = y;
    }
  }

  void move() {
    if (keyPressed) {
      if (keyCode == UP) { vx = 0; vy = -speed; }
      if (keyCode == DOWN) { vx = 0; vy = speed; }
      if (keyCode == LEFT) { vx = -speed; vy = 0; }
      if (keyCode == RIGHT) { vx = speed; vy = 0; }
    }

    // 몸통 이동
    for (int i = bodyLength - 1; i > 0; i--) {
      bodyX[i] = bodyX[i - 1];
      bodyY[i] = bodyY[i - 1];
    }

    if (bodyLength > 0) {
      bodyX[0] = headX;
      bodyY[0] = headY;
    }

    headX += vx;
    headY += vy;
  }

  void show() {
     // 머리: 스케일 d = 1(기준 크기)
    nik(headX, headY, 1, colorValue);

    // 몸통
    for (int i = 0; i < bodyLength; i++) {
      nik(bodyX[i], bodyY[i], 1, colorValue);
    }
  }

  boolean hitbody() {
    // 머리와 연결된 목 부분(인덱스 0~9)은 충돌 체크에서 제외
    for (int i = 10; i < bodyLength; i++) {
      if (dist(headX, headY, bodyX[i], bodyY[i]) < headSize/2) {
        return true;
      }
    }
    return false;
  }
}

// 부모 클래스:Item
class Item {
  float x, y, size = 25;

  Item(float x, float y) {
    this.x = x;
    this.y = y;
  }

  void ran() {
    x = random(50, width - 50);
    y = random(50, height - 50);
  }

}

// 자식클래스 Item
// 길이 증가
class GrowItem extends Item {
  
  GrowItem(float x, float y) {
    super(x, y); // 부모 클래스(Item)의 생성자 호출
  }

  void show() {
    fill(0, 255, 0);
    ellipse(x, y, size, size);
    fill(255);
    textAlign(CENTER, CENTER);
    text("+", x, y);
  }

  void applyEffect(Snake s) {
    // 배열 범위 체크
    if (s.bodyLength + 2 <= s.bodyX.length) {
      // 꼬리 늘리기 로직 (현재 꼬리 위치를 복사해서 길이 추가)
      s.bodyX[s.bodyLength] = s.bodyX[s.bodyLength - 1];
      s.bodyY[s.bodyLength] = s.bodyY[s.bodyLength - 1];  
      s.bodyLength++;
      
      s.bodyX[s.bodyLength] = s.bodyX[s.bodyLength - 1];
      s.bodyY[s.bodyLength] = s.bodyY[s.bodyLength - 1];  
      s.bodyLength++;
    }
  }
}

// 길이 감소
class ShrinkItem extends Item {
  
  ShrinkItem(float x, float y) {
    super(x, y);
  }

  void show() {
    fill(255, 0, 0);
    ellipse(x, y, size, size);
    fill(255);
    textAlign(CENTER, CENTER);
    text("-", x, y);
  }

  void applyEffect(Snake s) {
    if (s.bodyLength > 2) {
      s.bodyLength -= 2;
    }
  }
}

// 색상변경
class ColorItem extends Item {
  
  ColorItem(float x, float y) {
    super(x, y);
  }

  void show() {
    fill(0, 150, 255);
    ellipse(x, y, size, size);
    fill(255);
    textAlign(CENTER, CENTER);
    text("C", x, y);
  }

  void applyEffect(Snake s) {
    s.colorValue -= 20;
    if (s.colorValue < 50) s.colorValue = 50;
  }
}

void nik(float a, float b, float d, float c){ //c=255
  noStroke();
  // 머리 1
  fill(c-104, c-171, c-51);
  ellipse(a + 9*d, b - 2*d, 20*d, 16*d);
  
  // 머리 2
  fill(c-123, c-147, c-53);
  ellipse(a - 12*d, b + 4*d, 16*d, 10*d);

  // 얼굴
  fill(c-25, c-85, c-76);
  ellipse(a, b, 22*d, 22*d);
   
  // 머리 3
  fill(c-104, c-171, c-51);
  ellipse(a + 7*d, b - 6*d, 14*d, 9*d);
  
  // 머리 4
  fill(c-123, c-147, c-53);
  ellipse(a - 4*d, b - 6*d, 20*d, 14*d);

  // 머리 5
  fill(c-123, c-147, c-53);
  ellipse(a + 14*d, b + 10*d, 18*d, 12*d);

  // 눈알
  fill(c-3, c-58, c-200);
  ellipse(a - 4*d, b + d/2, 6*d, 6*d);   // 왼
  ellipse(a + 4*d, b + d/2, 6*d, 6*d);   // 오

  // 눈동자
  fill(c, c-25, c-115);
  ellipse(a - 4*d, b + d/2, 3*d, 3*d);   // 왼
  ellipse(a + 4*d, b + d/2, 3*d, 3*d);   // 오
  
  // 입
  fill(c-76, c-174, c-179); 
  triangle(a - 2*d, b + 5*d, a + 2*d, b + 5*d, a, b + 7*d); 
}
