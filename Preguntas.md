Como crear un proyecto

Como crear una aplicacion con S2I?
**Usando un Branch**
**Usando un sub-directorio*

Listar Recursos 

pods
build config
deploy
deploy config
todos por selector
$ oc get all --selector app=nbviewer -o name


Eliminar Recursos
proyecto
aplicacion
todos los recursos con un selector
todos los recursos con un label

Cree un proyecto y no le asigne un label requerido
Quiero cambiar el nombre de la app
Quiero cambiar el nombre de un proyecto
Quiero cambiear el image stream 
Como editar un build config.

He creado un Proyecto y no agregue la descripcion, como lo agrego sin borrar el proyecto.

oc edit <nom-proyecto>
  
Se edita con vi el campo de descripcion y se guarda.

Cuando en un build no se expone el servicio, se puede crear manual:

oc expose dc/<Nom-Deploymentconfig> --port=<port> --target-port=<pod-port>
  
Como inyectarle o crear una variable de entorno en un POD:

Se realiza desde un deployment config para que persista. Ya que el POD se crea a partir de esa configuracion.

oc set env dc/<nom-deploymentconfig> RESPONSE="<valor>"
  
**Como aislar las redes de los proyectos por defecto?**

Se debe crear una politica de RED que se agregara al template de creacion de proyectos para que cada vez que se crea un proyecto tenga la politica generada. En esa politica se debe permitir el accesso de los operators de **monitoreo** e **ingress**.
  
  
