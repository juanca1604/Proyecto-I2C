# Proyecto-I2C
Proyecto I2C: Configuración y elementos del módulo I2C en un procesador RISC-V.
# Proyecto de módulo de escritura I2C
## Objetivo
El módulo desarrollado implementa un controlador I2C con soporte para operación en modo maestro y esclavo, enfocado en realizar operaciones de escritura. Este controlador genera las señales requeridas para la transmisión de datos hacia un periférico conectado mediante el bus I2C, incluyendo el manejo de las condiciones de inicio, parada y control de respuestas 
## Configuración
- Frecuencia de operación: 100 kHz.
- Modo de operación: Maestro/Esclavo.
- Direcciones soportadas: 7 bits.
### Función
- Generación de condiciones Start/Stop.
- Transmisión de datos al periférico.
- Manejo de señales ACK/NACK.
- Generación de interrupciones al completar la escritura.
- Configuración de prescaler para control del reloj.
## Especificaciones
### Especificaciones implementadas
- El diseño implementado cubre las siguientes funcionalidades:
- Operación básica en modo Maestro para escritura.
- Generación de condiciones de inicio (Start) y parada (Stop).
- Transmisión de datos hacia el periférico.
- Gestión de señales ACK/NACK provenientes del periférico.
### Especificaciones pendientes 
- Soporte para múltiples periféricos operando en simultáneo.
- Mecanismo de gestión mediante interrupciones.

## Configuración de pines
| Alfiler     | Dirección   | Descripción           |
| ----------- | ----------- |---------------------- |
| SCL         | Producción  | Línea de reloj serial.|
| Reiniciar   | Aporte      | Señal de reinicio del controlador.  |
| INT	        | Producción	| Señal de interrupción de escritura. |
| CLK	        | Aporte	    | Reloj del sistema.                |
| VDD	        | Fuerza	    | Alimentación positiva.            |
| Tierra	    | Fuerza	    | Conexión a tierra.                |

