Lập trình ESP32
===============

Đây là một dự án tài liệu/phần cứng/mã nguồn mở hỗ trợ lập trình ESP32 một cách nhanh chóng dựa trên nền tảng IoT Development Kit (IDF) từ Espressif.

Đi kèm với chương trình là board mạch phần cứng mở ``ESP32-Uno`` với các tính năng:

- Duacore CPU ESP32 240MHz, 512KB Internal RAM, 4MB FLASH
- WiFi 2.4GHz tốc độ cao, nhiều chế độ hoạt động (Station, access point)
- Bluetooth 4.2, BLE, Audio
- Tương thích Arduino UNO, và tương thích với thư viện arduino cho ESP32
- Có nút nhấn, LED, oled header, nguồn dòng 2A cho các shield ứng dụng GSM, khe cắm thẻ nhớ
- Hỗ trợ network GPRS/3G thông qua module external với các api network sẵn có
- Hỗ trợ các giao thức kết nối có dây: Serial, CAN, Ethernet, I2C, SPI, IRDA, TOUCH
- Hỗ trợ nhiều thư viện kết nối cho #ioT: MQTT, CoAP, HTTP, SSL, Websocket, IPv6 ...
- Hỗ trợ 4MB PSRAM bên ngoài dùng cho ứng dụng phức tạp
- Hỗ trợ I2S/DAC/ADC cho các ứng dụng Audio
- Hỗ trợ Camera/LCD
- Lowpower với dynamic clock switch & ULP Processor siêu tiết kiệm năng lượng

Pinout
******

.. image:: _static/Esp32-boad__1_.png
   :width: 800

Mạch nguyên lý
**************

.. image:: _static/sch.svg
   :width: 800

.. toctree::
   :caption: Cơ bản
   :maxdepth: 2

   Cài đặt <basic/index>

.. toctree::
   :caption: Network/Protocol
   :maxdepth: 1


.. toctree::
   :caption: API
   :maxdepth: 1

.. toctree::
   :caption: Tài liệu ESP32

.. toctree::
   :caption: Đóng góp và bản quyền
   :maxdepth: 1

   Hướng dẫn đóng góp <contributing>



Indices
=======

* :ref:`genindex`
