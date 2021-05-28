# Clasificación de rostros reales vs generados

## Introducción

El objetivo de este proyecto consiste en realizar la clasificación de rostros, siendo capaz de percibir aquellos que son reales frente a otros generados mediante aprendizaje automático.

Para ello, se realiza una serie de pruebas, variando los descriptores de caracterísiticas de las imágenes, los descriptores e incluso, el uso de una región de interés (ROI).

## Conjunto de datos

El conjunto de datos usado es una variante del que podemos encontrar en Kaggle, [Real and Fake Face Detection](https://www.kaggle.com/ciplab/real-and-fake-face-detection), la cuál ha sido reducida la resolución de los 600px originales a 160px para facilitar el entrenamiento.

El conjunto de datos posee imágenes de caras reales (1081 muestras) y falsas (980 muestras). A su vez, los rostros falsos se categorizan de tres maneras: fácil (240 muestras), normal (480 muestras) y difícil (240 muestras).

## Descriptores y *embeddings*

En este proyecto, se usaron 4 formas de descriptores y *embeddings*
: **HOG**, **LBP**, **Facenet** y **VGG-Face**.

### HOG

El [histograma de gradientes orientados](https://en.wikipedia.org/wiki/Histogram_of_oriented_gradients) es un descriptor que cuenta las veces en las que hay un cierto gradiente orientado el localizaciones específicas de una imagen.

En nuestro caso, este descriptor realiza una descripción de la imagen con **32 características**.

![HOG](https://miro.medium.com/max/683/1*XZxX8V4OrVSwQ9e9snE3gA.png)

### LBP

El descriptor de [patrones locales binarios](https://en.wikipedia.org/wiki/Local_binary_patterns) consiste en dividir la imagen en celdas, realziar comparaciones entre los píxeles vecinos de cada celda. A continuación, si el valor del pixel central es mayor que el de sus vecinos, se escribe un 1 y, en caso contrario, un 0. Con esta codificación, se contruye un histograma.

En nuestro caso, este descriptor realiza una descripción de la imagen con **531 características**.

![LBP](https://raw.githubusercontent.com/arsho/local_binary_patterns/master/screenshot/lbp_output.png)

### Facenet

[Facenet](https://arxiv.org/abs/1503.03832) es una red convolucional profunda que genera unos *embeddings* a partir de las caras detectadas en las imágenes.

En nuestro caso, este descriptor realiza una descripción de la imagen con **128 características**.

![Facenet](https://miro.medium.com/max/1024/1*OmFw4wZx5Rx3w4TpB7hS-g.png)

### VGG-Face

VGG-Face es una red convolucional profunda que, a diferencia de Facenet, tiene una mayor dimensionalidad.

En nuestro caso, este descriptor realiza una descripción de la imagen con **2622 características**.

![VGG-Face](https://i1.wp.com/sefiks.com/wp-content/uploads/2019/04/vgg-face-architecture.jpg?ssl=1)

## Clasificadores

### SVM

Las [SVM o Máquinas de vectores de soporte](https://es.wikipedia.org/wiki/M%C3%A1quinas_de_vectores_de_soporte). A grandes rasgos, SVM construye unos hiperplanos de alta dimensionaldiad que separe las clases involucradas en el proceso.

![SVM](https://lh3.googleusercontent.com/proxy/HeBuoqkEzyIThjJ5_mIhVHfdJc1UP9-HU5LkIOPphKftQEZ0F8nPhscgDQnmpqujApWoppDj7SDtKn_0br7gAMGzGH86rgrpCGHAtfbAzNIXWQWVsg)

### Random Forest

Los [Random Forest o Bosques aleatorios](https://es.wikipedia.org/wiki/Random_forest) consisten en la combinación de árboles predictores, de los cuáles cada uno obtiene una parte distinta del conjunto de entrenamiento para clasificar y, al final, se realiza una votación, en la que se decide el resultado de las predicciones.

![Random Forest](https://miro.medium.com/max/724/1*i0o8mjFfCn-uD79-F1Cqkw.png)

### MLP

[MLP o Perceptrón multicapa](https://es.wikipedia.org/wiki/Perceptr%C3%B3n_multicapa) consiste en una red neuronal de múltiples capas que permiten superar el déficit que tiene el perceptrón para resolver: clases no linealmente separables. Este cuenta con una capa de entrada, una capa oculta y una de salida.

![MLP](https://upload.wikimedia.org/wikipedia/commons/6/64/RedNeuronalArtificial.png)

## K-fold

También conocido como [validación cruzada de K iteraciones](https://es.wikipedia.org/wiki/Validaci%C3%B3n_cruzada), es una técnica que permite garantizar que las particiones de entrenamiento y test del conjunto de datos sea independientes.

En este proyecto hacemos uso de K-fold con **K = 5**.

## ROI

En una serie de pruebas, se genera una versión alternativa del conjunto de datos (presente en [*/roi_dataset*](https://github.com/darkhorrow/real-fake-faces-classification/tree/master/roi_dataset)) que trata de obtener únicamente la región de la cara, eliminándose así cosas como los detalles del fondo.

Para este fin, se usa el [*CascadeClassifier* de OpenCV](https://docs.opencv.org/3.4/db/d28/tutorial_cascade_classifier.html). Además, OpenCV proporciona un modelo por defecto a usar para caras frontales, el fichero [haarcascade_frontalface_default.xml](https://github.com/darkhorrow/real-fake-faces-classification/blob/master/haarcascade_frontalface_default.xml), evitando la necesidad de entrenar uno propio.

## Pruebas realizadas

Con todas las técnicas y recursos mencionados anteriomente, se realizan un total de **12 pruebas** (sin contar el *baseline*).

Dichas pruebas son:

* HOG + SVM (Baseline)
* HOG + Random Forest
* HOG + MLP


* LBP + SVM
* LBP + Random Forest
* LBP + MLP


* Facenet + SVM
* Facenet + Random Forest
* Facenet + SVM + ROI
* Facenet + Random Forest + ROI
* Facenet + MLP


* VGG-Face + SVM + ROI	61.9	58.7	58.9
* VGG-Face + Random Forest + ROI	59.0	68.2	58.0

## Resultados

### HOG

|                           | Precision (%)| Recall (%)| Accuracy (%) | Time (s) |
|---------------------------|:------------:|:---------:|:------------:|:--------:|
| **HOG + SVM (Baseline)**  |   **59.8**   |   53.7    |     56.3     | **7.792**|
| **HOG + Random Forest**   |     57.6     | **62.7**  |     55.8     |  11.849  |
| **HOG + MLP**             |     58.9     |   61.5    |   **56.9**   |  543.912 |

### LBP

|                           | Precision (%)| Recall (%)| Accuracy (%) | Time (s) |
|---------------------------|:------------:|:---------:|:------------:|:--------:|
| **LBP + SVM**             |     59.3     |   56.2    |     56.4     |  35.582  |
| **LBP + Random Forest**   |   **60.7**   | **70.2**  |   **60.2**   |**29.915**|
| **LBP + MLP**             |     59.8     |   64.5    |     58.2     | 1413.396 |

### Facenet

|                                     | Precision (%)| Recall (%)| Accuracy (%) | Time (s) |
|-------------------------------------|:------------:|:---------:|:------------:|:--------:|
| **Facenet + SVM**                   |   **62.0**   |   61.6    |     59.6     |  23.195  |
| **Facenet + Random Forest**         |     58.8     | **70.6**  |     58.3     |  21.402  |
| **Facenet + SVM + ROI**             |     57.9     |   56.1    |     55.1     |**12.987**|
| **Facenet + Random Forest + ROI**   |     55.9     |   66.7    |     54.5     |  21.964  |
| **Facenet + MLP**                   |     61.3     |   67.1    |   **60.1**   | 907.388  |

### VGG-Face

|                                     | Precision (%)| Recall (%)| Accuracy (%) | Time (s) |
|-------------------------------------|:------------:|:---------:|:------------:|:--------:|
| **VGG-Face + SVM + ROI**             |   **61.9**   |   58.7   |   **58.9**   | 184.388  |
| **VGG-Face + Random Forest + ROI**   |     59.0     | **68.2** |     58.0     |**93.691**|


## Conclusiones
