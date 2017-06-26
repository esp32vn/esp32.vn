Lập trình "Hello World" với ESP32 IDF
-------------------------------------
Giới thiệu
=================
Espressif Internet Development Framework (ESP-IDF) sử dụng FreeRTOS để tận dụng tốt hơn hai bộ xử lý tốc độ cao và quản lý nhiều thiết bị ngoại vi được cài sẵn. Nó được thực hiện bằng cách tạo các tác vụ. Hãy bắt đầu bằng chương trình "Hello world" để hiểu rõ hơn.

Chương trình Hello world sau mỗi 10 giây in ra một chuỗi "Hello world" và hiển thị trên màn hình máy tính thông qua chuẩn truyền thông UART.

Demo
==================
.. youtube:: https://www.youtube.com/watch?v=SxPDVPu8tug

Chuẩn bị
==================
    +--------------------+----------------------------------------------------------+
    | **Tên board mạch** | **Link**                                                 |
    +====================+==========================================================+
    | Board IoT Wifi Uno | https://github.com/esp32vn/esp32-iot-uno                 |
    +--------------------+----------------------------------------------------------+

Hướng dẫn
==================

Include thư viện
******************
.. code:: cpp

    #include <stdio.h>
    #include "freertos/FreeRTOS.h"
    #include "freertos/task.h"
    #include "esp_system.h"

* freertos/FreeRTOS.h : Thư viện này bao gồm các thiết lập cấu hình yêu cầu để chạy freeRTOS trên ESP32.
* freertos/task.h: Cung cấp chức năng đa nhiệm. (Chúng tôi sẽ làm đa nhiệm ở các ví dụ sau)
* esp_system.h: Bao gồm cấu hình các thiết bị ngoại vi trong hệ thống ESP. Chức năng của nó như là hệ thống khởi tạo.

Hàm app_main()
******************

Ngay khi khởi động thực hiện chương trình bắt đầu với app_main (), cũng giống như hàm main () thường dùng. Đây là chức năng đầu tiên được gọi tự động.

.. code:: cpp

    void app_main()
    {
        xTaskCreate(&hello_task, "hello_task", 2048, NULL, 5, NULL);
    }


Tác vụ
******************

Các chức năng được gọi là từ nhiệm vụ tạo ra ở trên là một chức năng đơn giản như hình dưới đây. Nó chỉ đơn giản là in chuỗi để UART. Dòng in được cấu hình để UART0 ESP32.

.. code:: cpp

    void hello_task(void *pvParameter)
    {
        printf("Hello world!\n");
        for (int i = 10; i >= 0; i--) {
            printf("Restarting in %d seconds...\n", i);
            vTaskDelay(1000 / portTICK_RATE_MS);
        }
        printf("Restarting now.\n");
        esp_restart();
    }

Lập trình
==================
    Bây giờ, bạn có thể xem code hoàn chỉnh.

    .. code:: cpp

        /* Hello World Example

           This example code is in the Public Domain (or CC0 licensed, at your option.)

           Unless required by applicable law or agreed to in writing, this
           software is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
           CONDITIONS OF ANY KIND, either express or implied.
        */
        #include <stdio.h>
        #include "freertos/FreeRTOS.h"
        #include "freertos/task.h"
        #include "esp_system.h"

        void hello_task(void *pvParameter)
        {
            printf("Hello world!\n");
            for (int i = 10; i >= 0; i--) {
                printf("Restarting in %d seconds...\n", i);
                vTaskDelay(1000 / portTICK_RATE_MS);
            }
            printf("Restarting now.\n");
            fflush(stdout);
            esp_restart();
        }

        void app_main()
        {
            xTaskCreate(&hello_task, "hello_task", 2048, NULL, 5, NULL);
        }
Lưu ý
=================
* Hướng dẫn cài đặt ESP-IDF `tại đây <https://esp-idf.readthedocs.io/en/latest/index.html>`_
* Nạp và Debug chương trình xem `tại đây <https://esp-idf.readthedocs.io/en/latest/index.html>`_
* Tài nguyên hệ thống xem `tại đây <https://github.com/espressif/esp-idf>`_
..
