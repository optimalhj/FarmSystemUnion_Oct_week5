# farm_system_union(IoTogether) / 10월 4주차 기술보고서
##  - (산업시스템공학과 2022112386 김호준)

## 저번 주까지의 활동

 저번 주에는 아두이노에 직접 텔레그램을 연동시켜, 아두이노가 특정 상황을 감지하면 텔레그램으로 자료를 보내어 문자를 수신하는 것까지 진행하였다.

 다행히 기기 자체 불량 아닌 불량으로 인한 어려운 시련(?)을 잘 극복하고, 실제로 계획한 활동들을 모두 수행하는 데에 성공하였다.

## 이번 주의 회의와 스터디 활동

 아두이노 기기를 통하여 거리 측정과 텔레그램 알림 시스템을 이용하는 것을 구현하기 까지가 저번 주 활동이었다.
 
 이번 주 회의에서 아두이노 기기에 다양한 센서를 붙여 환경의 다양한 속성들을 파악하고, 이를 기존의 시스템과 잘 연동시키는 것을 목표로 삼았다.

 비록 필자는 저번 활동에서 개인적인 호기심으로 아두이노 LCD를 통해 아두이노 실습을 진행하였지만, 조별 활동에서는 수집한 자료를 시각화하는 활동은 하지 않았음을 알게 되었다.

 그래서 이번 주 스터디에서는 본 조의 시스템과 LCD 모니터를 연동시키고, 기존의 시스템도 개선할 부분을 찾아보기로 하였다.

 ## 실습 시작

  먼저 선술하였던 "수집한 자료를 시각화"한다는 것을 다시 설명하자면, 실제로 아두이노 센서가 측정한 거리를 확인할 때에 아두이노 IDE를 통해서만 확인할 수 있었다.
  휴대폰으로 확인했던 자료는 그저 "물체가 감지"되었다는 내용을 설명할 뿐이지 정확히 얼마인지도 알 수는 없었다.

  그래서 본 조의 이번 주 활동은 '시각화'에 집중하지 않을까는 생각이다.

  우선 LCD 모니터 연동은 필자는 이미 수행해보았기 때문에 하드웨어 연결과 실제로 LCD 상의 자료 노출은 구현할 수 있었다.

  ```
//LCD 디스플레이와 연동시 추가해야하는 코드들

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Distance");
}

void loop() {
  lcd.setCursor(0, 1);
  lcd.print(distanceCM);
}
  ```
LCD디스플레이 이해하기 위한 실습을 수행했을 때, 추가했었던 코드를 기존의 시스템에 추가하는 작업이었다.

하지만 본 조는 LCD에 실시간으로 감지된 자료를 계속 출력하는 것이 아닌, 특정 상황(물체 감지)에서만 LCD에 거리가 출력되는 것으로 응용하였다.

그래서 void loop()문에 적혀있는 코드는 if에 적절히 추가하는 작업을 수행하였다.

한편, 선술하였듯이 휴대폰에 전송될 문자에 측정된 거리를 출력할 수 있도록 코드를 다시 작성하는 활동을 가지기도 하였다.
기존의 메세지를 계속 살려두기 위해 문자열 포맷팅을 이용하였으며, 다행히 원하는 결과를 받을 수 있었다.

<div align="center">
 <img width="881" height="39" src="https://github.com/user-attachments/assets/f604f416-6668-4509-a97e-f5a0baa2cb61" />
 <img width="450" src="https://github.com/user-attachments/assets/4748d57d-0ea6-4aa5-982a-962767657af8" />
 <img width="450" src="https://github.com/user-attachments/assets/13c81544-e7a6-4ba8-b29b-4860eaaed85a" />
</div>

이렇게 시각화와 관련된 하드웨어/소프트웨어를 구현할 수 있었다. 다음 활동에는 본 조의 기존 목적이었던 공부 환경으로 적절한지 판단하기 위한 소음 측정과 다양한 환경 특성들을 파악하기 위한 센서를 기존의 센서에서 대체하여 스터디를 진행할 예정으로 확인된다.

