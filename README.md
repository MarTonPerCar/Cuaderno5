# Cuaderno 5 

Este Cuaderno muestra el desarrollo del quinto cuaderno de la asignatura, el cual incluye un único ejercicio.

### Pasos Previos

Antes de si quieras empezar hay que seccionarte de tener disponible 2 apartados fundamentales:

* Un entorno virtual creado con Python, opencv-python instalado (En este cuaderno se utilizará Anaconda). Además tambien se tiene que instalado estás librerías:

```
pip install imutils
pip install mtcnn
pip install tensorflow   
pip install deepface
pip install tf-keras
pip install cmake
pip install dlib
```

### Desarrollo

Para empezar dentro de nuestro cuadernos tendremos unas librerías instaladas:

```python
import cv2
import numpy as np

from mtcnn.mtcnn import MTCNN

import tkinter as tk
```
 
* **OpenCV (cv2)**: Se utiliza para la captura y procesamiento de imágenes y video en tiempo real. En el cuaderno se usará para capturar videos de la cámara web y añadir figuras sobre fotogramas. Es la librería principal de tratamiento de Fotogramas

* **NumPy (np)**: Proporciona estructuras de datos eficientes. En el cuaderno se usa para manipular datos de las imágenes y es una opción para crear un collage.

* **from mtcnn.mtcnn import MTCNN**: Esta librería importa el detector de rostros MTCNN (Multi-Task Cascaded Convolutional Neural Networks), que se usa para detectar caras en imágenes o videos. Es más preciso que los clasificadores tradicionales de Haar y puede detectar rostros en ángulos y condiciones variadas.

* **import tkinter as tk**: Esta librería importa Tkinter, que permite crear interfaces gráficas de usuario en Python. En este caso, se usa para construir una ventana con botones que permiten al usuario seleccionar diferentes modos o filtros visuales en la aplicación.

#### Tarea Unica - Desarrollar un filtro 

La tarea consiste en desarollar una aplicación interactiva que utiliza la cámara en tiempo real para aplicar diferentes filtros visuales sobre los rostros detectados. Usando clasificadores de detección de rostros y características faciales

El proyecto se estructura de la siguiente manera:

* Se desarrollarán 4 filtros únicos.

* Estos filtros se organizarán en un menú que permite seleccionar el filtro deseado mientras la cámara esté activa.

La implementación se compone de 5 bloques de código: 4 funciones que generan cada filtro y un archivo principal que configura el panel de control y administra la cámara.

El primer paso es desarrollar este archivo principal, que configura el panel de selección de modos y activa la cámara.

### Codigo Principal
---

El código comienza configurando los clasificadores Haar y cargando las imágenes que se utilizarán para cada filtro.

1. **Clasificadores Haar**:
   - `face_cascade`: Detecta rostros en tiempo real.
   - `eyepair_cascade`: Detecta pares de ojos, utilizado para posicionar gafas.
   - `nose_cascade`: Detecta la nariz, útil para filtros que requieren superponer elementos en esa área (por ejemplo, nariz de payaso).

2. **Carga de imágenes para filtros**:
   - **Sombrero** (`hat.png`)
   - **gafas** (`pngtree-wayfarer-sunglasses-with-black-frames-png-image_4487246.png`)
   - **peluca** (`peluca.png`)
   - **nariz roja** (`nariz roja.png`) 
   -  **mejillas** (`mejilla.png`) 
   - **Other face** (`pngimg.com - barack_obama_PNG39.png`): La imagen de un rostro de referencia se procesa para extraer solo la región del rostro, que luego se superpone sobre el usuario en uno de los modos.
   
    Todas ellas se cargan usando `cv2.imread` con el modo `cv2.IMREAD_UNCHANGED` para conservar la transparencia.

### Configuración de la ventana de selección de modos

Se configura una ventana de interfaz gráfica usando `Tkinter` para seleccionar los diferentes filtros de forma interactiva:

- **Variables de control**:
  - `mode`: Almacena el modo de filtro actualmente seleccionado.
  - `running`: Controla si el bucle de video está activo.

- **Interfaz de selección de modos**:
  - **Botones de modo**: Se crean cuatro botones que permiten al usuario cambiar entre modos de filtro específicos (`Sombrero de Vaquero`, `Obama`, `Gafas`, `Payaso`), cada uno con un color distinto para diferenciarlos.
  - **Botón de cierre de cámara**: Este botón permite al usuario cerrar la cámara y finalizar el programa sin necesidad de usar el teclado.

### Bucle principal de captura de video y aplicación de filtros

El programa inicia la captura de video y aplica el filtro correspondiente al modo seleccionado.

