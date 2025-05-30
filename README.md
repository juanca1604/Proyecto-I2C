# Proyecto-I2C
Proyecto I2C: Configuración y elementos del módulo I2C en un procesador RISC-V.
## Objetivo
El módulo desarrollado implementa un controlador I2C con soporte para operación en modo maestro y esclavo, enfocado en realizar operaciones de escritura. Este controlador genera las señales requeridas para la transmisión de datos hacia un periférico conectado mediante el bus I2C, incluyendo el manejo de las condiciones de inicio, parada y control de respuestas 
## Configuración
### Especificaciones
- Frecuencia de operación : 100 kHz (configurable).
- Modo de operación : Maestro/Esclavo.
- Direcciones soportadas : 7 bits.
- Funciones :
  - Generación de condiciones Start/Stop.
  - Transmisión de datos al periférico.
  - Manejo de señales ACK/NACK.
  - Generación de interrupciones al completar la escritura.
  - Configuración de preescalador para control del reloj.
### Configuración de pines
| Alfiler     | Dirección   | Descripción           |
| ----------- | ----------- |---------------------- |
| SCL         | Producción  | Línea de reloj serial.|
| Reiniciar   | Aporte      | Señal de reinicio del controlador.  |
| INT	        | Producción	| Señal de interrupción de escritura. |
| CLK	        | Aporte	    | Reloj del sistema.                |
| VDD	        | Fuerza	    | Alimentación positiva.            |
| Tierra	    | Fuerza	    | Conexión a tierra.                |
## Cobertura de la especificación
- El diseño implementado cubre las siguientes funcionalidades:
- Operación básica en modo Maestro para escritura.
- Generación de condiciones de inicio (Start) y parada (Stop).
- Transmisión de datos hacia el periférico.
- Gestión de señales ACK/NACK provenientes del periférico.
## Elementos pendientes por implementar
- Soporte para múltiples periféricos operando en simultáneo.
- Mecanismo de gestión mediante interrupciones.
## Diagramas de tiempo
![image](https://github.com/user-attachments/assets/8a008205-6fdc-42e2-93a9-01f41d6d7a00)
