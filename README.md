# Project_HandS
#include <Servo.h>    //서보모터 라이브러리 include
 
Servo microServo;    //서보모터 객체 선언
const int servoPin = 3;    //서보모터 제어핀을 3번에 할당
int buzzer = 6; // Buzzer를 6번에 할당
int Sensor1 = 8;    // 적외선 센서핀을 8번에 할당
int Sensor2;        // 진동센서, sensor의 값이 0~1023 사이에서 나옵니다.


int val;
int count = 0;

void setup()
{
    Serial.begin(9600);
    pinMode(Sensor1, INPUT);    // 적외선 센서값을 입력으로 설정
    pinMode(buzzer, OUTPUT);    // 부저를 출력으로 설정
    microServo.attach(servoPin);    //서보모터 초기화
    microServo.write(0);   // 서보모터를 0도로 설정
}

void loop()
{
    val = digitalRead(Sensor1);  // 센서값 읽어옴

  if (val == LOW) {          // 장애물 감지 시
    microServo.write(120);    //서보모터를 120도까지 회전
    delay(1000);
    microServo.write(0);    //서보모터를 0도까지 회전
    count = count + 1;      // 연속된 감지의 횟수 측정
    delay(1000);
    
    Sensor2 = analogRead(A0); // 진동감지 센서의 감지 시작
    if (Sensor2<900){    // 진동센서의 값이 900 미만이면 (진동이 발생하면)
      tone(buzzer, 2000, 1000);     // 주파수 2000Hz의 부저를 1000ms동안 발생
      delay(100);
    }
    else{ 
      noTone(buzzer);
      delay(100);
    }
  } 
  else {                   // 장애물이 감지되지 않으면
    count = 0;           
    delay(1000);
  }
  
  if (count == 5) {       // 5번 반복후에는 10초간 대기 
    delay(10000);
    count = 0;
  }
}
