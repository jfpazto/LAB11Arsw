### Integrantes
Jonathan Paez
Johan Guerrero
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

**Preguntas**

* ¿Qué es un Azure Function?  

	Azure Function es una solución para ejecutar fácilmente pequeños fragmentos de código en la nube. 
	Axure Functios hace que el desarrollo sea más productivo permite usar lenguajes como: C#, F#, Node,js, Java o PHP.

* ¿Qué es serverless?  

	Se refiere al modelo de computación que permita ejecutar durante un tiempo determinado de funciones sin necesidad de hacerse cargo de la infraestructura subyacente que se provisiona para dar el servicio, se encarga de ofrecer recursos de forma transparente, se escala de manera automática ,define una serie de restricciones referentes al procesamiento y un modelo pago por el consumo de los recursos.
	
* ¿Qué es el runtime y que implica seleccionarlo al momento de crear el Function App?    
	
	Un runtime carga todas las aplicaciones de un programa y las ejecuta en una plataforma,  este se implementa de manera local para proporcionar un servicio que sea muy similar al de la nube. 
	
* ¿Por qué es necesario crear un Storage Account de la mano de un Function App?  
	
	Crear un storage account es necesario para almacenar todos los objetos de datos como blobs, archivos,colas,tablas,discos entr otros, esta proporciona un espacio de nombres único para los datos de Azure Storage, los cuales se pueden acceder mediante HTTP o HTTPS.Otra carateristica importante es que con esto los datos se vuelven duraderos y altamente disponibles, seguros y escalabes de froma masiva.

* ¿Cuáles son los tipos de planes para un Function App?, ¿En qué se diferencias?, mencione ventajas y desventajas de cada uno de ellos.  
	
	**Plan de consumo** 
	Las instancias que se encuentran en ell host de Azure Functions se agregan y eliminan dinámicamente teniendo en cuenta la cantidad de eventos que entren, este plan no contiene un servidor y hace un escalamiento automatico, los recursos se cobran únicamente cuando las funciones son ejecutados, si las funciones se encuentran en una misma región se puede asignar el mismo plan de consumo
	El cobro se basa en la cantidad de ejecucuiones, tiempo de ejecución y la memoria que se utiliza.  
	* Plan de hospedaje predeterminado.  
	* Pague solo cuando se ejecutan las funciones.  
	* Escala de forma automática, incluso durante períodos de carga elevada. 
	
	**Plan Premium**   
 	Las instancias del host se agregan y eliminan en función de la cantidad de eventos que entran, este plan permite crear instancias perpetuamente calientes para así evitar los arranques frios, conectividad Vnet, ejecución limitada 60 minutos garantizados, varios tamaños de instancias como instancias de un núcleo, dos núcleos y cuatro núcleos. El cobro se basa en la cantidad de segundos centrales y la memoria utilizada en todas la instancias, al menos una de las instancias debe estar caliente. 
	* La aplicación de funciones se ejecuta de forma continua, o casi continua.  
	* Tiene un gran número de ejecuciones pequeñas y una factura de ejecución elevada, pero pocos GB por segundo en el plan de consumo.   
	* Necesita más opciones de CPU o memoria de las que proporciona el plan de consumo.  
	* Su código debe ejecutarse durante más tiempo del máximo permitido en el plan de consumo.  
	* Necesita características que no están disponibles en el plan de consumo, como la conectividad con red virtual.
	
	**Plan dedicado**  
	Las function app pueden ser ejecutadas en las maquinas virtuales dedicadas que otras aplicaciones de App Service, se debería usar para cuando se tienen maquinas virtuales subutilizadas que ta estén ejecutando otras instancias y si se desea proporcionar una imagen personalizada en la que se ejecuten las funciones.
	
	
* ¿Por qué la memorization falla o no funciona de forma correcta?  
	
	No funciona de la manera correcta porque los numeros que se generan son muy grandes, esto hace que la memoria se consuma dado esto la memorizacion no funciona.  
	
