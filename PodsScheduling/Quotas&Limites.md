VARIOS			- Last Update: 8-Octubre-2020
======

Unidad de CPU
--------------
La unidad de medida de CPU es KCU - Kubernetes Compute Unit (KooCoo)
 Equivale a 1 hilo hyperthread de CPU core
 1 milicore es 1/1000 de una CPU core
 
 Se fracciona en milicores (m)

	1KCU = 1000 milicores = 1000m

Si no aparece unidad, ¡Son KCUs!

QUOTAS, LÍMITES Y CLUSTER QUOTAS
--------------------------------

Sólo usuarios con role de cluster-admin (se puede tener este role sólo en un/os proyecto/s, no es necesario tenerlo a nivel de clúster) pueden crear ClusterResourceQuota, ResorceQuota y LimitRange.

	oc describe node -> se ven los recursos pedidos por los pods que ejecutan en los nodos

	oc adm top nodes -> los recursos que se están usando realmente

* Request:

 Se evalúan cuando se hace el schedule del pod. Sirven para decidir que nodo es el mejor.
 Lo que "piden" los pods para poder ejecutar en un nodo (resource request) y el límite de lo que piden (resource limits).
 Se definen en cada container del pod.	
 
 Se crean con oc set resources o modificando directamente el recurso donde está definido el pod con oc edit.

* Quotas:

 Están dentro de un proyecto y aplican a los recursos de ese proyecto. 

 Limitan cuántos recursos de un tipo se pueden crear (evitan crecimiento desmedido de Etcd) o cuánto se puede utilizar de recursos de computación (evitan utilización masiva de recursos de los nodos).

 Se evalúan al intentar crear un recurso en el proyecto.

 Se pueden crear a partir de fichero con definición de recurso o con oc create quota.
 
 NOTA: Para obtener la definición de recurso: PDF documentación oficial o oc explain resourcequota --recursive=true
 
	PDF - "Applications" - Capítulo 4 -> punto 4.1.6 - "Creating a quote"

* Límites:

 Están dentro de un proyecto. Valores por defecto para pods y containers de "resource request" y "resource limits".

 Se evalúan en Run-time, limitan la máx. cantidad de recursos que un contenedor/pod puede usar en runtime.

 Sólo se pueden crear a partir de fichero con definición de recurso.

 NOTA:  Para obtener la definición de recurso: PDF documentación oficial o oc explain limitrange --recursive=true

	PDF - "Nodes" - Capítulo 6 -> punto 6.3 "Setting Limit Ranges"

* Clúster Quotas:

 No están en ningún proyecto, van a nivel de clúster.
 Afectan a varios proyectos. Evitan ir proyecto por proyecto definiendo cuotas.

 Se pueden poner por "owner" (usando una anotación que lleva el proyecto) o por una etiqueta que lleven los proyectos.

	PDF - "Applications" - Capítulo 4 -> punto 4.2. - "Resource Quota across multiple projects"