## 통신

아두이노 센서의 측정과 별개로 통신 시스템을 개선해보기로 하였다. 사용되는 아두이노는 D1 R1 모델로 와이파이 칩셋이 내장되어 인터넷만 연결되면 인터넷 상의 활동을 할 수 있다는 장점이 있다.

하지만 아무래도 유선 랜이 아닌 Wifi를 이용하다보니 불안정한 부분이 있잖아 없는 면도 존재한다.

실제 육안으로 아두이노와 Wifi가 연결되어있는지 여부도 파악할 수 없기에, 연결이 끊기면 아두이노 기기 자체가 문제가 발생한 것인지, 인터넷의 문제인지를 전혀 모르는 상황이 연출된다.

이를 위해, 코드에 Wifi연결 여부를 파악할 수 코드를 조건문을 통해 구현하여 인터넷 연결 여부를 바로 파악할 수 있도록 하였다.
<div align="center">
 <img width="1023" height="57" src="https://github.com/user-attachments/assets/96b620a6-96e6-414c-91d5-3283537b380e" />
</div>

내장함수 WiFi.status()를 통해 인터넷 연결 여부를 파악할 수 있다는 것을 알아내었고, 연결이 끊기면 PC의 시리얼 모니터를 통해 시각화까지 구현하였다.

반대로 연결이 계속 이어지면 else문을 통해 정상 작동할 수 있도록 프로그래밍하였다.

그래서 종합적으로 기존의 시스템과 새로운 코드가 융합되어 다음과 같이 코딩을 할 수 있었다. 

  ```
#include <UniversalTelegramBot.h>
#include <WiFiClientSecure.h>
#include <ESP8266WiFi.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

char message[100];

#define WIFI_SSID ""//와이파이 이름
#define WIFI_PASSWORD ""//와이파이 비밀번호

#define BOT_TOKEN "8140194638:AAGOpEGxQ9Qf3nxU6sLhEFPsJLk23sPn7Jw"  //bot token
#define CHAT_ID "5561347315"     //Chat ID

#define TRIG_PIN D7
#define ECHO_PIN D6

WiFiClientSecure client;
UniversalTelegramBot bot(BOT_TOKEN, client);

void setup() {
  Serial.begin(115200);

  Serial.println("");
  Serial.println("와이파이에 연결 중...");
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED){
    Serial.print(".");
    delay(1000);
  }
  Serial.println("");
  Serial.println("와이파이 연결 완료.");
  Serial.println();

  configTime(0, 0, "pool.ntp.org");
  time_t now = time(nullptr);
  Serial.println("시간 동기화 중...");
  while (now < 24 + 3600){
    now = time(nullptr);
    Serial.print(".");
    delay(1000);
  }
  Serial.println("");
  Serial.println("시간 동기화 완료.");
  Serial.println("");

  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Distance");
  
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  unsigned long duration = pulseIn(ECHO_PIN, HIGH);
  float distanceCM = ((34000.0 * (float)duration) / 1000000.0) / 2.0;
  Serial.println(distanceCM);

  if (distanceCM > 2 && distanceCM < 20){
    Serial.println("물체 감지!!");
    lcd.setCursor(0, 1);
    lcd.print(distanceCM);

    if (WiFi.status() != WL_CONNECTED){
      Serial.println("WiFi에 연결이 되어있지 않아 텔레그램으로 메시지를 전송할 수 없습니다.");
    } else {
      sprintf(message, "물체가 감지되었습니다!!\n물체의 거리 : %g", distanceCM);
      Serial.println("텔레그램을 통해 문자를 보냅니다.");
      X509List cert(TELEGRAM_CERTIFICATE_ROOT);
      client.setTrustAnchors(&cert);
      bot.sendMessage(CHAT_ID, message, "");
    }

    for (int i = 0; i < 10; i++){
      delay(1000);
    }
  }
  delay(500);
}
  ```
