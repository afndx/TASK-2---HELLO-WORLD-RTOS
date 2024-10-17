# Real Time Operation System(RTOS)
hubungan antar task pickButton,dispUARTTask,getADCTask,dan ledLEVELTask

![f554a638-d6e7-48b3-9334-75263179243c](https://github.com/user-attachments/assets/1d482952-dc6d-49da-a7f0-9d64586e5351)

diagram tersebut mengilustrasikan hubungan antar task yang digunakan dalam project RTOS (Real Time Operating System) Hello World, dimana:
 
 * Task pickButton untuk memantau input tombol.
 
 * Task getADC untuk membaca data analog.
 
 * Task ledLEVEL untuk menampilkan hasil pembacaan ADC dengan LED.
 
 * Task dispUART untuk berkomunikasi melalui UART dengan pengguna.

Ketika beberapa tugas siap untuk dijalankan pada saat yang sama, prioritas tugas menentukan urutan di mana tugas dijalankan. Ini terjadi dalam sistem berbasis Real-Time Operating System (RTOS), di mana setiap tugas atau thread berjalan secara bersamaan, atau secara bersamaan.

* getADCTask memiliki prioritas lebih tinggi (osPriorityAboveNormal) karena pembacaan nilai tegangan ADC dianggap penting dalam kontrol hardware.

* defaultTask, pickButtonTask, ledLEVELTask, dan dispUARTTask memiliki prioritas yang sama (osPriorityNormal).
* Task dengan prioritas osPriorityNormal akan dijalankan bergantian sesuai waktu yang tersedia.
* Task getADCTask (prioritas lebih tinggi) akan mendapatkan lebih banyak waktu CPU dibanding task dengan prioritas normal, sehingga pembaruan pengukuran tegangan lebih sering terjadi.alur program tersebut:

Inisialisasi Sistem:

* HAL_Init(): Menginisialisasi hardware abstraction layer.
SystemClock_Config(): Mengatur clock sistem dengan PLL dan sumber clock eksternal (HSE).
Inisialisasi periferal: GPIO untuk LED dan tombol, ADC untuk sensor/input analog, I2C (tidak digunakan), dan UART untuk komunikasi serial.
Penggunaan RTOS:

* osKernelInitialize() dan osKernelStart(): Menginisialisasi dan menjalankan task RTOS.
Task yang dibuat:
defaultTask: Tidak melakukan apa-apa (idle task).
pickButtonTask: Mendeteksi tombol yang ditekan.
getADCTask: Membaca nilai ADC dan menyimpannya di variabel x_val.
ledLEVELTask: Menyalakan LED sesuai dengan nilai x_val dan berkedip jika tombol ditekan.
dispUARTTask: Menampilkan data melalui UART (nilai tegangan dan status tombol).

Fungsi Tambahan:

* Light_LED() dan Tf_LED(): Menyalakan dan mematikan LED berdasarkan nilai x_val.
Menu_Display(): Menampilkan nilai tegangan melalui UART.


hubungan antar task dalam program:

* pickButtonTask: Mendeteksi tombol fisik dan mengatur flag button1_pressed jika tombol ditekan.
* getADCTask: Membaca nilai ADC dari sensor dan menyimpan hasilnya di x_val untuk digunakan task lain.
* ledLEVELTask: Mengontrol nyala LED berdasarkan nilai x_val dari getADCTask. Jika tombol ditekan, LED berkedip; jika tidak, LED menyala tetap.
* dispUARTTask: Menampilkan data ADC dan status tombol melalui UART, serta memproses input dari pengguna.
* defaultTask: Task idle yang berjalan ketika task lain tidak aktif, tanpa fungsi spesifik.
  
Alur keseluruhan berjalan secara paralel, di mana setiap task saling melengkapi sesuai fungsinya. Misalnya, getADCTask meng-update nilai ADC yang kemudian digunakan oleh ledLEVELTask untuk mengontrol nyala LED. Di saat yang sama, dispUARTTask memungkinkan interaksi pengguna untuk melihat nilai ADC atau menampilkan pesan berdasarkan interaksi tombol yang dideteksi oleh pickButtonTask.


Demo:






https://github.com/user-attachments/assets/3c7f7b8f-d4ce-4700-9244-17dbda90b554

