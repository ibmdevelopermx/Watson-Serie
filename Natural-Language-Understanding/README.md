[![Build Status](https://api.travis-ci.org/IBM/watson-second-opinion.svg?branch=master)](https://travis-ci.org/IBM/watson-second-opinion)

#  Crea un Analizador de reviews utilizando Watson Natural Language Understanding

![demo](https://i.makeagif.com/media/6-07-2018/IeEcIv.gif)

En este IBM recipe, crearemos una app utilizando Node.js que toma los reviews de usuarios de Amazon y los manda a un servicio de Natural Language Understanding (NLU). Los reviews de los usuarios se guardarán en una base de datos en Cloudant. El servicio de Natural Language Understanding mostrará el sentimiento general de las reviews. La aplicación demo hará toda la lectura de las reviews por ti y dara un insight general acerca de estas. Este recipe puede ser útil para developers que estén buscando procesar múltiples documentos con Watson Natural Language Understanding.

Cuando termines este recipe, podrás entender como: 

* Interactuar con Watson Natural Language Understanding utilizando el Node SDK de Watson 
* Crear una interfaz de usuario para desplegar el resultado del servicio de NLU
* Crear y usar una base de datos NoSQL en Cloudant
* Desplegar una app en Node.js para analizar reviews de productos

<!--Remember to dump an image in this path-->
![Architecture](/docs/app-architecture.png)

## Antes de empezar te recomendamos:
* Realizar el [PreWork][url-prework].
* Si eres estudiante o profesor y tienes correo institucional te recomendamos los [Cupones][url-cupones].
* Si tienes algun codigo promocional te decimos como [Aplicarlos][url-aplica].

[url-prework]: https://github.com/ibmdevelopermx/Watson-Serie#Prework
[url-cupones]: https://github.com/ibmdevelopermx/Watson-Serie#Cupones-para-profesores-y-estudiantes
[url-aplica]: https://github.com/ibmdevelopermx/Watson-Serie#Cargar-cr%C3%A9ditos-en-IBM-Cloud


## Arquitectura de la app
1. El usuario despliega la app en IBM Cloud. El usuario interactua con la interfaz de usuario de la app.
2. El usuario ingresa la URL del producto y la app comenzará a obtener las reviews del producto.
3. Después, la app guarda las reviews en la base de datos para usarlas más tarde.
4. La app comienza a subir las reviews en el servicio de NLU.
5. Después de que el servicio de NLU termine de procesar las reviews, la app guarda el resultado (Sentimiento General y las Entidades más Importantes) en Cloudant. El usuario verá el resultado en la UI

## Componentes Incluidos
* [Watson Natural Language Understanding](https://www.ibm.com/watson/services/natural-language-understanding/): Analiza el texto para extraer meta-data de el contenido como sentimiento general, emociones, conceptos, entidades, palabras claves, categorias, relaciones y roles semánticos.
* [Cloudant NoSQL DB](https://cloud.ibm.com/catalog/services/cloudant): Una capa de datos completamente administrada y diseñada para aplicaciones web y móviles modernas que aprovecha un esquema JSON flexible.

## Tecnologías destacadas
* [Node.js](https://nodejs.org/): Un entorno JavaScript open-source para ejecutar código JavaScript del lado del servidor.
* [Databases](https://en.wikipedia.org/wiki/IBM_Information_Management_System#.22Full_Function.22_databases): Repositorio para guardar y manejar colecciones de datos.
* [Cloud](https://developer.ibm.com/depmodels/cloud/): Acceso a recursos informáticos y de tecnología de la información a través de Internet.

## Mira el Programa en Acción!

[![](docs/youtubePicture.png)](https://www.youtube.com/watch?v=wwNAEvbxd54&list=PLVztKpIRxvQXhHlMQttCfYZrDN8aELnzP&index=1&t=1s)

# Primeros pasos...

Utiliza el botón ``Deploy to IBM Cloud`` **O** crea el servicio y correlo de manera local.

## Desplegar a IBM Cloud

Si no tienes aún una cuenta de IBM Cloud, puedes crearla [aquí](https://cloud.ibm.com/registration?cm_mmc=Email_Events-_-Developer_Innovation-_-WW_WW-_-horea-porutiu%5CWatsonAPITutorial%5CNov2017%5CYoutubeTutorial%5Cglobaldevadvgrp%5CSanFrancisco%5CCA%5CUnitedStates%5CKubernetes&cm_mmca1=000019RS&cm_mmca2=10004805&cm_mmca3=M99938765&cvosrc=email.Events.M99938765&cvo_campaign=000019RS).

[![Desplegar a IBM Cloud](https://cloud.ibm.com/devops/setup/deploy/button.png)](https://cloud.ibm.com/devops/setup/deploy?repository=https://github.com/IBM/watson-second-opinion)

1. Presiona el botón `Deploy to IBM Cloud`
    > La toolchain utiliza GitHub para su control de fuentes. Es posible que se le solicite que autentique la toolchain para que puedas utilizarla en tu cuenta.
    La toolchain clonará este repositorio y lo utilizará para desplegar la app
<!--Paso opcional-->
2. En el apartado de Toolchains, da clic en ``Delivery Pipeline`` para mirar como se despliega la app. Una vez desplegada, puedes visualizar la app dando clic en ``View app``.

<!--update with service names from manifest.yml-->
3. Para ver la app y los servicios creados y configurados en este Recipe, utiliza el IBM Cloud dashboard. El nombre de la app lo podrás editar en la toolchain. Los siguientes servicios serán creados y, para identificarlos de una manera más fácil puedes nombrarlos de la siguiente manera utilizando estos prefijos:
    * wso-nlu 
    * wso-cloudant

### Actualiza el entorno para el despliegue de tu app 

![envVar1](https://i.makeagif.com/media/6-07-2018/Gfmeju.gif)

1. Ingresa a https://cloud.ibm.com/dashboard/apps/
2. Localiza y da clic en la app recién creada
3. Selecciona `Runtime` en en menú izquierdo
4. Selecciona el apartado de `Environment Variables` que está en el centro de la página
5. Desplazate hacia el apartado de `User defined variables` que se encuentra más abajo
6. Haz clic en ``add``. 
7. ESTE PASO ES EXTREMADAMENTE IMPORTANTE. Asegurate de escribir el nombre de la variable env EXACTAMENTE COMO SE MUESTRA, de no hacerlo, la app no funcionará. Desplazate hacia arriba hasta que veas el apartado `VCAP_SERVICES`. Entonces podrás ver el apartado `cloudantNoSQLDB` y debajo de el el apartado `url`. En el apartado de `Name`, escribe el `CLOUDANT_URL`, y debajo de el campo `Value`, pega la `url` de la sección `cloudantNoSQLDB` en el apartado de `VCAP_SERVICES`.

![envVar2](https://i.makeagif.com/media/6-07-2018/ubRZcv.gif)

8. **🚧🚧🚧 EL GIF DE ARRIBA ESTÁ DESACTUALIZADO. EN LUGAR DE NLU_USERNAME tienes que escribir una iam_apikey🚧🚧🚧** Repite el paso 6 pero ahora en el apartado de name, escribe la `iam_apikey` que se encuentra en el apartado `credential` del servicio `natural-language-understanding` en la sección `VCAP_SERVICES` y obtén el valor de `apikey`.
9. Da clic en ``save``.
10. Todo listo! . Da clic en ``visit App URL`` en la parte de arriba de la página para interactuar con la app. Solo copia y pega el url de un producto de Amazon en la app, y da clic en la lupa y voilà! Watson te dará insigts de tu producto.

## Desplegar de manera local

### 1. Clona este repositorio

```
$ git clone https://github.com/IBM/watson-second-opinion
$ cd watson-second-opinion/
```

### 2. Instala las Dependencies

```
$ npm install
```

### 3. Crea los IBM Cloud services

Crea el siguiente servicio:

* [**Watson Natural Language Understanding**](https://cloud.ibm.com/catalog/services/natural-language-understanding)
* [**Cloudant NoSQL DB**](https://cloud.ibm.com/catalog/services/cloudant/)


### 4. Obtén las credenciales de servicio

Una vez el servicio esté creado, da clic en el, y después da clic en `Service credentials` en la esquina superior izquierda de la página. Despues da clic en el apartado `New credential` y después en `add`. Para la `Cloudant NoSQL DB`, guarda el valor en la casilla `url`. Para el servicio de `Natural Language Understanding` guarda el valor en el apartado de `iam_apikey`.

### 5. Establecemos las variables de ambiente 

Después de correr el comando ``npm install`` de el paso 2, necesitamos definir 3 variables de ambiente que obtuvimos en el paso 4. Corre los siguientes comandos para establecer esas variables (tus usernames / passwords / url serán diferentes):

```
$ export CLOUDANT_URL=https://f9d9e9d9-8b3f-4625-a425-7a8bbd57aeaf-bluemix:f6f4f68665aa1b6b7821ca0117302353427eb643306d3d2652bc867768bc4c80@f9d9e9d9-8b3f-4625-a425-7a8bbd57hdsf-bluemix.cloudant.com

$ export IAM_APIKEY=xJLkOtKCXISqQHwjfdRBb99u2r-5sJ7n_sEsfsTJXZiJ


```

### 6. Corre la App

Corre la app iniciando Node.

```
$ node app.js
```
Ve a `http://localhost:4000/` en tu navegador, y podrás observar la UI de Watson Second Opinion.

Encuentra un producto en Amazon que te interese utilizar, cópia el URL de la página de producto y pegala en la app, da cloc en  🔍. Después de que el servicio de NLU termine de procesar todas las reviews, la app mostrará el sentimiento general y las entidades principales encontradas:


![Landing Page](docs/analysis.png)

Listo! Ahora puedes correr Watson Second Opinion en tu máquina!. 

# Links

* [Watson Node.js SDK](https://github.com/watson-developer-cloud/node-sdk)

# License
This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