- **Captura de video**: Se usa el bucle tipico utilizado en anteriores cuadernos para capturar cada frame de la cámara
- **Detección de rostros**: Convierte cada fotograma a escala de grises y detecta rostros utilizando el clasificador `face_cascade`.
- **Aplicación de filtros**: Según el valor de `mode`, se llama a una función específica que aplica el filtro correspondiente
- **Actualización y salida**:
  - Muestra el fotograma procesado en una ventana de OpenCV.
  - Actualiza la ventana de `Tkinter` para manejar cambios de modo en tiempo real.

Este flujo permite aplicar filtros en tiempo real sobre los rostros detectados, con la posibilidad de cambiar de filtro de forma interactiva.

### Filtros Implementados
---
El código incluye cuatro filtros únicos, cada uno diseñado para añadir un elemento gráfico específico sobre el rostro detectado:

- **`modo_sombrero`**: Este filtro coloca un sombrero sobre la cabeza de la persona en el fotograma.
- **`modo_cambio_de_caras`**: Superpone una imagen de rostro de referencia (por ejemplo, la imagen de otra persona) sobre el rostro detectado.
- **`modo_gafas`**: Coloca unas gafas en la detección de los ojos, ajustándolas para cubrir la región de los ojos.
- **`modo_payaso`**: Añade elementos típicos de un disfraz de payaso, como una peluca, una nariz roja y mejillas rojas.

Cada filtro introduce una nueva dificultad y agrega un desafío adicional. Empezamos con el sombrero, que es el filtro inicial y se enfoca en la detección de la parte superior del rostro. Luego, el cambio de caras abarca toda la cara. Las gafas plantean el reto de detectar y ajustar el filtro sobre ambos ojos, y finalmente, el filtro de payaso combina múltiples elementos (como peluca y nariz), integrando varios filtros simultáneamente sobre diferentes partes del rostro.

#### Proceso General de Aplicación de Filtros

Cada filtro se desarrolla siguiendo una estructura similar para lograr una superposición precisa en el fotograma:

1. **Comprobación de tamaño mínimo**: Primero se verifica si el área del rostro detectado cumple con el tamaño mínimo necesario para aplicar el filtro.
2. **Ajuste de dimensiones**: La imagen del elemento gráfico (como el sombrero o las gafas) se redimensiona para que se ajuste al tamaño del rostro detectado en el fotograma.
3. **Cálculo de posición**: Se calcula la posición exacta en el fotograma donde debe colocarse el elemento gráfico, garantizando que quede correctamente alineado.
4. **Superposición de la imagen**: Finalmente, la imagen redimensionada se superpone sobre el fotograma en la posición calculada, aplicando el filtro visual deseado. El fotograma luego se redimensiona para su visualización final.

El unico filtro que no hace estos pasos es el de payaso, y su variación no es muy distinta de los otros si se conoce el proceso. Ahora se va a explicar cada filtro poco a poco


    IMPORTANTE

    Es necesario que las imágenes para los filtros tengan un canal alfa porque este canal permite definir la transparencia de cada píxel. El canal alfa es un cuarto canal en una imagen que indica cuán opaco o transparente es cada píxel. 
    
    Esto es útil para superponer imágenes, como un sombrero o gafas, ya que permite que solo los píxeles visibles del filtro (por ejemplo, el sombrero) se apliquen al fotograma, mientras que las áreas transparentes dejan ver el rostro detrás sin cubrirlo. Esto crea una integración visual más natural y precisa en el fotograma.

* **EJEMPLO IMAGEN CON CANAL ALPHA**

<p align="center">
  <img src="hat.png" alt="Imagen con canal alpha" width="300"/>
</p>

* **EJEMPLO IMAGEN SIN CANAL ALPHA (o en este caso con canal alpha falso)**

<p align="center">
  <img src="IMG_README/png-clipart-cowboy-hat-cowboy-hat.png" alt="Imagen con canal alpha" width="300"/>
</p>

### Sombrero
---
El filtro sombrero coloca un sombrero en la cabeza del rostro detectado en el fotograma. Se seguirá el mismo planteamiento descrito anteriormente:

1. **Comprobación del tamaño mínimo**: Verifica si el rostro detectado cumple con el tamaño mínimo para aplicar el filtro, asegurando que el sombrero solo se coloque en rostros suficientemente grandes.

2. **Redimensionamiento del sombrero**: El ancho del sombrero se ajusta para ser ligeramente mayor que el rostro (aproximadamente un 20% más ancho) y, manteniendo la proporción, se calcula su altura. Esto garantiza que el sombrero esté bien dimensionado para la cara detectada.

