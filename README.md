# perenio-petra01
Универсальный Инфракрасный пульт Perenio Red Atom PETRA01


![photo_5204132837031264583_x](https://user-images.githubusercontent.com/64173457/204721353-a498410a-104e-4130-892d-30e1c0c6f091.jpg)
![photo_5204132837031264584_x](https://user-images.githubusercontent.com/64173457/204721409-9676d13f-477e-4f1b-a145-43b41d9d4639.jpg)

Через щель в крышке корпуса метиатором (пластиковой карточкой или ногтем ) отщелкиваем крепления и открываем крышку.

![photo_5204132837031264587_y](https://user-images.githubusercontent.com/64173457/204721649-d2efd010-427c-4223-a2c4-a3d8971028e1.jpg)
все нужные TP предусмотрительно подписаны.

наблюдаем очень полезный модуль TYWE3S
![photo_5204132837031264588_y](https://user-images.githubusercontent.com/64173457/204721758-9d592477-362d-488a-b6c2-ed515d62cb0f.jpg)

Подключаем gnd, 3,3v, tx, rx и gpio0

Для начала полезно считать текущую прошивку. вдруг пригодится.

Прошиваем в esphome:

```
substitutions:
  board_name: perenio-ir
 
esphome:
  name: $board_name
  platform: ESP8266
  board: esp01_1m

wifi:

logger:
  baud_rate: 0

api:
  password: !secret passwordapi

ota:
  password: !secret passwordota
web_server:
  port: 80

light:
  - platform: status_led
    name: Status_$board_name
    pin:
      number: GPIO04
  
button:
  - platform: restart
    name: Reset.$board_name
  - platform: template
    name: LG.$board_name
    on_press:
      - remote_transmitter.transmit_lg:
          data: 0x000800FF
          nbits: 32
  - platform: template
    name: nec.$board_name
    on_press:
      - remote_transmitter.transmit_nec:
          address: 0x1000
          command: 0xFF00

remote_receiver:
  pin:
    number: GPIO05
    inverted: true
  dump: all  
remote_transmitter:
  pin: GPIO14
  carrier_duty_percent: 50%
binary_sensor:
  - platform: gpio
    pin: GPIO13
    name: "gpio13"  
