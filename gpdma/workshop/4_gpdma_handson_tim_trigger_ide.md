----!
Presentation
----!

# Start TIM15

Start TIM15 with `HAL_TIM_Base_Start`

Add 

```c
 HAL_TIM_Base_Start(&htim15);
```

at the end of `/* USER CODE BEGIN 2 */` section

```c-nc
  /* USER CODE BEGIN 2 */
  MX_YourQueueName_Config();

  HAL_DMAEx_List_LinkQ(&handle_GPDMA1_Channel15, &YourQueueName);

  HAL_DMAEx_List_Start(&handle_GPDMA1_Channel15);

  ATOMIC_SET_BIT(huart1.Instance->CR3, USART_CR3_DMAT);

  __HAL_UART_ENABLE(&huart1);

  HAL_ADC_Start(&hadc1);

  HAL_TIM_Base_Start(&htim15);
  /* USER CODE END 2 */
```

# Correct bug in generate by MX

<aerror>
There is an bug in linked_list generation for more nodes
</aerro>

In file `linked_list.c`

Change the 

```c-nc
  ret |= HAL_DMAEx_List_BuildNode(&pNodeConfig, &YourNodeName2);

  /* Insert YourNodeName2 to Queue */
  ret |= HAL_DMAEx_List_InsertNode_Tail(&YourQueueName, &YourNodeName2);
```

to

```c
  ret |= HAL_DMAEx_List_BuildNode(&pNodeConfig, &YourNodeName);

  /* Insert YourNodeName2 to Queue */
  ret |= HAL_DMAEx_List_InsertNode_Tail(&YourQueueName, &YourNodeName);
```

# Compile and run code

1. Compile project
2. Run project in debugger

# What we have

we start GPDMA by TIM event. Then GPDMA transfer data from ADC and sent them over UART

![adc dma uart tim](./img/adc_dma_uart_tim.json)