3. **Cálculo de la posición**:

    Para colocar el sombrero correctamente sobre la cabeza, se realizan dos cálculos importantes:

    -   **Posición vertical (y_offset)**:

        Para situar el sombrero justo encima de la cabeza, se coloca la parte inferior del sombrero a la altura de la parte superior del rostro detectado (y).
        Se calcula restando la altura del sombrero (sombrero_resized.shape[0]) a la coordenada y del rostro, colocando el sombrero encima del borde superior del rostro.
        Si esta posición sale del límite superior de la imagen (es negativa), se ajusta a cero para evitar que el sombrero se salga de la imagen.

    -   **Posición horizontal (x_offset)**:

        Para centrar el sombrero respecto al rostro, se calcula su posición horizontal restando la mitad de la diferencia entre el ancho del sombrero y el ancho del rostro. Esto alinea el sombrero simétricamente.
        También se asegura que x_offset no sea negativo, manteniéndolo dentro del borde izquierdo de la imagen.

4. **Superposición del sombrero**: La función recorre cada píxel del sombrero redimensionado (`sombrero_resized`) y, si el sombrero contiene un canal alfa para transparencia, solo aplica los píxeles visibles (donde `alpha != 0`). Cada píxel se coloca en el fotograma original (`frame`) únicamente si las coordenadas `y_offset + i` y `x_offset + j` están dentro de los límites de la imagen, lo que evita errores de superposición fuera del área visible. De esta forma, el sombrero se integra visualmente en el fotograma sobre la posición calculada, logrando un ajuste natural sobre el rostro detectado.

Este proceso coloca el sombrero sobre la cabeza del rostro detectado, ajustado en tamaño y posición para un efecto visual preciso y realista.

<p align="center">
  <img src="IMG_README/Sombrero.PNG" alt="Imagen con canal alpha" width="800"/>
</p>

### Gafas
---

El filtro Gafas coloca unas gafas en los ojos detectados en el fotograma, se recalca **en los ojos no por separado, ambos ojos**. Nuevamente se seguirá el mismo planteamiento descrito anteriormente:

1. **Comprobación del tamaño mínimo**: Verifica si los ojos detectados cumple con el tamaño mínimo para aplicar el filtro, asegurando que no aparezcan gafas de forma expontanea por la cámara.

2. **Redimensionamiento de las gafas**: Calcula un ancho ajustado para las gafas, ligeramente mayor que el ancho de los ojos, y ajusta la altura manteniendo la proporción. Redimensiona las gafas para que se adapten bien a la región de los ojos detectada.

3. **Cálculo de la posición**: Se calcula la posición exacta para centrar las gafas sobre los ojos siguiendo los mismo pasos que con el sombrero sino que aquí no hay riesgo de que se salgan de la pantalla con tanta facilidad así que se ignora el problema

4. **Superposición del sombrero**: Por ultimo, se superpone las gafas sobre el fotograma en la posición calculada, utilizando el canal alfa para aplicar solo los píxeles visibles y lograr un ajuste natural sobre ambos ojos. Igual que con sombrero

<p align="center">
  <img src="IMG_README/Gafas.PNG" alt="Imagen con canal alpha" width="800"/>
</p>

### Cambiador de caras
---

El filtro Cambiador de caras coloca la cara de otra persona pasada como parametro, puede ponerse cualquier cara siempre en cuando este sobre un canal alpha. Nuevamente se seguirá el mismo planteamiento descrito anteriormente aunque algo cambiando ya que primero se debe de hablar de una nueva función:

Aquí tienes la explicación detallada de las funciones `extraer_rostro` y `modo_cambio_de_caras` siguiendo el esquema utilizado:

---

#### Función `extraer_rostro`

Esta función detecta y extrae el rostro de la imagen pasada por parametro para prepararlo para su superposición en el filtro.

1. **Conversión a escala de grises**: Convierte la imagen de entrada a escala de grises 
2. **Detección del rostro**: Utiliza el clasificador Haar para detectar solo la cara. Si por algún casual no se detecta ningún rostro, devuelve una imagen completamente negra del mismo tamaño para evitar errores de superposición en el filtro de cambio de caras.

3. **Extracción del rostro**: Si se detecta un rostro, la función extrae la región de la imagen correspondiente al primer rostro detectado, devolviendo solo esa sección. Esto permite obtener una imagen de rostro recortada que puede utilizarse para superponerse en el filtro de cambio de caras.

---
Una vez tenemos la cara del individuo con la imagen en si, se aplica sobre el rostro siguiendo nuevamente la miesma estructura

1. **Comprobación del tamaño mínimo del rostro**: La función recorre cada rostro detectado y aplica el filtro solo si el rostro cumple con el tamaño mínimo.

2. **Redimensionamiento del rostro de referencia**: Ajusta el tamaño del rostro de referencia para que coincida con el ancho y la altura del rostro detectado en el fotograma.

