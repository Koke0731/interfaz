# Acceso a periféricos por memoria mapeada (MMIO) y el calificador `volatile`.

## INTRODUCCION
Cuando hablamos de entrada y salida mapeada en memoria o por sus siglas "MMIO", entendemos que es una técnica que utilizamos para permitir 
la comunicación entre el procesador y los dispositivos de hardware, esto mediante direcciones de memoria específicos los cuales se verán
mas adelante en esta investigación. En este método en cuestión ciertos rangos del espacio de direcciones físicas se reservan para los para
los dispositivos, a su vez esto permite que el CPU interactúe con ellos utilizando operaciones normales de lectura y escritura.

Dentro de este contexto, el lenguaje C proporciona herramientas importantes para trabajar directamente con hardware, entre ellas el
calificador `volatile`. Este calificador le indica al compilador que valor de una variable puede cambiar de manera inesperada, por ejemplo,
debido a la acción de un dispositivo externo. `volatile` es especialmente relevante al trabajar con registros de hardware mediante MMIO, ya que
ayuda a garantizar que cada acceso realizado por el programa se efectué realmente sobre el dispositivo y no sea eliminado u optimizado
incorrectamente por el compilador

## DESARROLLO
## 1. Comunicación entre el procesador y los periféricos
## 1.1 Regiones de MMIO y mapas de memoria
Las regiones MMIO (Memory-Mapped I/O) ocupan rangos de direcciones físicas fijas establecidas por el diseño del SoC (System on a Chip).
Durante el proceso de arranque, el kernel de Linux descubre la ubicación y el tamaño de estas regiones consultando el Árbol de Dispositivos (Device Tree, o DT).
Una vez identificadas, Linux reserva estas áreas y crea mapeos de memoria virtual, habitualmente utilizando la función ioremap(). Es crucial
que estas asignaciones se configuren con atributos de memoria de "tipo de dispositivo", esto asegura que se desactive la memoria caché
se evite la reordenación de instrucciones. De esta manera, se garantiza que el procesador lea y escriba los datos interactuando directamente con los registros del periférico 
físico en tiempo real.

### Mapa de direcciones físicas del SoC
| Rango de direcciones | Región |
|---|---|
| `0x0000_0000 - 0x0FFF_FFFF` | DDR |
| `0x1000_0000 - 0x1000_0FFF` | UART MMIO |
| `0x1234_0000 - 0x1234_0FFF` | Device MMIO |
| `...` | ... |

### 1.2 Traducción de direcciones virtuales a físicas
La CPU siempre emite direcciones virtuales (VA). Antes de que las transacciones lleguen a la interconexión, la unidad de gestión de Memoria (MMU) traduce las VA
a direcciones físicas (PA). La MMU utiliza un búfer de traducción anticipada (TLB) para las traducciones almacenadas en caché y realiza un recorrido de la tabla de 
páginas en caso de fallos en el TLB

![Traduccion de VA-PA](https://media2-dev-to.translate.goog/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Feutqp5b9kxb4epqw21cf.png?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc)
## 2. E/S mapeada en memoria (MMIO)
La MMIO representa un avance notable en la forma en que los dispositivos de E/S se integran en el espacio de direcciones de memoria, básicamente remodelando la interacción entre estos dispositivos y la CPU.
### 2.1 Espacio y mapa de direcciones
### 2.2 Registros de los periféricos
### 2.3 Lecturas y escrituras MMIO

## 3. MMIO frente a Port-Mapped I/O

## 4. Acceso a MMIO desde C

## 5. El calificador `volatile`

### 5.1 Funcionamiento de `volatile`
### 5.2 Optimizaciones del compilador
### 5.3 Uso de `volatile` con MMIO

## 6. Limitaciones de `volatile`

### 6.1 Atomicidad
### 6.2 Sincronización
### 6.3 Barreras de memoria

## 8. Aplicaciones

## 9. Relación con Lenguajes de Interfaz

## Conclusiones

## Referencias
