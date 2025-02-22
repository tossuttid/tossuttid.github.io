---
title: "DEZ80 Nivel1 Práctica"
date: 2025-02-17
draft: false
description: "a description"
series: ["dez80"]
series_order: 2
---

## 001, 002

**EJEMPLO:** Introducción al curso, Instalación de WinApe.

## 003

**EJEMPLO:** Pintar el primer pixel de color rojo.

![003](003.jpg)

## 004, 005

**PRÁCTICA:** Jugando con el código, Ejecutando paso a paso

## 006

**RETO:** Pintar primer pixel de varios colores.

Rojo:

Cyan:

Etc...

## Pintar de dos en dos

*007; ejemplo*

Hasta ahora hicimos programas que agarran un byte, lo meten en el procesador, y de ahí lo llevan a la memoria.

Veremos dos instrucciones que permiten mover de dos en dos datos (21 y 22).

![](/img/dez80/1-nivel/p-007-pixeles.jpg)

### Tracing

| Memoria | Ensamblador | Descripción |
| --- | --- | --- |
| 21 08 88 | LD HL, 8808 | Carga el par de registros HL con el valor 8808 |
| 22 00 C0 | LD (C000), HL | Almacena el valor del par de registros HL en la dirección de memoria C000 |

#### LD HL, 8808h (21 08 88):

1. **Fetch (Obtener):**

- El contador de programa (PC) contiene la dirección de la primera instrucción (4000).
- La CPU lee el byte 21 de la dirección 4000 y lo coloca en el registro de instrucciones (IR).
- El PC se incrementa a 4000+1.

2. **Decode (Decodificar):**

- La CPU decodifica el byte 21 como la instrucción LD HL, nn donde nn es un valor de 16 bits que se obtendrá a continuación. Reconoce que necesita dos bytes más para completar la instrucción.

3. **Execute (Ejecutar):**

- La CPU lee el byte 08 de la dirección 4000+1 y lo coloca en el registro L.

- El PC se incrementa a 4000+2.

- La CPU lee el byte 88 de la dirección 4000+2 y lo coloca en el registro H.

- El PC se incrementa a 4000+3, listo para la siguiente instrucción.

- El par de registros HL ahora contiene el valor 8808.

**Ciclo Fetch-Decode-Execute para LD (C000h), HL (22 00 C0):**

1. **Fetch (Obtener):**

- El contador de programa (PC) ahora contiene la dirección 4000+3.
	
- La CPU lee el byte 22 de la dirección 4000+3 y lo coloca en el registro de instrucciones (IR).
	
- El PC se incrementa a 4000+4.
	
2. **Decode (Decodificar):**

- La CPU decodifica el byte 22 como la instrucción LD (nn), HL donde nn es una dirección de 16 bits que se obtendrá a continuación. Reconoce que necesita dos bytes más.
	
3. **Execute (Ejecutar):**

- La CPU lee el byte 00 de la dirección 4000+4 y lo usa como el byte bajo de la dirección de destino.
- El PC se incrementa a 4000+5.
- La CPU lee el byte C0 de la dirección 4000+5 y lo usa como el byte alto de la dirección de destino.
- El PC se incrementa a 4000+6.
- La CPU copia el valor del registro L (el byte bajo de HL) a la dirección de memoria C000.
- La CPU copia el valor del registro H (el byte alto de HL) a la dirección de memoria C001.

## Varios patrones de colores en la primera línea

*008; reto*

### Sub-reto 1

```
4000 3E FF 32 00 C0 18 FE
```

### Sub-reto 2

- 24 píxeles
- En grupos de 4 del mismo color
- Usar colores rojo, cyan y amarillo
- Bonus por usar la instrucción 21

![](/img/dez80/1-nivel/008-p2.jpg)

![](/img/dez80/1-nivel/Pasted%20image%2020250129201210.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250129201538.png)

```
C000 F0 FF 0F FF F0 FF
```

### Sub-reto 3

- 12 píxeles
- En grupos de 2
- Colores rojo, cyan, amarillo
- Bonus por usar 3E y 21

![](/img/dez80/1-nivel/Pasted%20image%2020250129230651.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250129230634.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250129230741.png)

```
C000 F3 3F F3
```

### Sub-reto 4

- 8 píxeles
- Cada píxel de distinto color

![](/img/dez80/1-nivel/Pasted%20image%2020250129235536.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250129235635.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250129235714.png)

rojo, cyan, amarillo, cyan rojo, cyan, rojo, amarillo

```
1010 1101 1011 1110

C000 AD BE
```

### Sub-reto 5

- 80 píxeles o más
- Dibujo libre
- Usar todos los colores

![](/img/dez80/1-nivel/Pasted%20image%2020250130021017.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250130021318.png)

![](/img/dez80/1-nivel/Pasted%20image%2020250130021538.png)

![](/img/dez80/1-nivel/finalboss-008.svg)

```
C000 38 77 07 38 E0 FF CD 0F 1C F0 E0 FF FF 8B 0F 0F

C010 70 F0 F0 55
```

## Pintando por la pantalla

*009; ejemplo* 

Pantalla tiene un montón de posiciones
- FFFF (65535) - C000 (49152) = 16384 bytes (16k)
- Ancho c/fila pantalla: 320 px "80 dec grupos de 4" =  80 posiciones dec
    - fila termina 80 dec posiciones adelante en memoria
- 80 dec = `5*16` = 50 hex

![Cálculo dimensiones](/img/dez80/1-nivel/practica-y-desafios/calculo-dimensiones.png)

![Contextualización Mapa de Memoria](/img/dez80/1-nivel/practica-y-desafios/contexto-mapa-memoria.png)

## Varios patrones de colores en varios lugares de la pantalla

*010; reto*

### Sub-reto 1

- 1 píxel rojo
- En cualquier lugar
- Elegir lugar

![Resultado](/img/dez80/1-nivel/practica-y-desafios/10/sub-1.png)

```
3E 11 32 71 D1 18 FE
```

### Sub-reto 2

- 3 píxeles en la mísma fila
- debajo de 3 letras
- distintos colores


![Resultado](/img/dez80/1-nivel/practica-y-desafios/10/sub-2.png)

```
3E 11 32 A2 C8 3E 10 32 A4 C8 3E 01 32 A6 C8 18 FE
```

### Sub-reto 3

- 5 píxeles en la mísma columna
- forma dibujo de colores
- cualquier lugar



### Sub-reto 4

minidibujo

- al menos 3 columnas
- al menos 4 filas
- colores y pixeles que quieras

