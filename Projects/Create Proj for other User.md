CREAR UN PROYECTO EN NOMBRE DE OTRO USUARIO					Last Update: 30-Abril-2021
===========================================

Para crear un proyecto con un usuario pero que pertenezca a otro:

	oc new-project <nombre_proyecto> --as=<usuario> --as-group=system:authenticated --as-group=system:authenticated:oauth

 y miro la configuración del proyecto:

	oc describe project <nombre_proyecto>

  en la anotación "openshift.io/requester" veré el nombre del usuario al que he hecho pertenecer el proyecto.


 Otra forma más artesanal es modificar esa anotación en el namespace (en el proyecto no me deja) con:

	 oc edit namespace <nombre_proyecto>
 o con:

	oc annotate project  <nombre_proyecto> openshift.io/requester=<usuario> --overwrite

Para ver el administrador de un proyecto:

	oc describe rolebindig.rbac -n <nombre_proyecto>

 también se ven las serviceAccounts


PDF. "Applications" - Capítulo 1 "Projects" - 1.2. "Creating a project as another user"
	
	https://docs.openshift.com/container-platform/4.5/applications/projects/creating-project-other-user.html

IMPERSONALIZAR UN USUARIO
=========================

 Impersonalizar un usuario es permitir a ese usuario ejecutar comandos oc privilegiados añadiendo al final del comando "--as system:admin"
 Comportamiento similar al Sudoers del sistema operativo.

 Para esto, con usuario con privilegios de "cluster-admin", impersonalizo el usuario:

	oc create clusterrolebinding <nombre_rolebinding> --clusterrole=sudoer --user=<usuario>
