# Coding cheatsheet

## Язык C

### Синтаксис

- Регистр (большие/маленькие буквы) имеет значение
- Каждое выражение должно заканчиваться `;`
- `//` начинает однострочный комментарий - текст после этого символа не влияет на работу программы, но позволяет программисту "объяснить", что она делает

### Функции

- Функция - участок кода, которым можно воспользоваться по имени

### Условия

```c
// Example

if(condition1)
{
    // идем сюда, если условие (condition1) выполняется
}
else if(condition2)
{
    // идем сюда, если условие (condition1) не выполняется, условие (condition2) выполняется
}
else if(condition3)
{
    // идем сюда, если условия (condition1, condition2) не выполняются, условие (condition3) выполняется
}
else
{
    // и сюда, если никакие условия не выполняются
}
```

В качестве условия можно сравнивать значения:
- `!a` не a
- `a>b` a больше b
- `a<b` a меньше b
- `a==b` a равно b
- `a!=b` a не равно b
- `a>=b` a больше либо равно b
- `a<=b` a меньше либо равно b
- `(a) || (b)` либо a, либо b 
- `(a) && (b)` a и b
- `a%b` a не делится на b

```c
// Example

if(1 == 2);
if(4 < 5);
```

### Циклы

Циклы позволяют выполнять один и тот же код на повторе.

`for` выполняет код заданное количество раз

```c
// Example

for(int i=0; i<10; i++)
{
    Serial.println(i);
}
```


## Arduino

[Официальная документация](https://www.arduino.cc/reference/de/)

### Serial: передача данных между компьютером и Arduino

`Serial.begin(speed)` настроить скорость передачи данных

`Serial.print(value)` передать `value` на компьютер текстовым сообщением

`Serial.println(value)` то же самое, что `Serial.print`, но с переносом строки

```c
// Example

void setup(){
    Serial.begin(115200);
}

void loop(){
    Serial.print("I am ");
    Serial.print(10);
    Serial.println(" years old.");
}
```

### Работа с пинами

`pinMode(pin, mode)` переключить пин номер `pin` в режим `mode` (ввод `INPUT` или вывод `OUTPUT`)

`digitalWrite(pin, value)` вывести на пин номер `pin` значение `value` (`HIGH` или `LOW`)

```c
// Example

void setup(){
    pinMode(13, OUTPUT);
}

void loop(){
    digitalWrite(13, HIGH); // подать ток на пин 13
}
```

### Время

`delay(ms)` приостановить программу на `ms` миллисекунд (в 1 секунде 1000 миллисекунд)

### Звук

`tone(pin, frequency, duration)` воспроизводить на пине номер `pin` ноту частоты `frequency` в течение `duration` миллисекунд

[Таблица частот музыкальных нот](https://upload.wikimedia.org/wikipedia/commons/a/ad/Piano_key_frequencies.png)

```c
// Example

void loop(){
    tone(10, 440, 100);
}
```

## LOCO Board

### Создание проекта

- скопировать `modules.h` и `src` в папку проекта

- `#include "src/locoboard.h"`

### Пульт управления

`setup_ir()` настроить пульт управления (нужно сделать 1 раз в `setup`). Приемник должен быть подключен к пину 2.

`check_ir_button_pressed()` проверить, была ли нажата какая-то кнопка на пульте

`get_ir_button()` кнопка, которая была нажата на пульте. Доступные кнопки: `BTN_0`...`BTN_9`, `BTN_UP`, `BTN_DOWN`, `BTN_LEFT`, `BTN_RIGHT`, `BTN_OK`, `BTN_STAR`, `BTN_SHARP`.

```c
// Example

void setup() {
  setup_ir();
  Serial.begin(115200);
}

void loop() {
  if(check_ir_button_pressed())
  {
    if(get_ir_button() == BTN_0)
    {
      Serial.println(0);
    }
  }
}
```

### Серво-мотор

`setup_servo_pins()` настроить серво-моторы (нужно сделать 1 раз в `setup`).

`rotate_servo(servo_ind, degrees)` повернуть мотор номер `servo_ind` (`SERVO_S1-3`) на `degrees` градусов (0 - 180).

```c
// Example

void setup() {
  setup_servo_pins();
}

void loop() {
  rotate_servo(SERVO_S1, 0);
  rotate_servo(SERVO_S2, 90);
  rotate_servo(SERVO_S3, 180);
}
```

### RGB светодиоды

`setup_addr_leds()` настроить RGB светодиоды (нужно сделать 1 раз в `setup`).

`set_led_color(led_index, r, g, b)` установить цвет светодиода номер `led_index` (`LED1-4`) в значение `r` (0-255), `g` (0-255), `b` (0-255). Значения RGB для выбранного цвета можно проверить по [ссылке](https://www.rapidtables.com/web/color/RGB_Color.html).

```c
// Example

void setup() {
  setup_addr_leds();
}

void loop() {
  set_led_color(LED_1, 255, 0, 0);
  set_led_color(LED_2, 0, 255, 0);
  set_led_color(LED_3, 0, 0, 255);
  set_led_color(LED_4, 255, 255, 255);
}
```

### Кнопка User

`setup_user_button()` настроить кнопку User (нужно сделать 1 раз в `setup`).

`check_user_button_pressed()` проверить, нажата ли в данный момент кнопка (возвращает `true` или `false`).

```c
// Example

void setup() {
  setup_user_button();
}

void loop() {
  if(check_user_button_pressed()) digitalWrite(13, HIGH);
  else digitalWrite(13, LOW);
  delay(10);
}
```

### LED матрица

`clear_led_matrix()` очистить матрицу.

`set_led_matrix_pixel(row, col, value)` включить или выключить выбранный пиксель, т.е. проставить пиксель с координатами `row`, `col` (0-7) в значение `value` (`HIGH` или `LOW`).

`show_led_matrix()` показать изменения пикселей на матрице. Эту функцию нужно использовать каждый раз после `clear_led_matrix` и `set_led_matrix_pixel`, чтобы соответствующие пиксели действительно включились/выключились.

```c
// Example

void loop() {
  set_led_matrix_pixel(1, 5, HIGH);
  show_led_matrix();
  delay(1000);
  clear_led_matrix();
  show_led_matrix();
  delay(1000);
}
```

### Потенциометр (крутилка)

`read_potentiometer_value()` получить текущее значение потенциометра (0 - 1024).

```c
// Example

int val = 0;

void setup() {
  Serial.begin(115200);
}

void loop() {
  val = read_potentiometer_value();
  Serial.println(val);
  delay(10);
}
```

### Сенсор расстояния

`setup_distance_sensors()` настроить сенсоры расстояния (нужно сделать 1 раз в `setup`).

`measure_distance_mm(sensor_id)` получить расстояние в миллиметрах с сенсора номер `sensor_id` (`DISTANCE_L1-3`).

```c
// Example

void setup() {
  setup_distance_sensors();
  Serial.begin(115200);
}

void loop() {
  Serial.println(measure_distance_mm(DISTANCE_L1));
}
```