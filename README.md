# Clasificación de rostros reales vs generados

## Introducción

El objetivo de este proyecto consiste en realizar la clasificación de rostros, siendo capaz de percibir aquellos que son reales frente a otros generados mediante aprendizaje automático.

Para ello, se realiza una serie de pruebas, variando los descriptores de caracterísiticas de las imágenes, los descriptores e incluso, el uso de una región de interés (ROI).

## Conjunto de datos

El conjunto de datos usado es una variante del que podemos encontrar en Kaggle, [Real and Fake Face Detection](https://www.kaggle.com/ciplab/real-and-fake-face-detection), la cuál ha sido reducida la resolución de los 600px original a 160px para facilitar el entrenamiento.

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



## Resultados

### HOG

|                           | Precision (%)| Recall (%)| Accuracy (%) |
|---------------------------|:------------:|:---------:|:------------:|
| **HOG + SVM (Baseline)**  |   **59.8**   |   53.7    |     56.3     |
| **HOG + Random Forest**   |     57.6     | **62.7**  |     55.8     |
| **HOG + MLP**             |     58.9     |   61.5    |   **56.9**   |

### LBP

|                           | Precision (%)| Recall (%)| Accuracy (%) |
|---------------------------|:------------:|:---------:|:------------:|
| **LBP + SVM**             |     59.3     |   56.2    |     56.4     |
| **LBP + Random Forest**   |   **60.7**   | **70.2**  |   **60.2**   |
| **LBP + MLP**             |     59.8     |   64.5    |     58.2     |

### Facenet

|                                     | Precision (%)| Recall (%)| Accuracy (%) |
|-------------------------------------|:------------:|:---------:|:------------:|
| **Facenet + SVM**                   |   **62.0**   |   61.6    |     59.6     |
| **Facenet + Random Forest**         |     58.8     | **70.6**  |     58.3     |
| **Facenet + SVM + ROI**             |     57.9     |   56.1    |     55.1     |
| **Facenet + Random Forest + ROI**   |     55.9     |   66.7    |     54.5     |
| **Facenet + MLP**                   |     61.3     |   67.1    |   **60.1**   |

### VGG-Face

|                                     | Precision (%)| Recall (%)| Accuracy (%) |
|-------------------------------------|:------------:|:---------:|:------------:|
| **Facenet + SVM + ROI**             |   **61.9**   |   58.7    |   **58.9**   |
| **Facenet + Random Forest + ROI**   |     59.0     | **68.2**  |     58.0     |