* ¿Cómo funciona el sistema de facturación de las Function App?  

	Functions se factura según el consumo de recursos observado, medido en gigabytes por segundos (GB-s). El consumo de recursos observado se calcula multiplicando el tamaño medio de memoria en GB por el tiempo en milisegundos que dura la ejecución de la función. La memoria que una función utiliza se mide redondeando al alza a los 128 MB más cercanos hasta un tamaño de memoria máximo de 1.536 MB, y el tiempo de ejecución se redondea al alza a los 1 ms más cercanos. Para la ejecución de una única función, el tiempo de ejecución mínimo es de 100 ms y la memoria mínima es de 128 MB, respectivamente. Los precios de Functions incluyen una concesión gratuita al mes de 400.000 GB-segundos.
	
**Informe**

Al realizar los pasos anteriores tenemos en la configuracion lo siguiente

![img](https://github.com/jfpazto/LAB11Arsw/blob/master/images/readme/Captura1.PNG)

Por último, para crear la aplicación de funciones, revisamos que todo haya quedado configurado correctamente en la pestaña **Revisar y crear**, para posteriormente darle clic sobre el botón **Crear** en la parte inferior de la pantalla.

Luego de crear la aplicación de funciones, se despliega la siguiente pantalla indicando que se ha completado la implementación de la función correspondiente.

![img](https://github.com/jfpazto/LAB11Arsw/blob/master/images/readme/Captura2.PNG)

## Despliegue de la aplicación de funciones desde Visual Studio Code

Después de crear satisfactoriamente la aplicación de funciones desde Microsoft Azure, abrimos el proyecto en **Visual Studio Code**. Luego, instalamos la extensión **Azure Functions** 

Seleccionamos la función creada en **Microsoft Azure** FuntionProjectfibonacci

![img](https://github.com/jfpazto/LAB11Arsw/blob/master/images/readme/Captura3.PNG)

y Desplegamos

![img](https://github.com/jfpazto/LAB11Arsw/blob/master/images/readme/Captura4.PNG)

Luego de desplegar la función a Microsoft Azure desde VIsual Studio Code, se observa un consumo de memoria, indicando que la función se desplegó satisfactoriamente utilizando los recursos correspondientes para la operación, como se puede ver a continuación.

![img](https://github.com/jfpazto/LAB11Arsw/blob/master/images/readme/Captura6.PNG)

5. Modifique la coleción de POSTMAN con NEWMAN de tal forma que pueda enviar 10 peticiones concurrentes. Verifique los resultados y presente un informe.

	![img](https://github.com//ARSW-Lab9/blob/main/images/readme/5.1.PNG)

	![img](https://github.com//ARSW-Lab9/blob/main/images/readme/5.2.png)


6. Cree una nueva Function que resuleva el problema de Fibonacci pero esta vez utilice un enfoque recursivo con memoization. Pruebe la función varias veces, después no haga nada por al menos 5 minutos. Pruebe la función de nuevo con los valores anteriores. ¿Cuál es el comportamiento?.

	Primero, se realizaron dos pruebas, en la primera se calculó el 1000 n-ésimo número y en la segunda el 10000 n-ésimo número.

	**Primera prueba**

	![img](https://github.com//ARSW-Lab9/blob/main/images/readme/6.2.PNG)

	![img](https://github.com//ARSW-Lab9/blob/main/images/readme/6.3.1.png)

	**Segunda Prueba**

	![img](https://github.com//ARSW-Lab9/blob/main/images/readme/6.4.1.png)

	![img](https://github.com//ARSW-Lab9/blob/main/images/readme/6.5.1.png)

	Como se puede observar en la primera prueba todas las peticiones fueron exitosas, mientras que en la segunda prueba todas las peticiones fallaron debido a los límites de la recursión. Además, con este nuevo enfoque los tiempos de respuesta se redujeron bastante.

