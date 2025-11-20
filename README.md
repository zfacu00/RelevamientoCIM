# RelevamientoCIM![Logo Institucional](https://github.com/JonatanBogadoUNLZ/PPS-Jonatan-Bogado/blob/9952aac097aca83a1aadfc26679fc7ec57369d82/LOGO%20AZUL%20HORIZONTAL%20-%20fondo%20transparente.png)

# Universidad Nacional de Lomas de Zamora - Facultad de Ingeniería

## Ingeniería Mecatrónica  
### Informe de Práctica Profesional Supervisada

# Automatización de la Estación de Control de Calidad con Robot SCARA en el Laboratorio CIM

## Introducción / Objetivo

En la Universidad Nacional de Lomas de Zamora, nuestra Facultad de Ingeniería se dedica a la formación de profesionales en diversas ramas de la ingeniería. Este repositorio corresponde al proyecto de **automatización y robótica industrial**, desarrollado como parte de la **Práctica Profesional Supervisada** de la carrera de **Ingeniería Mecatrónica**.

La práctica se realizó en el **Laboratorio CIM** de la Facultad de Ingeniería, un entorno orientado a la automatización industrial y a los sistemas de manufactura flexible. En particular, el trabajo se desarrolló sobre la **estación de control de calidad**, equipada con un robot **SCARA**, un robot cartesiano, cinta transportadora, sensores y actuadores asociados.

El objetivo de este proyecto es **diseñar, ajustar y programar la estación de control de calidad**, implementando rutinas modulares (GET, PUT y subrutinas R1–R4) para la manipulación de bandejas y recipientes, la interacción con el surtidor de bolillas y la coordinación con el robot cartesiano, integrando señales digitales provenientes del tablero de control principal. Con ello se busca:
- Optimizar el espacio de trabajo y la disposición física de la estación.
- Aumentar la cantidad de posiciones operativas mediante bases porta-bandejas impresas en 3D.
- Estandarizar las rutinas de movimiento del robot SCARA mediante vectores de puntos.
- Integrar la estación en la línea automatizada del laboratorio CIM como etapa final de control de calidad.

---

## Índice

