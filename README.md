### Escuela Colombiana de Ingeniería
### Arquitecturas de Software - ARSW

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/en-us/free/search/?&ef_id=Cj0KCQiA2ITuBRDkARIsAMK9Q7MuvuTqIfK15LWfaM7bLL_QsBbC5XhJJezUbcfx-qAnfPjH568chTMaAkAsEALw_wcB:G:s&OCID=AID2000068_SEM_alOkB9ZE&MarinID=alOkB9ZE_368060503322_%2Bazure_b_c__79187603991_kwd-23159435208&lnkd=Google_Azure_Brand&dclid=CjgKEAiA2ITuBRDchty8lqPlzS4SJAC3x4k1mAxU7XNhWdOSESfffUnMNjLWcAIuikQnj3C4U8xRG_D_BwE). Al hacerlo usted contará con $200 USD para gastar durante 1 mes.

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Escalabilidad Serverless (Functions)

1. Cree una Function App tal cual como se muestra en las  imagenes.

![](images/part3/part3-function-config.png)

![](images/part3/part3-function-configii.png)

2. Instale la extensión de **Azure Functions** para Visual Studio Code.

![](images/part3/part3-install-extension.png)

3. Despliegue la Function de Fibonacci a Azure usando Visual Studio Code. La primera vez que lo haga se le va a pedir autenticarse, siga las instrucciones.

![](images/part3/part3-deploy-function-1.png)

![](images/part3/part3-deploy-function-2.png)

4. Dirijase al portal de Azure y pruebe la function.

![](images/part3/part3-test-function.png)

5. Modifique la coleción de POSTMAN con NEWMAN de tal forma que pueda enviar 10 peticiones concurrentes. Verifique los resultados y presente un informe.

6. Cree una nueva Function que resuleva el problema de Fibonacci pero esta vez utilice un enfoque recursivo con memoization. Pruebe la función varias veces, después no haga nada por al menos 5 minutos. Pruebe la función de nuevo con los valores anteriores. ¿Cuál es el comportamiento?.

**Preguntas**

* ¿Qué es un Azure Function? 
    - Azure Function nos permite ejecutar pequeños y  simples (tanto como lo desee el desarrollador) fragmentos de codigos en la nube. Esto sin la necesidad de preocuparce de administrar la infraestructura como por ejemplo crear una maquina virtual configurar su red, etc. 
    
* ¿Qué es serverless? 
    -  Como anteriormente se describia en Azure Function no nos tenemos que preocupar por la infraestructura, esto es lo que caracteriza principalmente el significado de serverless, el proveedor (en este caso Azure) en la nube  es responsable de ejecutar un fragmento de código mediante la asignación dinámica de los recursos. Y cobrando solo por la cantidad de recursos utilizados para ejecutar el código. El código, generalmente, se ejecuta dentro de contenedores sin estado que pueden ser activados por una variedad de eventos que incluyen solicitudes HTTP
    
* ¿Qué es el runtime y que implica seleccionarlo al momento de crear el Function App? 
    - En los contenedores  que se ejecutan estos codigos, tienen su propio sistema operativo o tambien llamado runtime que por lo general son mas simples y menos pesados que un sistema operativo habitual, permitiendo asi ejecutar los programas. Como por ejemplo dockers que su motor provee instancias en cada servidor en el que desee ejecutar contenedores y proporciona un conjunto sencillo de comandos que puede utilizar para crear, iniciar o detener contenedores.

* ¿Por qué es necesario crear un Storage Account de la mano de un Function App? 
    -  Este es neserio para definir un espacio unicos  de almacenamiento en la nube, en los que se guardaran los blobs, archivos , colas, tablas entre otros.
    
* ¿Cuáles son los tipos de planes para un Function App?, ¿En qué se diferencias?, mencione ventajas y desventajas de cada uno de ellos.
    -  Consumption: El plan de consumo de Azure Functions factura en función del consumo de recursos y las ejecuciones que se hace por cada segundo, este plan viene con una concesión mensual de 1.000.000 de solicitudes y 400.000 Gbs de consumo de recursos por mes y por suscripción en el precio de pago por uso en todas las aplicaciones de funciones de esa suscripción.

    - Premium plan: El plan Premium ofrece básicamente las mismas funciones y el mismo mecanismo de escalado que usa el plan Consumption sin arranque en frio. Este plan a diferencia del consumption cuenta con un rendimiento mucho mejor y adicionalmente acceso a VNET. Este plan factura por segundo en función del numero de vCPUs y Gbs que consuman sus funciones Premium. 
    
* ¿Por qué la memorizacion falla o no funciona de forma correcta? 
     - Por lo general esos contenedores tienen un espacio de memoria definido, para casos muy grandes en el script de realizar la funcion fibonacci puede ser que se la memoria no de para calcular casos muy grandes dando como resultado fallas otra razón son que si pueda realizar este calculo pero la magnitud del número calculado es demasiado grande dando como resultados valores incorrectos. 
     
* ¿Cómo funciona el sistema de facturación de las Function App? 
     - Como se dijo anteriormente los funciones permiten ejecutar simples scrypts por tal razón facturan en función del consumo de recursos y las ejecuciones por segundo
     
* Informe 

## Informe 

* En la siguente imagen podemos ver el proceso de creación y ejecución de la function en Azure. 
![](images/fibonacci%20memo.PNG)

* Acontinuación podemos ver los resultados al momento de ejecutar las pruebas con Postman, realizando con y sin la tecníca de memorización. ( El proyecto por defecto se podria decir que existia una memorización parcial ya que este no era recurrentemente del todo, impidiendo asi que se realizarán de nuevo estos calculos). Se puede ver una leve reducción en el tiempo de ejecución de las dos pruebas.

    - Con dinamica 
    
    ![](images/pruebas%20con%20dinamica.PNG) 
    - Sin dinamica  
    
    ![](images/pruebas%20sin%20dinamica.PNG)  


* Los resultados en consumo de cpu si varian bastante y esto es por que los resultados con dinamica no tenian que ser calculados de nuevo, reduciendo asi el consumo de la cpu.
    - Con dinamica
![](images/rendimiento%20con%20dinamica.PNG)  
    - Sin dinamica 
![](images/rendimiento%20sin%20dinamica.PNG)  


## Autores 

* Andres Felipe Davila (Guaton)
* David Leonardo Coronado 
