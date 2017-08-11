Lập trình cảm ứng với ESP32 IDF
-------------------------------

Giới thiệu
==========

* ESP32 hỗ trợ đọc lên đến 10 bộ cảm biến cảm ứng điện dung (T0 - T9), được kết nối với các chân GPIO cụ thể:

  * ``TOUCH_PAD_NUM0``: Touch pad channel 0 is GPIO4
  * ``TOUCH_PAD_NUM1``: Touch pad channel 0 is GPIO0
  * ``TOUCH_PAD_NUM2``: Touch pad channel 0 is GPIO2
  * ``TOUCH_PAD_NUM3``: Touch pad channel 0 is GPIO15
  * ``TOUCH_PAD_NUM4``: Touch pad channel 0 is GPIO13
  * ``TOUCH_PAD_NUM5``: Touch pad channel 0 is GPIO12
  * ``TOUCH_PAD_NUM6``: Touch pad channel 0 is GPIO14
  * ``TOUCH_PAD_NUM7``: Touch pad channel 0 is GPIO27
  * ``TOUCH_PAD_NUM8``: Touch pad channel 0 is GPIO33
  * ``TOUCH_PAD_NUM9``: Touch pad channel 0 is GPIO32

* Dưới đây là ví dụ đọc và hiển thị giá trị từ các cảm biến cảm ứng điện dung pad được kết nối với kênh T0 (GPIO4) lập trình trên board ESP32-Wifi-Uno.

* Khi đã được cấu hình, ESP32 liên tục đo điện dung của cảm biến cảm ứng. Điện dung lớn khi ngón tay chạm vào Touch PAD và giá trị đo sẽ nhỏ. Trong trường hợp ngược lại, khi không được chạm, giá trị điện dung nhỏ và giá trị đo lớn.

Demo
====
.. youtube:: https://www.youtube.com/watch?v=SxPDVPu8tug

Chuẩn bị
========

+-------------------------------+--------------------------------------------+
| **Phần cứng**                 | **Link**                                   |
+===============================+============================================+
| Board ESP32-Wifi-Uno          | https://github.com/esp32vn/esp32-iot-uno   |
+-------------------------------+--------------------------------------------+

Hướng dẫn
=========

Tải dự án mẫu:
**************
.. code:: bash

    git clone https://github.com/espressif/esp-idf.git

Include
*******
.. code:: cpp

    #include "driver/touch_pad.h"

* ``driver/touch_pad.h``: Bao gồm cấu hình các thiết bị ngoại vi trong hệ thống ESP. Chức năng của nó như là hệ thống khởi tạo.

Define
******
.. code:: cpp

		#define LED_GPIO 23
		#define BTN_GPIO 18

* Định nghĩa ``LED_GPIO 23`` là ``GPIO23``.
* Định nghĩa ``BTN_GPIO 18`` là ``GPIO18``.

GPIO
****
.. code:: cpp

    gpio_pad_select_gpio (uint8_t gpio_num);

* Chọn pad làm chức năng GPIO từ IOMUX.

.. code:: cpp

		gpio_set_direction (gpio_num_t gpio_num, gpio_mode_t mode);

* Định hướng GPIO, chẳng hạn như output only, input only, output and input.
* Có 2 đối số được truyền vào hàm:
	* ``gpio_num_t gpio_num``: Lựa chon PIN
		*	``GPIO_NUM_0`` ... ``GPIO_NUM_39``  hoặc ``0`` ... ``39``.
	* ``gpio_mode_t mode``	: Lựa chọn Mode
		* ``GPIO_MODE_INPUT``: input only
		* ``GPIO_MODE_OUTPUT``: output only mode
		* ``GPIO_MODE_OUTPUT_OD``: output only with open-drain mode
		* ``GPIO_MODE_INPUT_OUTPUT_OD``: output and input with open-drain mode
		* ``GPIO_MODE_INPUT_OUTPUT``: output and input mode

.. code:: cpp

		gpio_set_pull_mode (gpio_num_t gpio_num, gpio_pull_mode_t pull);

* Sử dụng chức năng này để configure GPIO pull mode, chẳng hạn như pull-up, pull-down.
* Hàm này có 2 đối số được truyền vào:
	* ``gpio_num_t gpio_num``: Lựa chon PIN
	* ``gpio_pull_mode_t pull``	: Lựa chon chế độ
		* ``GPIO_PULLUP_ONLY``: Pad pull up
		* ``GPIO_PULLDOWN_ONLY``: Pad pull down
		* ``GPIO_PULLUP_PULLDOWN``: Pad pull up and pull down
		* ``GPIO_FLOATING``: Pad floating

.. code:: cpp

    gpio_get_level (gpio_num_t gpio_num);

* Hàm này trả về mức logic giá trị đầu vào:
.. code:: cpp

		touch_pad_read(touch_pad_t touch_num, uint16_t * touch_value);

* Thiết lập mức (LOW hoặc HIGH) cho GPIO.
* Có 2 đối số được truyền vào hàm:
	* ``gpio_num_t gpio_num``: Lựa chon PIN
		*	``GPIO_NUM_0`` ... ``GPIO_NUM_39``  hoặc ``0`` ... ``39``.
	* ``uint32_t level``	: Lựa chọn mức logic
		* ``0``: Mức thấp
		* ``1``: Mức cao

Make file:
**********
.. code:: bash

    PROJECT_NAME := myProject
    include $(IDF_PATH)/make/project.mk

* ``PROJECT_NAME := myProject`` : Tạo ra một mã nhị phân với tên này tức là - myProject.bin, myProject.elf.

Lập trình
=========
    Bây giờ, bạn có thể xem code hoàn chỉnh.

.. code:: cpp

    /* Touch Pad Read Example

      This example code is in the Public Domain (or CC0 licensed, at your option.)

      Unless required by applicable law or agreed to in writing, this
      software is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
      CONDITIONS OF ANY KIND, either express or implied.
    */
    #include <stdio.h>
    #include "freertos/FreeRTOS.h"
    #include "freertos/task.h"

    #include "driver/touch_pad.h"

    /*
      Read values sensed at T0 (GPIO4) available touch pads.
      Print out values in a loop on a serial monitor.
    */
    static void tp_example_read_task(void *pvParameter)
    {
      while (1) {
          uint16_t touch_value;

          ESP_ERROR_CHECK(touch_pad_read(TOUCH_PAD_NUM0, &touch_value));
          printf("T:%4d \n", touch_value);
          vTaskDelay(500 / portTICK_PERIOD_MS);
      }
    }

    void app_main()
    {
      // Initialize touch pad peripheral
      touch_pad_init();

      // Start task to read values sensed by pads
      xTaskCreate(&tp_example_read_task, "touch_pad_read_task", 2048, NULL, 5, NULL);
    }

Hướng dẫn config, nạp và debug chương trình:
********************************************

.. code:: cpp

    make menuconfig
    make flash
    make moniter

Lưu ý
=====
* Hướng dẫn cài đặt `ESP-IDF <https://esp-idf.readthedocs.io/en/latest/index.html>`_
* Nạp và Debug chương trình `xem tại đây <https://esp-idf.readthedocs.io/en/latest/index.html>`_
* Tài nguyên hệ thống xem `tại đây <https://github.com/espressif/esp-idf>`_
