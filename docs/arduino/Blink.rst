Lập trình GPIO với  ESP32 bằng Arduino IDE.
--------------------------------------------------------

Giới thiệu.
===============

Sau khi Arduino IDE đã hỗ trợ board ESP32vn IoT Uno thì mình làm một bài ví dụ đơn giản về IO  là nhấp nháy LED.

Chuẩn bị.
==========

* Phần cứng:

    * Board ``ESP32 IoT Wifi Uno.``
    * ``LED``.
    * ``Trở 330 ohm``.
    * ``Testboard cắm``.
    * ``Dây nối``.

* Phần mềm:
	
    * Arduino IDE (xem cài đặt `Arduino IDE với ESP IoT Wifi Uno tại đây <https://esp32.vn/arduino/install.html>`_.

Kết nối.
========

    * Chân ``Anode`` của ``LED``  nối với điện trở đi vào chân 21 trên board ``ESP32 IoT Wifi Uno``.
    * Chân ``Cathode`` nối với cổng GND trên board ``ESP32 IoT Wifi Uno``.

Chưong trình.
=============
Chạy chương trình Arduino IDE lên và copy toàn bộ code dưới đây vào và save với lại với một tên bất kì.

.. code:: cpp

    int LED = 21; 

    void setup(){
        pinMode(LED, OUTPUT);
    }

    void loop(){
        digitalWrite(LED, HIGH);
        delay(500);
        digitalWrite(LED, LOW);
        delay(500);
    }

Nạp chương trình
===================

Kết nối ``ESP32 IoT Wifi Uno`` với máy tính và thực hiện theo `hướng dẫn tại đây <https://esp32.vn/hardware/connection.html#cau-hinh-ket-noi>`_ 

Kết luận
=========

Vì đây chỉ là một chương trình IO đơn giản nên bài viết này mình xin dừng ở đây, chúc các bạn thành công và hẹn gặp lại ở các bài viết khác.