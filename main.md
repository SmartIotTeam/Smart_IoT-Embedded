# main

옷장 하드웨어입니다.

## spec
사용보드: Nucleo-F103RB

## Code

main.c
```C

/*-------------------USER CODE-------------------*/
uint8_t rx_data;

/*__weak*/void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) {
	if(huart->Instance == USART3) {
		push(rx_data);
		// 데이터 1개를 수신하면 인터럽트를 발생시킨다.
		HAL_UART_Receive_IT(&huart3, &rx_data, 1);
	}
}

/*__weak*/int __io_putchar(int ch)
{
	if (ch == '\n') {
		HAL_UART_Transmit(&huart3, (uint8_t *)"\r", 1, 0xFFFF);
	}
	HAL_UART_Transmit(&huart3, (uint8_t *)&ch, 1, 0xFFFF);  

	return ch;
}


/*__week*/int __io_getchar(void)
{
	uint8_t ch = 0;

	__HAL_UART_CLEAR_OREFLAG(&huart3);

	HAL_UART_Receive(&huart3, (uint8_t *)&ch, 1, 0xFFFF);
	HAL_UART_Transmit(&huart3, (uint8_t *)&ch, 1, 0xFFFF);  

	return ch;
}

/*-------------------MAIN CODE-------------------*/
int main(void)
{
  /* USER CODE BEGIN 1 */
	uint8_t data;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_USART2_UART_Init();
  MX_USART3_UART_Init();
  /* USER CODE BEGIN 2 */
  HAL_UART_Receive_IT(&huart3, &rx_data, 1);
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
	  if(isEmpty() == 0) {
		  data = pop();

		  // 받은 데이터를 전송한다.
		  HAL_UART_Transmit(&huart2, &data, 1, 10);
		  HAL_UART_Transmit(&huart3, &data, 1, 10);
	  }
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}