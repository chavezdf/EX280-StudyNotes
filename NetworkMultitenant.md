NETWORKING MULTITENANT				3-Mayo-2021
-----------------------
Cuando en el clúster se está utilizando el modo de red/plugin Multitenant, se puede:

-> Ver la VNID de un proyecto:

	oc get netnamespaces 		-> 	columna NETID

-> Comunicar dos proyectos para que sus pods se vean: (se les pone a ambos proyectos la misma VNID)

	oc adm pod-network join-projects --to=<project1> <project2>

-> Hacer un proyecto global (que tenga la VNID=0): todos los pods de otros proyectos tienen acceso a los pods de este proyecto 
	y los pods de este proyecto tienen acceso a todos los pods de otros proyectos:

	oc adm pod-network make-projects-global <project1> <project2>

- Aislar dos proyectos: ningún pod de otros proyectos tendrá acceso:

	oc adm pod-network isolate-projects <project1> <project2>


En los tres comandos, en lugar de dar los nombres de los proyectos unos detrás de otros, podemos utilizar un "selector", que es una lista de etiquetas con sus valores separadas por comas, p.e.:

	oc adm pod-network join-projects --selector="<etiq1>=<valor1>,<etiq2>=<valor2>"

   y etiquetamos los proyectos a comunicar con esas etiquetas:

	oc label namespace <proyecto1> <etiq1>=<valor1> <etiq1>=<valor1>

