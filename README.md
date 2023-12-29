# ArtBot
Сборка и програмирование робота-художника.
## Сборка робота
Детали для робота были произведены в Fablab на 3D принтере из PLA и Flex, основание робота было вырезано на лазерном станке
Электроника для робота: Arduino, Амперка.

![Составные части робота](https://github.com/HellraidKraid/ArtBot/assets/144485874/c326ac7c-b40d-4df9-9972-f56141a1e286)

**Рисунок 1 - Детали робота**

![IMG_20231124_162103](https://github.com/HellraidKraid/ArtBot/assets/144485874/00d1282b-0824-4ce0-b870-c8fdba65368e)
![IMG_20231124_162115](https://github.com/HellraidKraid/ArtBot/assets/144485874/841cc4aa-fd7f-4d65-8447-fbbbe197e5e4)

**Рисунок 2.3 - Готовый собранный робот**

## Программирование робота
**Код робота для нарисовки квадрата**

```C++
#include <IRremote.hpp>

#define IR_RECEIVE_PIN 0
#define IR_BUTTON_PLUS 21
#define IR_BUTTON_MINUS 69

#define SPEED_1      5 
#define DIR_1        4

#define SPEED_2      6
#define DIR_2        7

void setup(){
  Serial.begin(9600);
  IrReceiver.begin(IR_RECEIVE_PIN);

  for (int i = 4; i < 8; i++) {     
    pinMode(i, OUTPUT);
  }
}

void loop(){
   if (IrReceiver.decode()) {
      IrReceiver.resume(); // Enable receiving of the next value
      int command = IrReceiver.decodedIRData.command;
      
      switch (command) {
         case IR_BUTTON_PLUS: { 
          digitalWrite(DIR_1, LOW); // set direction
          analogWrite(SPEED_1, 600); // set speed

          delay(800);

          digitalWrite(DIR_2, HIGH); // set direction
          analogWrite(SPEED_2, 600); // set speed

          delay(800);

          digitalWrite(DIR_1, HIGH); // set direction
          analogWrite(SPEED_1, 600); // set speed

          delay(800);

          digitalWrite(DIR_1, LOW); // set direction
          analogWrite(SPEED_1, 600); // set speed

          delay(800);

          digitalWrite(DIR_1, HIGH); // set direction
          analogWrite(SPEED_1, 600); // set speed

          delay(800);

          analogWrite(SPEED_1, 0); 
          analogWrite(SPEED_2, 0);  
          
          break;
        }
        case IR_BUTTON_MINUS: { // stop mototrs
          analogWrite(SPEED_1, 0); 
          analogWrite(SPEED_2, 0);  
          break;
        }
      }
  }
}
```

**Тестирование робота**

https://github.com/HellraidKraid/ArtBot/assets/144485874/058752f2-16ee-484e-bebf-991ba1530f09

**недочёты:**
- Так как передние и задние колёса соединены ремённой передачей и закреплены на болтах без общей оси, под воздействием нагрузки они скошены под углом, что в свою очередь затрудняет передвижение робота.
- Не точные размеры отверстий под болты и фломастер (приходилось дорабатывать инструментами)
