# Network Working Group
**Category:** Informational  
**Institution:** Universidad de Carabobo  
**Date:** 13 Diciembre 2025  

**Authors:** 
1. Jeison Herrera V-26.840.542 
2. Brayan Ceballos V-29.569.937
3. Miguel Matute V-30.733.549
4. Gustavo Rivas V-30.988.485
5. Joshtin Mejías V-31.071.016

---

# IMPLEMENTACIÓN FÍSICA Y VERIFICACIÓN DE CABLEADO ETHERNET (UTP) Y ANÁLISIS DE FALLOS

## Status of This Memo

Este documento proporciona un informe técnico sobre la Práctica I del Laboratorio de Redes de Computadores I. Detalla los procedimientos de conectorización bajo estándares TIA/EIA-568, los resultados de las pruebas de continuidad y un análisis post-mortem de los fallos encontrados en la fabricación de cables de par trenzado.

## Abstract

Este informe documenta la construcción y validación de medios de transmisión guiados para redes Ethernet. Se abordó la fabricación de un cable directo (*straight-through*) bajo la norma T568B con resultados exitosos. Posteriormente, se intentó la fabricación de un cable cruzado (*crossover*), resultando en fallos críticos de capa física debido a errores en el ordenamiento de pares y la inserción en el conector RJ45.

---

## Table of Contents