## Diagramas de tiempo
![Imagen de WhatsApp 2025-05-30 a las 21 11 34_50f9b603](https://github.com/user-attachments/assets/4f54513b-4149-4059-821e-b0a11729304d)
El diagrama de tiempo corresponde a una simulación realizada con el banco de pruebas del módulo I2C durante una operación de escritura. En esta simulación se observa el comportamiento de las señales involucradas en la transferencia de datos desde el controlador I2C hacia un periférico conectado al bus. La secuencia inicia con la señal addr[6:0], que especifica la dirección del dispositivo esclavo al que el maestro intenta acceder; en este caso, la dirección configurada es 0x55. La señal de reloj del sistema (clk) permanece activa durante toda la operación, sincronizando las transiciones del resto de señales. Asimismo, la señal enable se encuentra habilitada, indicando que el módulo está operativo.

En la etapa de configuración, el registro i2c_bitrate[31:0] define la frecuencia de operación del bus I2C. El valor establecido, 0x0007A120, corresponde a una frecuencia cercana a los 100 kHz, en cumplimiento con los parámetros estándar del protocolo. Los registros i2c_ctrl_reg[7:0] e i2c_ctrl_wire[7:0] gestionan acciones clave como el inicio de la comunicación y las condiciones de parada. Durante la simulación, estos registros varían entre los valores 0x50 y 0x48, lo que refleja instrucciones específicas que controlan el flujo de la escritura.

En cuanto a la transferencia de datos, el registro i2c_data_out[7:0] presenta inicialmente el valor 0xAA, seguido de 0x55, lo que demuestra la transmisión secuencial hacia el periférico. Las señales i2c_scl (reloj serial) e i2c_sda (datos) muestran una actividad sincronizada conforme al protocolo I2C, donde i2c_scl genera los pulsos necesarios y i2c_sda refleja los cambios correspondientes al envío de cada bit. Esto confirma que la lógica de transmisión de datos básicos se comporta de forma adecuada.

Por otro lado, se observa la interacción con la memoria a través de señales como mem_addr[31:0], mem_wdata[31:0], mem_ready, mem_valid y mem_wstrb. La dirección de memoria cambia a 0x0000001F, mientras que los datos escritos (mem_wdata) coinciden con los enviados al bus (0x00000050 y 0x00000048). Las señales mem_ready y mem_valid sincronizan la escritura, y mem_wstrb, al activarse con valor F, habilita la escritura en memoria. La señal rst permanece inactiva durante todo el proceso, garantizando estabilidad del sistema.

Finalmente, el análisis del diagrama revela algunas oportunidades de mejora. Aunque las señales principales funcionan correctamente, se detectan posibles desincronizaciones entre mem_ready y mem_valid, lo que podría generar errores bajo ciertas condiciones. El diseño aún no ha sido validado con múltiples dispositivos esclavos ni con velocidades superiores como 400 kHz (Fast Mode). Tampoco se ha implementado un mecanismo para verificar la integridad de los datos escritos, como una lectura de confirmación. Además, es necesario evaluar el comportamiento ante respuestas NACK del periférico y realizar pruebas con bloques de datos más extensos. Por último, no se han analizado los tiempos de configuración y retención del bus, lo que podría comprometer la conformidad con el estándar I2C en escenarios críticos. Estas observaciones subrayan la necesidad de una fase de validación más rigurosa.

# Proyecto de módulo de lectura I2C
## Objetivo
El módulo desarrollado integra un controlador I2C completamente funcional, capaz de operar tanto en modo Maestro como en modo Periférico. Soporta direcciones de 7 bits y cuenta con funcionalidades esenciales como la gestión de señales ACK/NACK y la detección de condiciones de inicio (Start) y parada (Stop).
## Configuración
- Especificaciones del módulo
- Frecuencia de operación: 100 kHz (configurable).
- Modo de operación: Maestro/periferico.
- Direcciones soportadas: 7 bits.
### Función
- Detección de condiciones Start/Stop.
- Manejo de ACK/NACK.
- Configuración del prescaler.
## Especificaciones 
### Especificaciones implementadas
- Operación en modo Maestro y Periférico.
- Configuración y manejo de direcciones I2C de 7 bits.
- Soporte para generación y manejo de interrupciones.
- Detección de condiciones de inicio (Start) y parada (Stop).
### Especificaciones pendientes
- Implementación del direccionamiento extendido de 10 bits.
- Soporte para múltiples periféricos operando simultáneamente en el bus.
- Optimización del tiempo de respuesta del controlador.
## Configuración de pines
| Alfiler     | Dirección   | Descripción           |
| ----------- | ----------- |---------------------- |
| SCL	        | Aporte	    | Línea de reloj serial. |
| REINICIAR	  | Aporte	    | Señal de reinicio del controlador. |
| INT	        | Producción	| Señal de interrupción. |
| CLK	        | Aporte	    | Reloj del sistema. |
| VDD	        | Fuerza	    | Alimentación positiva. |
| Tierra	    | Fuerza	    | Conexión a tierra. |
## Diagrama de tiempo
![Imagen de WhatsApp 2025-05-30 a las 21 16 04_e59236cd](https://github.com/user-attachments/assets/4591514a-e902-4775-b5bb-3c170f955a34)
El diagrama de tiempo presentado corresponde a una prueba de lectura del módulo I2C, cuyo objetivo es evaluar su capacidad de comunicación con un dispositivo periférico y una memoria asociada. La dirección del periférico está configurada en 0x54, según se observa en la señal addr[6:0], y la señal de reloj (clk) sincroniza todas las operaciones. La activación de la señal enable indica que el módulo está operativo. Estos parámetros iniciales permiten iniciar el proceso de lectura. El controlador se configura mediante los registros i2c_bitrate[31:0], que definen la frecuencia del bus, y i2c_ctrl_reg[7:0] junto con i2c_ctrl_wire[7:0], los cuales están ajustados para una operación de lectura activa, representada por el valor 0x50.

Durante la simulación, la señal i2c_data_out[7:0] envía el valor 0xA9 al periférico, lo que indica un intento de lectura. Sin embargo, la señal i2c_data_in[7:0] permanece en 0x00, lo que sugiere que no se reciben datos válidos de retorno. A pesar de que las señales i2c_scl (reloj del bus) e i2c_sda (datos del bus) muestran actividad, la falta de respuesta en i2c_sda indica una posible falla en la comunicación con el periférico. Este comportamiento podría deberse a que el dispositivo esclavo no reconoce la dirección configurada o no responde correctamente a las señales del maestro. La ausencia de datos también podría estar vinculada a una mala implementación del protocolo por parte del periférico.

En cuanto a la interacción con la memoria, las señales mem_addr[31:0] y mem_rdata[31:0] muestran intentos de lectura en distintas direcciones, pero el valor de mem_rdata se mantiene en 0x00000000, lo que indica que no se obtiene información válida. Esto podría deberse a una desincronización entre las señales mem_ready y mem_valid, que deben estar correctamente alineadas para garantizar la transferencia de datos. Aunque el sistema se inicializa correctamente mediante la señal rst y se configura para lectura (rw = 0), el flujo de datos no se completa exitosamente. Es necesario revisar la configuración del periférico, validar que la dirección sea reconocida, y garantizar que las señales de control —especialmente las relacionadas con memoria— cumplan con las especificaciones del protocolo I2C.

## Diagrama de bloques
![Imagen de WhatsApp 2025-05-30 a las 21 40 21_1479bb6e](https://github.com/user-attachments/assets/cd0b40d6-b49f-4c6e-9504-1fadd5d04eed)
![Imagen de WhatsApp 2025-05-30 a las 21 39 12_0752a794](https://github.com/user-attachments/assets/e728ef5d-5400-43a7-a7e8-0e4326e284be)

El diagrama de bloques presentado muestra la arquitectura general del módulo I2C, destacando las secciones clave que hacen posible la implementación del protocolo de comunicación. En el centro del diseño se gestionan dos buses fundamentales: el bus de datos y el bus de direcciones. El primero, con señales como mem_wdata y mem_rdata, permite la transferencia de información hacia y desde los registros internos del módulo. El bus de direcciones (mem_addr), por su parte, se encarga de seleccionar los registros específicos sobre los cuales se realizarán operaciones de lectura o escritura, facilitando así la configuración del módulo por parte de un sistema externo.

Dentro del núcleo del sistema se encuentran los registros internos encargados de controlar el funcionamiento del protocolo. El registro I2C_BITRATE configura la frecuencia del bus mediante divisores de reloj, permitiendo establecer velocidades estándar como los 100 kHz. Los registros I2C_DATA_OUT e I2C_DATA_IN almacenan los datos enviados al periférico y los recibidos durante una lectura, respectivamente. Además, el registro I2C_CTRL administra señales esenciales de control como el inicio y final de transmisión (START/STOP), así como la selección del tipo de operación (lectura o escritura).

El diseño incluye también un controlador de reloj compuesto por un contador y un divisor, cuya función es transformar el reloj del sistema (CPU_CLOCK) en la señal de reloj del bus I2C (SCL), garantizando la sincronización de la comunicación. Este controlador trabaja junto con una máquina de estados secuencial que ejecuta la lógica del protocolo I2C, manejando condiciones de arranque y parada, generación de señales de habilitación (EN_I2C), control de la línea de datos (SDA), y respuesta a señales ACK/NACK. Finalmente, señales adicionales como mem_valid, mem_ready y mem_wstrb aseguran una correcta sincronización y control de las operaciones de lectura y escritura entre el módulo y el entorno externo.

## Máquina de estados
![image](https://github.com/user-attachments/assets/e2eaa503-3aef-4f41-95e1-066f9f5793f4)
La máquina de estados finita del módulo maestro I2C inicia su operación en el estado **IDLE**, donde las líneas SDA y SCL permanecen en nivel alto, indicando que el bus está libre. Cuando se activa la señal **I2C\_START**, el sistema genera una condición de inicio llevando la línea SDA a nivel bajo mientras SCL permanece alta. Esta condición notifica a los dispositivos conectados que una nueva comunicación está por comenzar, y el sistema avanza al estado **ADDRESS**, donde el maestro transmite la dirección de 7 bits del periférico objetivo, seguida de un bit que indica si se desea realizar una lectura o escritura.

Tras el envío de la dirección y el bit de control, el maestro ingresa al estado **ACK\_CHECK** para verificar si el periférico ha respondido con una señal de reconocimiento (ACK). Si se recibe el ACK, el maestro prosigue con la operación indicada: en caso de escritura (**R/W = 0**), entra al estado **WRITE** para enviar un byte de datos contenido en el registro `I2C_DATA_OUT` hacia el periférico. Al completar el envío, se genera una interrupción indicando que la transmisión fue exitosa, y el maestro puede continuar enviando más datos o finalizar la operación si el periférico responde con un NACK.

Si la operación es de lectura (**R/W = 1**), el maestro pasa al estado **READ**, donde recibe un byte de datos desde el periférico, el cual se almacena en el registro `I2C_DATA_IN`. Al igual que en la escritura, se genera una interrupción tras la recepción del dato. Si no se reciben más datos o el periférico emite un NACK, el maestro avanza al estado **STOP**, donde genera una condición de parada cambiando la línea SDA de bajo a alto mientras SCL permanece en alto. Esto indica que la comunicación ha finalizado, y el sistema regresa al estado **IDLE**, quedando listo para una nueva transacción.

## Sintesis I2C
![image](https://github.com/user-attachments/assets/7451a8e9-6cd0-458e-82e8-745b79852390)

La anterior imagen corresponde a el informe de síntesis que muestra el uso de recursos y el consumo de potencia del módulo I2C diseñado. En este reporte se evidencia que las celdas secuenciales, que incluyen registros y flip-flops, ocupan el 63.8% del área total del diseño y representan el 80.8% del consumo de potencia interna, lo cual es consistente con una arquitectura basada en máquinas de estados finitas utilizada para controlar las distintas fases del protocolo I2C (como START, ADDRESS, READ, WRITE y STOP). Las celdas lógicas, aunque ocupan un 30.8% del área, contribuyen con solo el 7.2% del consumo interno, lo que indica que la lógica combinacional no es el principal factor de consumo energético dentro del sistema.

## Layout final
![image](https://github.com/user-attachments/assets/4bd2eebf-f75e-47ae-abee-d41ea3124ce9)

La imagen presentada corresponde al diseño final en vista de celda colocada y ruteada (layout) del bloque digital del módulo I2C, obtenido tras seguir el flujo completo de síntesis. En este diseño, se puede observar la distribución física de las celdas estándar dentro del área del chip, donde cada componente lógico del módulo (como registros ) ha sido asignado a una ubicación específica, respetando las restricciones de área, potencia y temporización. Las líneas de interconexión en colores representan los diferentes niveles metálicos utilizados para enrutar las señales internas, lo cual garantiza la conectividad entre los elementos del circuito.



