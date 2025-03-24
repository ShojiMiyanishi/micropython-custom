### ESP32-C3,C6のDeepsleepからの復帰にGPIOを利用できるように改変
例:
```
from machine import deepsleep
from esp32 import wake_on_gpio,WAKEUP_GPIO_LOW,WAKEUP_GPIO_HIGH
def enter_deepsleep():
    a=Pin(0,Pin.IN,hold=True)
    b=Pin(1,Pin.IN,hold=True)
    c=Pin(2,Pin.IN,hold=True)
    wake_on_gpio([a,b],esp32.WAKEUP_GPIO_LOW);
    wake_on_gpio([c],esp32.WAKEUP_GPIO_HIGH);
    deepsleep()
```   
