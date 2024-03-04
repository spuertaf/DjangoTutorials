# Taller 01

# Sobre el proyecto

El proyecto ReceiptStruct pretende digitalizar y estructurar la información no estructurada hallada en las imágenes de recibos de compra de los establecimientos comerciales.

Como usuario, la plataforma proveerá de un registro y login, podrá seleccionar un conjunto de imágenes de recibos y cargarlos a la misma, para su posterior estructuración; una vez estructuradas la plataforma enviara una notificación vía correo electrónico notificándole si su información ha podido, o no, ser estructurada. 

## La diferencia de este taller con la estructura propuesta

Dada la naturaleza del problema que se está tratando de solucionar, lo que se encontrará en este taller difiere de la estructura planteada. Es por esto que, este README tiene como objetivo el entendimiento de la arquitectura general propuesta para la solución de la problemática planteada y ahondar sobre qué parte de la solución abarca el código desarrollado.

# Arquitectura del sistema

## Contexto del sistema

Tal y como se había mencionado, la problemática a tratar es la estructuración de datos no estructurados, en este caso particular, la estructuración de datos presentes en imágenes de recibos de compra. Con este propósito en mente, la solución planteada consta de dos (2) sistemas, los cuales se detallan en la figura uno (1) y tabla uno (1).

**Figura 1**

*Diagrama de contexto del sistema ReceiptStruct.*

![receiptStructProject-system-context.svg](https://raw.githubusercontent.com/spuertaf/TopicosIngSoftware/f0d632b996dc7f09d7a1a1d24b930ed6de2bd424/taller_01/img/receiptStructProject-system-context.svg)

**Tabla** *Sobre los sistemas y sus roles.*

| Nombre del sistema | Rol del sistema |
| --- | --- |
| ReceiptStruct | Este sistema se encarga de interactuar con el usuario. Habilita un registro y login. Recibe y pasa a Google Cloud Platform las imágenes de los recibos. Muestra al usuario el resultado del trabajo de estructuración del recibo. |
| Google Cloud Platform | Almacena tanto la imagen del recibo, como los datos estructurados del mismo luego de efectuado el trabajo de estructuración. Realiza el trabajo de estructuración de datos. |

## Servicios a usar y arquitectura general en Google Cloud Platform

A continuación, la figura dos (2) muestra los servicios a usar y cómo se vería plasmada la arquitectura general del sistema en Google Cloud Platform; la tabla dos (2) ahonda en la explicación de los diferentes servicios y su propósito dentro de la arquitectura mostrada.

**Figura 2**

*Diagrama de la arquitectura del sistema en Google Cloud Platform.*

![receiptStructProject-gcp-arquitectural-diagram.png](https://raw.githubusercontent.com/spuertaf/TopicosIngSoftware/dev/taller_01/img/receiptStructProject-gcp-arquitectural-diagram.png)

**Tabla 2**

*Servicios de Google Cloud Platform a user y propósito de los mismos.*

| Nombre del servicio | Rol que desempeña |
| --- | --- |
| Cloud Functions: Front-end | Función serverless encargada de mostrar la página web al usuario una vez que este la invoque. La invocación de la misma se da una vez que el usuario accede a la URL de la página web. |
| Cloud Functions: Back-end | Función serverless encargada de proveer al front-end los datos y gestionar el proceso de carga de las imágenes en Cloud Storage. La invocación de esta función se da una vez que el front-end realice una petición a uno de los endpoints habilitados por la misma. |
| Cloud SQL | Servicio de almacenamiento encargado de persistir el nombre, correo electrónico, nombre de usuario y contraseña de los usuarios de la aplicación. |
| Cloud Functions: Application | Función serverless encargada de estructurar los datos no estructurados de la imagen; se comunica con el LLM Vertex AI Gemini para solicitar que, según la imagen dada, extraiga los datos de la misma. Convierte la respuesta dada por el modelo LLM en un archivo de Excel y carga el mismo a Cloud Storage. |
| Cloud Storage | Servicio de almacenamiento encargado de guardar tanto las imágenes dadas por el usuario, como los archivos de Excel resultantes del trabajo de estructuración. |
| Vertex AI Gemini | LLM encargado de extraer el contenido de una imagen de un recibo de compra. |

# Lo que contiene este taller

Por su parte este taller contiene el desarollo de la Cloud Functions: Application, encargada de estructurar los datos no estructurados de la imagen; se comunica con el LLM Vertex AI Gemini para solicitar que, según la imagen dada, extraiga los datos de la misma. Convierte la respuesta dada por el modelo LLM en un archivo de Excel y carga el mismo a Cloud Storage; tal y como fue mencionado en la tabla dos (2).

La figura número tres (3) enseña el diagrama de clases y las relaciones entre las mismas, desarrolladas para satisfacer el funcionamiento anteriormente descrito.

**Figura 3**

*Diagrama de clases del servicio Cloud Functions: Application*

![receiptStructProject-structure-receipt-job-class-diagram.svg](https://raw.githubusercontent.com/spuertaf/TopicosIngSoftware/f0d632b996dc7f09d7a1a1d24b930ed6de2bd424/taller_01/img/receiptStructProject-structure-receipt-job-class-diagram.svg)
