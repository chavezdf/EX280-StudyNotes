MODOS DE AISLAMIENTO DE LA RED SDN EN OPENSHIFT			Last Update: 3-Mayo-2021
=================================================

El modo de funcionamiento de la red de pods se configura cuando se instala OpenShift y no se puede cambiar después.
Se hace en el fichero inicial con los datos de la instalación "install-config,yaml", en la sección "defaultNetwork".

Hay 3 modos de configurar la red de pods:

	-> Network policy (explicado en detalle en el PDF del curso)
	Permite configurar a nivel de proyecto el aislamiento usando recursos NetworkPolicy. En OCP 4.5 es el modo por defecto.
	Si no se define en el proyecto ningún recurso networkPolicy, los pods del proyecto pueden acceder a los pods de otros
	proyectos.
	Una vez que se crea un recurso networkPolicy en el proyecto, OpenShift prohibe todo tráfico entrante y/o saliente salvo 
	el que se defina en el recurso.

	-> Multitenant
	Aislamiento a nivel de proyecto. Pods de distintos proyectos no se pueden comunicar, no pueden recibir/enviar paquetes 
	de/a otros pods/servicios de proyectos distintos al que están. 

	A cada proyecto se le asigna una VLAN ID única (VNID) y los pods sólo pueden acceder a pods que tengan la misma VNID.

	Los pods en un proyecto con VNID=0 pueden comunicar con pods en cualquier proyecto y los pods de cualquier proyecto
	pueden comunicar con ellos. Suelen ser proyectos de infraestructura.
 	
	Ver más info en Networking_multitenant.txt
	
	-> Subnet
	Proporciona una red plana de pods donde todos pueden comunicar con todos aunque estén en proyectos distintos.
	No recomendable en producción.



PDF: "Networking" - Capítulo 12 - 12.1.1.- "OpenShift SDN network isolation modes"

 	https://docs.openshift.com/container-platform/4.5/networking/openshift_sdn/about-openshift-sdn.html#nw-openshift-sdn-modes_about-openshift-sdn

NOTA:
	En el caso de OpenShift 3.x, existen 3 plugins para configurar la red de pods:

	-> ovs-subnet: (por defecto)
	Red plana de pods donde cualquier pod puede comunicar con otro

	-> ovs-multitenant:
	Aislamiento a nivel de proyecto para pods y servicios. Cada proyecto recibe un VNID que identifica el tráfico de los pods asignados
	al proyecto. Los pods de distintos proyectos no pueden enviarse tráfico.
	Con VNID=0 los pods de ese proyecto pueden comunicar con cualquier otro y al revés.

	Ver más info en Networking_multitenant.txt

	-> ovs-networkpolicy:
	Configuración de aislamiento con políticas usando el recurso NetworkPolicy.
