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
### ESP32のreset_reasonを改変
* MPに未定義な理由によるreset原因の場合ESP32の原因コードに0x100をプラスして出力。
* Brownoutを分離し、原因の区別ができるように変更。
* MPにBrownout_resetコードBROWNOUT_RESETを追加
* ESP32のreset原因コードが統一されているかわからないので、IC毎にデコードする必要があるかもしれない。

> [!IMPORTANT]
> Brownoutのような電源が不安定でリセットした場合はディープスリープに入り運用を継続しないほうが安全。よって判別可能にした。
> デバック上、不揮発な領域に原因コードを残したほうがよい。ESPのような無線を利用するICは送信時の使用電力が大きく変化するので発進の際にBrownoutが発生する可能性が高い。
> よって、Brownoutを起こしたとしても、無線を使用しなければ、不揮発領域にメッセージを残すことは可能だろう。

> [!NOTE]
> 処理はMPのコードで記述する。
> main.py
> 1. リセット原因解析
> 2. Brownoutの場合は記録を残し、deepsleep
> 3. 記録を確認。Brownoutの記録があれば、デバック接続まち。電池駆動の場合は過放電を避けるために一定時間経過後にdeepsleep
> 4. 通常処理開始
