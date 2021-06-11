PROBES			Last Update: Nov-2020
-------

Monitorizan la app para ver si funciona bien.
OCP realiza estas pruebas de forma continua.

* 2 tipos:

	-> Liveness: Si la aplicación está bien, ha levantado de forma correcta. Si no está bien -> OCP reinicia el contenedor del pod.

	-> Readiness: si el contenedor del pod está preparado para recibir peticiones, tras arrancar, el contenedor tarda un tiempo en poder atender peticiones. Si no está "ready", OpenShift
	 	lo borra de los endpoints del servicio. Cuando esté "ready", lo añade a los end-points.

* Opciones para estos probes:

initialDelaySeconds: cuantos segundos pasan entre que arranca el pod y se ejecuta el probe por primera vez.
	Obligatorio: SI	   Por defecto: 0

timeoutSeconds: si el probe no contesta en esos segundos, se considera que ha fallado (un probe es una comprobación rápida, algo rápido de hacer)
	Obligatorio: SI	   Por defecto: 1

periodSeconds: Cada cuantos segundos se hace el probe
	Obligatorio: NO	   Por defecto: 1

successThreshold: Mínimo nº de éxitos consecutivos tras un fallo para considerar que el fallo fue algo puntual y que no pasa nada.

	Obligatorio: NO	   Por defecto: 1

failureThreshold: nº mínimo de fallos consecutivos del probe para considerar que ha fallado tras haber estado funcionando antes
	
	Obligatorio: NO	   Por defecto: 3

* Formas de chequeo de los probes:

	-> HTTP: se hace petición HTTP GET a un path dentro de la ruta y puerto donde esté la app expuesta. Si el código HTTP están entre 200-399 -> probe OK, si no, probe fallido.

		Se añaden al DC con:  oc set probe dc/<nombre_dc> --<tipo> --get-url=http://:<puerto>/<ruta> --period=x  --timeout-seconds=y ....  

			donde <tipo> es readiness o liveness, <nombre_dc> es el nombre del recurso DeploymentConfig, <puerto> y <ruta> es el puerto y ruta donde está el probe (debe estar 
			implementado en la app, no me lo puedo inventar).

	-> Ejecución de comando: Se ejecuta un comando dentro del contenedor, se usan cuando la app no es web. El comando debe ser algo muy "tonto", básico (echo, cat, touch). Si el comando
		devuelve 0 -> probe OK, si es distinto de cero -> probe fallido.

		Se añaden al DC con:  oc set probe dc/<nombre_dc> --<tipo>  -- <comando>

		   donde <tipo> es readiness o liveness, <nombre_dc> es el nombre del recurso DeploymentConfig y <comando> es el comando a ejecutar dentro del contenedor con sus parámetros.
		  Este comando debe existir en el contenedor del pod.


	-> Socket: Se abre un puerto TCP, si se consigue abrir -> probe OK, si falla -> probe fallido


		Se añaden al DC con:  oc set probe dc/<nombre-dc> --<tipo> --open-tcp=<puerto> --period=x  --timeout-seconds=y ....  

		donde <tipo> es readiness o liveness, <nombre_dc> es el nombre del recurso DeploymentConfig y <puerto> es el puerto a abrir en el contenedor.

* Eliminar Probes:
		
  Para eliminar un probe de readiness:  oc set probes dc/<nombre_dc> --remove --readiness
  Para eliminar un probe de liveness:   oc set probes dc/<nombre_dc> --remove --liveness
  Para eliminar ambos probes: 		oc set probes dc/<nombre_dc> --remove --liveness --readiness
