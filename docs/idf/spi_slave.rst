SPI Slave driver
=================

Overview
--------

The ESP32 has four SPI peripheral devices, called SPI0, SPI1, HSPI and VSPI. SPI0 is entirely dedicated to
the flash cache the ESP32 uses to map the SPI flash device it is connected to into memory. SPI1 is
connected to the same hardware lines as SPI0 and is used to write to the flash chip. HSPI and VSPI
are free to use, and with the spi_slave driver, these can be used as a SPI slave, driven from a 
connected SPI master.

The spi_slave driver
^^^^^^^^^^^^^^^^^^^^^

Trình điều khiển giao tiếp spi_slave của ESP32 cho phép HSPI và VSPI giao tiếp full-duplex (song công) như một thiết bị SPI slave. Nó có thể dùng DMA cho việc truyền và nhận dữ liệu với mọi độ dài của dữ liệu.

Terminology
^^^^^^^^^^^

Trình điều khiển spi_slave dùng những thuật ngữ sau:

* Host: Thiết bị ngoại vi giao tiếp SPI bên trong ESP32 khởi tạo truyền giao tiếp SPI, HSPI hoặc VSPI.
 
* Bus: là bus giao tiếp SPI, thường cho tất cả thiết bị giao tiếp SPI kết nối với một master. Nói chung bus SPI bao gồm miso, mosi, sclk và các chân tùy chọn tín hiệu quadwp và quadhd. Các SPI slave kết nối với các chân tín hiệu như nhau. Mỗi SPI slave kết nối với một chân tín hiệu CS (chip select) riêng.

  - miso - Còn được gọi là q, đây là chân output của ESP32 đến thiết bị master.

  - mosi - Còn được gọi là d, đây là chân input của ESP32 lấy tín hiệu từ thiết bị master.

  - sclk - xung giữ nhịp cho giao tiếp spi, mỗi nhịp trên chân sclk báo 1 bit dữ liệu đến hoặc đi.

  - cs - Chip Select. Một chip select hoạt động, mô tả một đường truyền từ một slave.

* Transaction: Khi chân CS hoạt động, việc truyền dữ liệu từ hoặc đến master xảy ra, và CS sẽ ngưng hoạt động. Tín hiệu truyền sẽ không bao giờ bị gián đoạn bởi các đường truyền khác.


SPI transactions
^^^^^^^^^^^^^^^^

Một trao giao tiếp SPI full-duplex (song công) bắt đầu khi chân CS của master được kéo xuống mức thấp. Sau đó, master gửi ra một xung clock từ bộ xử lí trên đường CLK: mỗi nhịp xung clock của bộ xử lí gây ra là một bit dữ liệu sẽ được truyền từ master đến slave trên đường MOSI và truyền ngược lại từ slave qua master trên đường MISO. Sau khi giao tiếp kết thúc thì chân CS sẽ trở lại mức cao.

Using the spi_slave driver
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Khởi tạo một thiết bị ngoại vi giao tiếp SPI như một slave bằng cách gọi ``spi_slave_initialize``. Bạn phải set chính xác các chân IO trong cấu trúc ``bus_config``. Hãy cẩn thận set các chân tín hiệu không cần thiết để nó là -1. Bạn cần có một kênh DMA (1 hoặc 2) nếu các dữ liệu truyền lớn hơn 32 byte, nếu không thì bạn có thể set ``dma_chan`` là 0.


- To set up a transaction, fill one or more spi_transaction_t structure with any transaction 
  parameters you need. Either queue all transactions by calling ``spi_slave_queue_trans``, later
  quering the result using ``spi_slave_get_trans_result``, or handle all requests synchroneously
  by feeding them into ``spi_slave_transmit``. The latter two  functions will block until the 
  master has initiated and finished a transaction, causing the queued data to be sent and received.


- Optional: to unload the SPI slave driver, call ``spi_slave_free``.


Transaction data and master/slave length mismatches
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Normally, data to be transferred to or from a device will be read from or written to a chunk of memory
indicated by the ``rx_buffer`` and ``tx_buffer`` members of the transaction structure. The SPI driver
may decide to use DMA for transfers, so these buffers should be allocated in DMA-capable memory using 
``pvPortMallocCaps(size, MALLOC_CAP_DMA)``.

The amount of data written to the buffers is limited by the ``length`` member of the transaction structure:
the driver will never read/write more data than indicated there. The ``length`` cannot define the actual
length of the SPI transaction; this is determined by the master as it drives the clock and CS lines. In
case the length of the transmission is larger than the buffer length, only the start of the transmission
will be sent and received. In case the transmission length is shorter than the buffer length, only data up 
to the length of the buffer will be exchanged.

Warning: Due to a design peculiarity in the ESP32, if the amount of bytes sent by the master or the length 
of the transmission queues in the slave driver, in bytes, is not both larger than eight and dividable by 
four, the SPI hardware can fail to write the last one to seven bytes to the receive buffer.


Application Example
-------------------

.. include:: /_build/inc/spi_commom.inc

API Reference
-------------

.. include:: /_build/inc/spi_slave.inc

