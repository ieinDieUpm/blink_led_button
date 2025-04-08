# Blink LED con espera activa de pulsación de botón

Este proyecto hace parpadear el LED LD2 de la Nucleo-STM32F446RE a una frecuencia de `c` Hz. La frecuencia es controlada por el pulsado del botón de usuario B1. Cada vez que se pulsa el botón, la frecuencia (`c`) aumenta en 1. **El LED está apagado cuando `c` es igual a 0**. La lectura del botón se realiza por espera activa o *polling*.

Cuando se pulse el botón, el LED parpadea a la frecuencia indicada por el contador `c`. El tiempo de encendido y apagado del LED es igual a 1/(2*c) segundos. Por ejemplo, si `c` es 1, el LED parpadea a 1 Hz (500 ms encendido y 500 ms apagado). Si `c` es 2, el LED parpadea a 2 Hz (250 ms encendido y 250 ms apagado), etc.

¿A qué frecuencia deja de notar el parpadeo del LED?

## Ejercicio: parpadeo con botón

1. Haga una copia del proyecto [blink_led](https://github.com/ieinDieUpm/blink_led/tree/hal_version) y renómbrelo a `blink_led_button`.

2. Cree los ficheros `port_button.h` y `stm32f4_button.c` en las correspondientes carpetas de la parte portable del proyecto.

3. Cree en `stm32f4_button.c` la función de inicialización `port_button_gpio_setup()` que configure una GPIO como entrada **haciendo uso de la HAL**, de forma similar a como se ha inicializado el LED en `port_led_gpio_setup()`.

    Tenga en cuenta que el botón de usuario B1 está conectado a la GPIO PC13 (GPIOC pin 13) del microcontrolador. El botón **es una entrada**, no una salida como LED.

4. Ponga en `port_button.h` los prototipos de las funciones públicas.

5. En el fichero `main.c`, incluya el fichero de cabecera `port_button.h` y llame a la función `port_button_gpio_setup()` desde la función `main()`. 

6. Haga *polling* para leer el valor del botón usando `HAL_GPIO_ReadPin()`. Para ello llame a esta función dentro de otra: `port_button_get_status()` que devuelva un booleano con el estado del botón (0, no pulsado; o 1, pulsado). La función `port_button_get_status()` debe estar en `stm32f4_button.c`, y debe hacerse pública en `port_button.h`.

7. Tenga en cuenta que para aumentar el contador `c` el botón debe haberse pulsado (el botón baja a 0 V) y soltado (sube a 3.3V) **ambas condiciones**. Recuerde que el LED está apagado cuando `c` es 0.

## References

- **[1]**: [Fundamentos teóricos de sistemas basados en microcontrolador STM32. Sistemas Digitales II, Sistemas Electrónicos](https://oa.upm.es/88460)
- **[2]**: [Embedded Systems with ARM Cortex-M Microcontrollers in Assembly Language and C (Fourth Edition)](https://web.eece.maine.edu/~zhu/book/index.php) for explanations and examples of use of the ARM Cortex-M microcontrollers in C with CMSIS.
- **[3]**: [Programming with STM32: Getting Started with the Nucleo Board and C/C++](https://ingenio.upm.es/primo-explore/fulldisplay?docid=34UPM_ALMA51126621660004212&context=L&vid=34UPM_VU1&lang=es_ES&search_scope=TAB1_SCOPE1&adaptor=Local%20Search%20Engine&tab=tab1&query=any,contains,Programming%20with%20STM32:%20Getting%20Started%20with%20the%20Nucleo%20Board%20and%20C%2FC%2B%2B&offset=0) for examples of use of the STM32 microcontrollers with the HAL of ST.
- **[4]**: [The C Programming Language](https://ingenio.upm.es/primo-explore/fulldisplay?docid=34UPM_ALMA2151866130004212&context=L&vid=34UPM_VU1&lang=es_ES&search_scope=TAB1_SCOPE1&adaptor=Local%20Search%20Engine&isFrbr=true&tab=tab1&query=any,contains,C%20Programming%20Language)
- **[5]**: [Nucleo Boards Programming with th STM32CubeIDE](https://www.elektor.com/products/nucleo-boards-programming-with-the-stm32cubeide) for examples of use of the STM32 microcontrollers with the STM32CubeIDE.