1. [Introducción](#1-introducción)
2. [Materiales y Herramientas](#2-materiales-y-herramientas)
3. [Especificaciones del Estándar](#3-especificaciones-del-estándar)
4. [Metodología de Implementación](#4-metodología-de-implementación)
5. [Resultados de las Pruebas](#5-resultados-de-las-pruebas)
6. [Análisis de Errores](#6-análisis-de-errores)
7. [Conclusiones](#7-conclusiones)
8. [Consideraciones de Seguridad](#8-consideraciones-de-seguridad)

---

## 1. Introducción

El objetivo fundamental de la práctica realizada el 13 de diciembre de 2025 fue adquirir competencias prácticas en la manipulación de la Capa Física (Capa 1) del modelo OSI. La integridad de la señal en redes Ethernet depende directamente de la correcta terminación de los conductores de cobre en los conectores modulares.

Este documento valida la capacidad técnica para diferenciar y ensamblar cables directos (PC a Switch) y cables cruzados (PC a PC), siguiendo los estándares de cableado estructurado ANSI/TIA-568.

## 2. Materiales y Herramientas

Para la ejecución de la práctica se dispuso del siguiente hardware:

* **Medio de Transmisión:** Cable de par trenzado sin blindaje (UTP), Categoría 5e.
* **Terminaciones:** Conectores modulares RJ45 (interfaz 8P8C).
* **Herramientas Mecánicas:** Crimpadora (tenaza de engaste) con cuchilla de corte.
* **Instrumento de Medición:** Tester de cable de red (probador de continuidad y wiremap).

## 3. Especificaciones del Estándar

Se adoptó el esquema de cableado **T568B** como norma base para los extremos del cable directo. El pinout estipulado es:

| Pin | Color | Par | Función (10/100Base-T) |
| :---: | :--- | :---: | :--- |
| **1** | Blanco-Naranja | 2 | Tx+ |
| **2** | Naranja | 2 | Tx- |
| **3** | Blanco-Verde | 3 | Rx+ |
| **4** | Azul | 1 | No usado |
| **5** | Blanco-Azul | 1 | No usado |
| **6** | Verde | 3 | Rx- |
| **7** | Blanco-Marrón | 4 | No usado |
| **8** | Marrón | 4 | No usado |

## 4. Metodología de Implementación

El procedimiento de ensamblaje consistió en cuatro fases críticas:

1.  **Preparación:** Retiro de la chaqueta exterior (aprox. 2.5 cm) cuidando la integridad del aislamiento de los conductores.
2.  **Ordenamiento:** Destrenzado de pares y alineación de los hilos según el código de colores (planchado de los hilos con los dedos).
3.  **Inserción:** Corte perpendicular de los hilos (dejando 1.2 cm) e inserción a tope en el conector RJ45.
4.  **Crimpado:** Presión mecánica para desplazar los contactos dorados hacia los conductores.

## 5. Resultados de las Pruebas

Las pruebas de validación se realizaron utilizando un tester de continuidad básico (Master/Remote).

### 5.1. Caso A: Cable Directo (T568B - T568B)
* **Configuración:** Ambos extremos terminados bajo norma T568B.
* **Resultado del Tester:** Secuencia de luces 1-2-3-4-5-6-7-8 sincrónica.
* **Estado:** **PASS (ÉXITO)**.
* **Observación:** Continuidad eléctrica correcta y correspondencia 1:1.

<div style="text-align: center;">
  <img src="https://github.com/user-attachments/assets/189b9932-a928-42a5-9d91-4df1b46f8db8" width="250">
  <img src="https://github.com/user-attachments/assets/5f026477-18bf-445f-8137-a991fd09a374" width="250">
  <img src="https://github.com/user-attachments/assets/de94bd77-71a4-4163-badd-55562675355c" width="250">
</div>


### 5.2. Caso B: Cable Cruzado (Intento T568A - T568B)
* **Configuración:** Extremo A (T568A) / Extremo B (T568B).
* **Resultado del Tester:** Sin señal coherente.
* **Estado:** **FAIL (FALLO)**.
* **Observación:** Falla severa en la conectividad física.

## 6. Análisis de Errores

Se realizó una inspección visual y lógica del cable cruzado fallido, identificando dos errores de procedimiento fatales:

### 6.1. Error de Secuencia (Miswire)
El tester no mostró el recorrido secuencial esperado.
> **Diagnóstico:** Los hilos se cruzaron dentro del conector justo antes del crimpado, perdiendo el orden del estándar cromático.

### 6.2. Inserción Invertida y Falso Contacto
Se observó visualmente que los hilos no llegaban al final del conector RJ45 en uno de los extremos.
> **Diagnóstico:** Al no hacer tope con el chasis plástico del conector, las cuchillas de contacto (pines dorados) bajaron sobre el vacío en lugar de morder el cobre, resultando en un circuito abierto (Open). Adicionalmente, la inserción se realizó con el conector invertido (pestaña arriba), invirtiendo la polaridad de todo el esquema (Pin 1 físico conectado al cable marrón en lugar del blanco-naranja/verde).

## 7. Conclusiones

1.  **Certificación Visual:** La verificación visual del orden de colores y la profundidad de inserción *antes* de usar la crimpadora es el paso más crítico del proceso; el error humano es irreversible una vez realizado el engaste.
2.  **Integridad Mecánica:** Un código de colores correcto es inútil si la chaqueta del cable no queda mordida por la traba de seguridad del conector, ya que esto provoca fatiga mecánica y desconexiones futuras.
3.  **Resultado Académico:** Se logró construir exitosamente un cable directo funcional. Se identificaron empíricamente las consecuencias de una mala terminación en el cable cruzado, reforzando el aprendizaje mediante el análisis del error.

## 8. Consideraciones de Seguridad

* Se recomienda precaución extrema con la cuchilla de la crimpadora para evitar cortes. Varios miembros del equipo sufrieron de cortaduras leves al momento de manipulación y retiro de la chaqueta.
* Los residuos de cobre cortados deben desecharse en contenedores apropiados, ya que son conductores y pueden dañar equipos electrónicos si caen dentro de ellos.
* Se recomienda equipamiento de protección para prevenir accidentes.
* Es idóneo mantener el área de trabajo limpia e ir desechando los equipos que ya sean útiles para evitar confusiones que interrumpan el trabajo.

---

**Universidad de Carabobo - Facultad Experimental de Ciencias y Tecnología**