3. **Superposición del rostro con transparencia**: Si la imagen del rostro de referencia incluye un canal alfa, se utiliza este canal para aplicar la transparencia sobre el rostro detectado en el fotograma. 

    - Se crea una máscara alfa (`alpha_mask`) y su inversa (`alpha_inv`) para controlar la transparencia.
    - Luego, para cada canal de color (BGR), combina los valores de `other_face` y del rostro en el fotograma, aplicando la máscara de transparencia para que el rostro de referencia se integre naturalmente.

Si la imagen de referencia no tiene canal alfa, se aplica directamente sobre el rostro detectado sin transparencia. El motivo de aplicar un canal alpha es que si no este se muy directo sobre el rostro.

El ejemplo utiliza una imagen de Obama con canal alpha

<p align="center">
  <img src="IMG_README/Obama.PNG" alt="Imagen con canal alpha" width="800"/>
</p>

### Filtro de payaso
---

El filtro de payaso es el más complejo de todos porque combina múltiples elementos superpuestos (peluca, nariz roja y mejillas) y utiliza varias detecciones faciales. 

A diferencia de los otros filtros, que aplican solo un elemento gráfico, el filtro de payaso necesita coordinar la posición y tamaño de varios componentes en diferentes partes del rostro, asegurando que se ajusten correctamente a la cara y no se sobrepongan de manera inconsistente. 

Este nivel de detalle y superposición de múltiples elementos hace que el filtro de payaso requiera un procesamiento más elaborado.

Primero se debe explicar una función aparte que es de vital importancia para este apartado.

#### overlay_image_alpha
---

La función `overlay_image_alpha` la cual permite integrar varios elementos gráficos sobre el rostro detectado en el fotograma de la cámara, **manteniendo sus áreas transparentes**. En palabras más simple, permite poner cada elemento poco a poco en su respectiva posición sin alterar nada más.

La función toma cuatro parámetros: la imagen base (`img`), la imagen que se quiere superponer (`img_overlay`), la posición en la que debe colocarse (`pos`), y una máscara de transparencia (`alpha_mask`). 

Este último es el canal alfa de la imagen superpuesta, que controla la opacidad de cada píxel, permitiendo áreas transparentes, semitransparentes u opacas.

1. **Definición de la región de superposición**: Se determina la posición en la imagen base donde se colocará la imagen de superposición, definiendo una región de interés (ROI) en la imagen base que coincide con el tamaño de `img_overlay`.

2. **Aplicación de transparencia**: Para cada canal de color (BGR), la función combina los valores de `img_overlay` y la imagen base usando `alpha_mask`. Así:
    - Los píxeles opacos de `img_overlay` reemplazan los de la imagen base.
    - Los píxeles parcialmente transparentes se mezclan, creando una transición suave.
    - Los píxeles completamente transparentes permiten ver la imagen base sin alteraciones.

3. **Integración en la imagen base**: La función asegura que los elementos gráficos se coloquen solo donde corresponda, manteniendo sus áreas transparentes o semitransparentes. 

---
Una vez se entiende esta función se puede explicar el nuevo filtro el cual lo unico que acaba haciendo es nuevamente lo mismo de antes, pero por secciones y aplicando la función de antes para la superpocisión de los elementos. Realmente lo que interesa es dividir lo que se va a añadir por secciones y ver unicamente sus caracteristicas, ya que nuevamente se repite los mismos procesos de arriba

1. **Superposición de la peluca**:
   - La peluca se coloca en la parte superior de la cabeza detectada. 
   - Se ajusta para cubrir un área que sea aproximadamente **1.5 veces** el ancho del rostro y tenga una altura similar a la del rostro detectado.
   - Las coordenadas de la peluca se calculan para colocarla justo por encima del rostro, sin salir del límite de la imagen.

2. **Detección y superposición de la nariz roja**:
   - Dentro del área del rostro detectado, se busca la región de la nariz utilizando el clasificador `nose_cascade`.
   - La imagen de la nariz roja se redimensiona para ser más grande y colocarse de manera un poco elevada sobre la nariz detectada
   - Se realiza una verificación de límites para evitar que la nariz roja salga de la región de interés o de la imagen principal.

3. **Superposición de las mejillas**:
   - Finalmente, se colocan dos imágenes de mejillas en ambos lados de la nariz. 
   - Las mejillas se redimensionan para ser proporcionales al ancho y altura del rostro detectado.
   - Se calcula la posición de cada mejilla a ambos lados de la nariz 
   - Se verifica que se superpongan correctamente sin salir de los límites de la región del rostro.


<p align="center">
  <img src="IMG_README/Payaso.PNG" alt="Imagen con canal alpha" width="800"/>
</p>

## DEMOSTRACIÓN ANIMADA DE TODOS LOS FILTROS

![Descripción del GIF](GIF-Filtros.gif)