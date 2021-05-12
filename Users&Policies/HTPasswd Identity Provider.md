PROCEDIMIENTOS USANDO EL PROVEEDOR DE IDENTIDAD HTPASSWD				Last Update: 29-Abril-2021
========================================================
Tengo las variables de entorno siguientes:

	PASSKUBEADMIN: almacena la password del usuario "kubeadmin".
	APIMASTER: almacena la URL de la API del master
	PASSADMIN: almacena la password del usuario "admin"
	PASSDEV: almacena la password del usuario "developer"


CONFIGURAR PROVEEDOR DE IDENTIDAD HTPASSWD				
===========================================
Pasos:

1.- Hago login en el clúster con el usuario "kubeadmin" (o con algún usuario administrador con privilegios de "cluster-admin" si tengo ya configurado otro proveedor de autenticación):

	oc login -u kubeadmin -p $PASSKUBEADMIN $APIMASTER

2.- Creo el fichero con los usuarios:

	htpasswd -c -B -b misusuarios admin ${PASSADMIN}		-> Creo el fichero y añado usuario "admin" con encriptación bcrypt
	htpasswd -b misusuarios developer ${PASSDEV}			-> Añado usuario "developer" con encriptación md5 (la que se usa por defecto si no indico ninguna)

3.- Creo el secret que almacene este fichero:

	oc create secret generic htpass-secret --from-file htpasswd=misuusarios -n openshift-config		-> es OBLIGATORIO crearlo en el proyecto openshift-config

4.- Modifico el CR OAuth para que incluya el proveedor de autenticacion HTPasswd basado en este secret:

	oc get -o yaml oauth cluster > oauth.yaml		-> Obtengo la definición de recurso del CR OAuth actual
	vim oauth.yaml						-> Modifico el spec para que contenga:

---------------------------------------------
	spec:
          identityProviders:
          - htpasswd:
              fileData:
                name: htpass-secret				-> Nombre del secret donde he cargado el fichero
            mappingMethod: claim
            name: myusers					-> Nombre que le pongo al proveedor de identidad dentro de OpenShift
            type: HTPasswd
---------------------------------------------

	oc replace -f oauth.yaml				-> Cargo la nueva configuración del CR OAuth en OpenShift

 El Operator del OAuth, al modificar el CR OAuth, redespliega los pods del OAuth.

5.- Verifico los pods del sevidor OAuth se redespliegan sin probleas:

	oc get pod -n openshift-authentication				-> RUNNING

6.- Compruebo que los nuevos usuarios añadidos pueden logarse en el clúster

  FIN

MODIFICAR LOS USUARIOS EXISTENTES EN EL PROVEEDOR DE IDENTIDAD HTPasswd
========================================================================
Pasos:

1.- Hago login en el clúster con el usuario "kubeadmin" (o con algún usuario administrador con privilegios de "cluster-admin" si tengo ya configurado otro proveedor de autenticación):

	oc login -u kubeadmin -p $PASSKUBEADMIN $APIMASTER

2.- Obtener los usuarios actuales del cluster:

	oc extract secret/htp-secret -n openshift-config --to - > fichpass			-> crea el fichero "htpasswd" en el directorio actual con los datos cargados en el secret

3.- Añadir/Modificar/Borrar los usuarios que sean, p.e. al usuario "admin" le cambio la password a "redhat", creo el usuario "pedro" con password "temporal" y borro el usuario "developer":

	htpasswd -b fichpass admin redhat
	htpasswd -b fichpass pedro temporal
	htpasswd -D fichpass developer

4.- Modifico el secret:

	oc set data secret/htpass-secret --from-file htpasswd=./fichpass -n openshift-config 

  El Operator del OAuth, al modificar el secret que usa el CR OAuth, redespliega los pods del OAuth.

5.- Verifico que los pods del servidor OAuth levantan sin problemas:

	oc get pod -n openshift-authentication				-> RUNNING

6.- Comprobar que se puede hacer login con los usuarios

 FIN

Configuracion de  ROLES
=======================
> NOTA: Una vez añadidos los usuarios, habría que configurar roles para ellos.

Cuando se installa por primera vez el openshift se debe crear un usuario **CLUSTERADMIN**:

	oc admin policy add-cluster-role-to-user cluster-admin **admin**



