Assalamualaikum wr wb.

kali ini kita akan coba oprek i2c di STM32L432KC.

kita bikin I2C Device Scanner..

1. Buka STM32CubeIDE, lalu buat project baru.

2. Pilih STM32L432KC, kali ini saya tidak menggunakan setingan default dari board Nucleo-32. Jadi seting sendiri peripheral yang dibutuhkan.

3. Setelah pilih IC, klik "Next" lalu beri nama project "i2c_device_scanner", klik "Next" lagi.

4. Setup STM32 target's firmware. klik "Finish".

5. pilih serial USART2 sebagai komunikasi serial ke pc.

	Mode : Asynchronous
	TX Pin = PA2
	RX Pin = PA15
	
	(saya mengacu pada board Nucleo-32 STM32L432KC yang saya pakai)
	
	Baudrate : 115200 
	
6. Selanjutnya kita setting I2C portnya. memakai I2C1

7. I2C > I2C
	PA9 = SCL
	PA10 = SDA
	
8. Untuk clock config kita set default dulu.. saya memakai 4 MHz.

9. "Save All"

10. Buka "main.c" (Core>Src>main.c)

11. Tambahkan kelengkapan stdio.h (#include "stdio.h") agar kita bisa menggunakan "printf".

12. tambahkan code berikut di bawah "USER CODE BEGIN 4" dan diatas "USER CODE END 4"

int __io_putchar(int ch)
{
	HAL_UART_Transmit(&huart2, (uint8_t*)&ch, 1, 0xFFFF);
	return ch;
}

13. scan address dari 0 sampai 127

menggunakan code berikut:

  int i = 0;
  int result = 0;
  printf("SCANNING I2C DEVICE...");

  for(i=0; i<128; i++)
  {
	  result = HAL_I2C_IsDeviceReady(&hi2c1, (uint16_t)(i<<1), 2, 2);
	  if(result != HAL_OK) // jika result tidak OK
	  {
		  printf("."); // maka print titik-titik
	  }
	  else // jika result OK
	  {
		  printf("0x%X",i); // maka print address
	  }

	  if(i%24==0) // ini biar gak memanjang kesamping titik-titiknya
	  {
		  printf("\r\n");
	  }
  }

14. "Save" lalu "Build All"
	"Build Finished. 0 errors, 0 warnings" artinya tidak ada kesalahan langsung bisa kita coba ke STM32L432KC.
	
15. klik "i2c_device_scanner" project lalu klik "Run".

16. pada "Config" klik "OK"

17. setelah "Download verified successfully", kita buka serial monitor.

18. Sesuaikan port dan baudrate

19. klik "Connect", lalu tekan tombol "restart" pada board.

20. oke ditemukan device i2c pada address 0x63 ..

Sekian dulu tutorialnya.. semoga bermanfaat

Wassalamualaikum wr wb...




















	
