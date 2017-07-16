Digital To Analog Converter
===========================

Tổng quan
--------

ESP32 có 2 kênh DAC 8 bit là GPIO25 ( kênh 1) và GPIO26 (kênh 2)

Bộ điều khiển DAC cho phép các kênh này được thiết lập điện áp tùy ý.

Các kênh DAC này cũng được dùng để điều khiển với kiểu DMA được viết với dữ liệu mẫu thông qua :doc:`I2S driver <i2s>` khi sử dụng chế độ ``build-in DAC``

Ví dụ minh họa
-------------------

Cài đặt DAC kênh 1 (GPIO25) với mức điện áp ngõ ra xấp xỉ 0,78xVDD_A (VDD*200/255). VỚi điện áp VDD_A=3.3v, ta sẽ được điện áp ngõ ra DAC kênh 1 (GPIO25) là 2.59V::

  #include <driver/dac.h>

  ...

      dac_output_enable(DAC_CHANNEL_1);
      dac_output_voltage(DAC_CHANNEL_1, 200);

API Reference
-------------

**Thư viện**

``#include <driver/dac.h>``

**Các hàm trong thư viện dac.h**

.. code:: bash
	
	esp_err_t dac_output_voltage(dac_channel_t channel, uint8_t dac_value)

Dùng để cài đặt điện áp ngõ ra DAC
Ngõ ra DAC 8-bit nên gía trị tối đa là 255 tương đương với VDD.

**chú ý:** cần phải khai báo chân DAC trước khi gọi hàm này. Kênh DAC 1 ứng với chân GPIO25 và DAC2 ứng với chân GPIO26.

**Gía trị trả về**

	* ``ESP_OK`` : khai báo thành công
	* ``ESP_ERR_INVALID_ARG`` : lỗi đối số

**Các đối số**

	* ``chanel`` : Kênh DAC được cấu hình
	* ``dac_value`` : gía trị ngõ ra DAC

.. code::

	esp_err_t dac_output_enable(dac_channel_t channel)

Cho phép ngõ ra DAC

**chú ý:** DAC kênh 1 tương ứng với GPIO25, còn DAC kênh 2 tương ứng với GPIO26.Kênh I2S bên trái sẽ được ánh xạ tới DAC kênh 2,kênh I2S  bên phải sẽ được ánh xạ tới DAC kênh 1.

**Các đối số** 
	*``channel``: lựa chọn kênh DAC
.. code::

	esp_err_t dac_output_disable(dac_channel_t channel)

Vô hiệu hóa ngõ ra kênh DAC.

**Các đối số**
	*``channel``: lựa chọn kênh DAC


.. code::

	esp_err_t dac_i2s_enable()

Cho phép dữ liệu ngõ ra DAC từ I2S


.. code::

	esp_err_t dac_i2s_disable()

Vô hiệu hóa dữ liệu ngõ ra DAC từ I2S

**Các đối số**

.. code:: bash

	enum dac_channel_t

**Gía trị**
	* ``DAC_CHANNEL_1 = 1`` :lựa chọn DAC kênh 1 (GPIO25)
	* ``DAC_CHANNEL_2`` :lựa chọn DAC kênh 1 (GPIO26)
	* ``DAC_CHANNEL_MAX``
