---
layout: post
title: Reparación de Móviles
date: 2024-03-28 18:11:23
category: sample
vm: movil
---
## 1. Herramientas
### 1.1. Herramientas de diagnóstico

- Multímetro o tester ()
- Fuente de alimentación ()
- usbTester ()
- Cámara térmica ()
	- Localizador de fallas o Removedor de partículas 
- Microscopio ()
	- Lupa

###  1.2. Herramientas de desarme y armado

- Destornilladores de precisión ()
- Púas plásticas y espátulas de desarme ()
- Plancha separadora o de calor ()
	- Pistola de calor ()
- Ventosa ()

### 1.3. Herramientas para soldadura

- Estación de soldadura ()
	- Cautín o soldador ()
- Estaño ()
- Flux ()
- Malla desoldante ()
	- Succionador de estaño ()
- Cinta metálica y/o Kapton ()
- Pinzas bruselas
- Holder ()

### 1.4. Herramientas de limpieza

- Batea de ultrasonido ()
	- Alcohol isopropílico ()
	- WD-40 ()
	- Bencina
- Removedor de flux ()
- Cepillo y/o Hisopos

### 1.5. Herramientas de seguridad anti-estática

- Pulsera antiestática ()
	- Guantes antiestáticos ()
- Manta de trabajo antiestática

### 1.6. Herramientas complementarias

- Alicate de precisión ()
- Pegamento para pantallas ()
- Pinzas sujetadoras y/o morsas ()
- Recipientes herméticos y/o táper ()

---

## 2. Conceptos de electrónica básica

### 2.1. Tensión, Corriente y Resistencia

 _* Tensión o Voltage_

	unidad de medida: V (voltio)
	Fuerza que se ejerce sobre los electrones para que se muevan.
	Mayor fuerza mas voltage

	Bateria
	+V +A

 _* Corriente_

	Cantidad de electrones que estan circulando en un circuito.
	unidad de medida: A (ampere)

 _* Resistencia_

	Opocision al paso de la corriente o electrones
	unidad de medida: Ω (ohmio)

	+R -A

### 2.2. Corriente Alterna y Corriente Continua

Corriente alterna **AC** (~)

	El curso de los electrones cambia de sentido todo el tiempo.
	No tiene polaridad

Corriente continua **DC/CC** (-)

	Los electrones siempre van en una misma direccion.
	Tiene polaridad

### 2.3. Multiplicadores y Divisores

|prefijo |Simbolo |Valor |Equivalencia en unidades|
|-------|---------|------|------------------------|
|exa    |E        |1x10(18)|trillón               |
|peta   |P        |1x10(15)|mil billones          |
|tera   |T        |1x10(12)|billón                |
|giga   |G        |1x10(9) |mil millones          |
|mega   |M        |1x10(6) |millón                |
|**kilo**   |k        |1x10(3) |mil                   |
|hecto  |h        |1x10(2) |cien                  |
|deca   |da       |1x10    |diez                  |
|**unidad** |**1**        |**1**       |**uno**                   |
|deci   |d        |1x10(-1)|décima                |
|centi  |c        |1x10(-2)|centésima             |
|**mili**   |m        |1x10(-3)|milésima              |
|       |         |1x10(-6)|millonésima           |
|       |         |1x10(-9)|mil millonésima       |

_Ejemplo:_

	1A = 1 mA (miliAmpere)
	
	0,550 -> 550 mA
	1,25  -> 1250 mA
	
### 2.4. Serie y Paralelo

#### Serie
Uno al lado del otro

_Ejemplo:_

	Tenemos 3 leds cada uno necesita 2v y consumen 3mA
		-> Voltage necesario para encenderlos: 
			2v+2v+2v = 6V    se suman
			3mA
	Potencia usado 6V*3mA=18

#### Paralelo
	cada conexión es independiente

_Ejemplo:_

	Tenemos 3 leds cada uno necesita 2V y consumen 3mA
		-> voltage necesario para encenderlos:
			2V
			3mA+3mA+3mA = 9mA   se suman
	Potencia usado 2V*9mA=18

##### Mediciones comunes en móviles
- voltages V
- ohmios Ω
- continuidad _
- diodos -▶\|-

### 2.5. Cortocircuitos

#### Circuito abierto
	Cuando el circuito esta abierto no funciona  Ejp:un cable cortado

#### Circuito cerrado
	Cuando el circuito esta cerrado si funciona

#### Cortocircuito
	Ocurre cuando Positivo y Negativo se unen sin ninguna resistencia en el medio.
	Ocurre una avalancha de electrones en ese punto que producen una disipacion colorica mayor.

### 2.6 Utilización del Tester

	⏚  Cable Negro (COM)
	Ω  Cable Rojo  (mA)

**Uno siempre debe elegir la escala inmediatamente superior a la que quiere medir**

	Medicion de tension en las Baterias.
	cargadas al 100% entregan 4.2V  4.3V
	En el tester elegir la escale superior a ello ej 20V

---

## 3. Componentes electrónicos

#### Tipos de componentes electrones según su soldadura
##### THT (tecnología de agujeros pasantes)
	Los contactos de los componentes estan soldados pasando la placa

##### SMT o SMD (Tecnología de montaje superficial)
	Tienen contactos para ser soldados de manera superficial sobre la placa

### 3.1. Resistencias R

	Componentes que se oponen al paso de la corriente.
	
	No tienen polaridad
	No continuidad
	En general los smd son de color negro con extremos plateados.

_* Medición de resistencia_

- Colocamos la perilla del tester en la parte del simbolo Ω
- Colocamos cada punta en cada extremo del resistor.
- Probar con la escala menor _(si marca 1 o 0 ir subiendo de escala)_
	- si ya llegamos a las escalas de k, multiplicar el valor que nos da por mil

### 3.2. Condensadores C

	Son como pequeñas baterias, capaces de almacenar cierta energia.
	Logra estabilizar la linea ante fluctuaciones. Absorve y/o suelta energia.
	casi todos los capacitores estan conectadas en paralelo.(Contacto en + el otro en -).
	
	No continuidad.
	No tiene polaridad
	En general los capacitores son color Marron(Mas claro o Oscuro) y/o grises.

_* Medición de capacitores_
- Colocamos el Tester en escala de Continuidad.
- Colocamos cada punta en cada extremos del capacitor.
	- No debe de haber continuidad (Sonido).
- Otra Forma de medir es colocar cualquier punta a tierra (Chapa).
	- Con la otra punta medimos cada extremo del capacitor.
		- En un extremo debe de haber continuidad(Sonido/Conexión a tierra) y en la otra no.

### 3.3. Bobinas L

	Cable enrollado en un nucleo aislado entre si.
	Capaz de cargarse pero con muy poca energia(electromagnetico).
		-A falta de corriente es capaz de elevar la tension.
		-A excesivo corriente es capaz de bajar la tension.
	Por lo general usados en serie y en linea positivo.
	
	A diferencia de capacitores o resistencia; normalmente son mas grandesitas.
	Si continuidad.

_próxima actualización de contenido ...._