- [Descripción](#descripción)
- [Instrucciones-de-uso](#instrucciones-de-uso)
- [Tecnologías-utilizadas](#tecnologías-utilizadas)
- [Listado-de-componentes](#listado-de-componentes)
- [Esquemáticos](#esquemáticos)
- [Fotos--videos](#fotos--videos)
- [Autor](#autor)

---

## Descripción

Este proyecto se basa en la **automatización de la estación de control de calidad** del laboratorio CIM, utilizando un robot **SCARA** como manipulador principal y un **robot cartesiano** para el posicionamiento de las bandejas bajo una cámara de inspección.

El sistema reproduce la etapa final de una línea de manufactura flexible: las bandejas se trasladan sobre una cinta transportadora mediante carros numerados que activan sensores a su paso. Cuando un carro llega a la estación de control de calidad, se elevan pistones neumáticos y se envían señales digitales al controlador del SCARA, que ejecuta la rutina correspondiente (R1, R2, R3 o R4).

Entre las principales tareas realizadas se incluyen:

- **Adecuación del espacio de trabajo**: elevación de la mesa de trabajo para asegurar que el SCARA alcance correctamente la cinta sin exceder sus límites operativos.
- **Diseño e impresión 3D de nuevas bases porta-bandejas**: se pasó de 2 a 6 posiciones sobre la mesa, más una base adicional para el robot cartesiano.
- **Programación de rutinas GET y PUT**: funciones modulares para tomar y colocar bandejas, utilizando un vector de posiciones (CIM2[ ]) con puntos de seguridad, aproximación y posición final.
- **Implementación de subrutinas principales (R1–R4)**: cada una asociada a una señal de entrada distinta, controlando desde movimientos simples de inspección hasta la carga automática de bolillas en un recipiente.
- **Integración con el robot cartesiano**: coordinación para posicionar la bandeja en la zona de inspección bajo la cámara y devolverla a la cinta.

De esta forma, la estación queda integrada como **etapa de control de calidad automatizada** dentro del circuito de manufactura flexible del laboratorio CIM.

---

## Instrucciones de Uso

Para utilizar este proyecto en el entorno del Laboratorio CIM, se recomienda seguir estos pasos generales:

1. **Puesta en marcha del sistema**
   - Verificar el encendido del tablero de control principal.
   - Asegurar el suministro de aire comprimido para los pistones neumáticos.
   - Comprobar el correcto funcionamiento del sensor inductivo NPN y de la cinta transportadora.

2. **Inicialización de los manipuladores**
   - Llevar el robot SCARA a su posición de referencia (home) desde el teach pendant.
   - Inicializar el robot cartesiano en su posición de recepción de bandejas (TESTB[1]).
   - Verificar que las bases porta-bandejas estén correctamente fijadas y alineadas en la mesa de trabajo.

3. **Carga de programas**
   - Cargar y habilitar el programa principal `RUT`, que ejecuta en paralelo las subrutinas R1, R2, R3 y R4.
   - Verificar la disponibilidad de las rutinas GETxx y PUTxx asociadas a cada posición de CIM2[ ].
   - Configurar desde el tablero central qué carro (señal 9, 10, 11 o 12) activará la rutina correspondiente.

4. **Ejecución del ciclo**
   - Iniciar la cinta transportadora.
   - Cuando el carro active el sensor en la estación SCARA, se elevarán los pistones y se enviará la señal al robot.
   - El SCARA ejecutará la rutina correspondiente:
     - R1: ciclo de control de calidad sin uso de surtidor.
     - R2: intercambio de carros entre cinta y base lateral.
     - R3: carga de bolillas con cantidad definida manualmente.
     - R4: ciclo completo con carga automática de bolillas y coordinación con robot cartesiano.
   - Al finalizar la rutina, se envía una salida digital indicando fin de proceso y el carro regresa a la cinta.

5. **Parada y verificación**
   - Detener la cinta y bajar los pistones en caso de parada prolongada.
   - Comprobar que no queden bandejas o carros en posiciones intermedias.
   - Guardar cualquier modificación en los puntos del vector CIM2[ ] antes de apagar el sistema.

> **Nota**: Los detalles específicos de cada programa (código fuente completo, parámetros de velocidad y puntos grabados) se encuentran en la carpeta `CODIGO` de este repositorio.

---

## Tecnologías Utilizadas

Este proyecto fue desarrollado utilizando una variedad de tecnologías, incluyendo:

### Robótica
- Robot **SCARA** de 5 ejes para manipulación de bandejas y recipientes.
- **Robot cartesiano** de 2 ejes para posicionar la bandeja bajo la cámara.
- Sistema de **cinta transportadora** con carros numerados.
- Pistones neumáticos para elevación de carros en la estación de control de calidad.

### Electrónica
- **Sensor de proximidad inductivo NPN** para detección de carros en la cinta.
- **Surtidor de bolillas accionado por motor**.
- **Compresor de aire** con émbolo y pistón para actuadores neumáticos.
- Entradas y salidas digitales (IN[9–12], OUT[11]) desde el tablero de control principal.
- Cámara analógica (fuera de servicio en esta práctica, pero integrada en la estación).

### Programación
- Lenguaje de programación propietario del **robot SCARA** (rutinas `GET`, `PUT`, `R1–R4`, `RUT`).
- Uso de vectores de posiciones (`CIM2[ ]`, `TESTB[ ]`) grabados desde el **teach pendant**.
- Estructuras modulares con subrutinas (`GOSUB`) y manejo de señales (`WAIT`, `SET OUT`).

### Plataformas
- Entorno de programación del controlador del robot SCARA.
- Tablero de control principal del Laboratorio CIM para gestión de señales y secuencias de proceso.

### Inteligencia Artificial
- **No se utilizaron técnicas de Inteligencia Artificial** en este proyecto.  
  La lógica implementada se basa en programación secuencial y manejo de señales digitales.

---

## Listado de Componentes

- **Robot SCARA de 5 ejes**: manipulador principal de la estación de control de calidad.
- **Robot cartesiano de 2 ejes**: encargado de posicionar la bandeja debajo de la cámara.
- **Cinta transportadora con carros numerados**: medio de transporte de bandejas a lo largo de la línea.
- **Bases porta-bandejas impresas en 3D**: seis posiciones de trabajo sobre la mesa y una base adicional para el robot cartesiano.
- **Sensor de proximidad inductivo NPN**: detección de carros en la estación SCARA.
- **Surtidor de bolillas** accionado por motor: dispositivo de carga de bolillas en recipientes.
- **Compresor de aire y pistones neumáticos**: elevación de carros en la estación de control de calidad.
- **Cámara analógica** (actualmente fuera de funcionamiento): prevista para inspección visual.
- **Cuerpos prismáticos plásticos y recipientes**: piezas utilizadas en las rutinas de carga de bolillas y simulación de control de calidad.
- **Elementos de fijación**: para asegurar las piezas sobre las bandejas durante el proceso.

---

## Esquemáticos

A continuación se presentan (para ser subidos al repositorio) los esquemáticos y diagramas de diseño que explican cómo se ensamblan y operan los sistemas del proyecto:

- **Esquemático 1**: Diagrama eléctrico de entradas y salidas digitales entre el tablero de control principal y el controlador del robot SCARA.
- **Esquemático 2**: Disposición de las **bases porta-bandejas** sobre la mesa de trabajo y su relación con el vector de posiciones `CIM2[ ]`.
- **Esquemático 3**: Diagrama de flujo de las rutinas `GET`, `PUT` y subrutinas `R1–R4`, incluyendo el programa principal `RUT`.
- **Esquemático 4**: Interacción entre el robot SCARA, el robot cartesiano y la cinta transportadora durante el ciclo de control de calidad.

(En esta sección se podrán subir imágenes o enlaces a los archivos relevantes en la carpeta `PLANOS`.)

---

## Fotos / Videos

A modo de documentación visual del proyecto, se proponen las siguientes imágenes y videos:

- **Foto 1 – Estación SCARA**  
  Vista general de la estación de control de calidad, mostrando el robot SCARA, el robot cartesiano y la cinta transportadora.

- **Foto 2 – Nuevas bases impresas**  
  Detalle de las bases porta-bandejas impresas en 3D y su disposición sobre la mesa de trabajo.

- **Foto 3 – Posición del recipiente para R3 y R4**  
  Ubicación exacta del recipiente sobre la bandeja, necesaria para la correcta ejecución de las rutinas de carga de bolillas.

- **Video 1 – Ciclo de la rutina R4**  
  Secuencia completa del proceso: toma de bandeja, transporte del recipiente al surtidor, carga de bolillas, retorno del recipiente y coordinación con el robot cartesiano.

(Los enlaces a las imágenes o videos se alojarán en la carpeta `MULTIMEDIA`.)

---

## Autor

Este proyecto fue realizado por:

- **Alumno**: Zaracho, Facundo – DNI N° 42.873.890  
- **Tutor**: Cristian Lukaszewicz  

Como parte de la **Práctica Profesional Supervisada** de la carrera de **Ingeniería Mecatrónica** en la **Facultad de Ingeniería de la Universidad Nacional de Lomas de Zamora**.
