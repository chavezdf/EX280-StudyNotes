
CAMBIAR LA SCC DE UN POD			Last Update: 4-Octubre-2020
========================

Cuando un proyecto se crea con el comando "oc new-project" o usando la consola Web de OpenShift, se crean en ese proyecto 
3 Service Account:

	deployer-> usada por el pod de despliegue
	builder -> usada por el pod de contrucción
	default -> usada por el resto de pods que se creen en el proyecto

La SA default tiene asociada la SCC restricted.

En ocasiones, un pod necesita otra SCC, p.e. anyuid, entonces hay que cambiar la Service Account "default" por otra que tenga asociada la SCC necesaria.

Pasos:

1.- Veo cual es la SCC que necesita el pod: (con usuario con privilegios de "cluster-admin")

	oc get pod <nombre_pod> -o yaml | oc adm policy scc-subject-review -f -

2.- Creo la SA para el pod: (con usuario regular)

	oc create sa <nombre_sa>

3.- Hago que la SA recien creada use la SCC que necesita el pod: (con usuario con privilegios de "cluster-admin")

	oc adm policy add-scc-to-user <nombre-scc> -z <nombre_sa>

4.- Modifico el pod para que use la SA nueva: (con usuario regular)

  -> Si el pod se desplegó con un Deployment:

	oc set serviceaccount deployment <nombre_deployment> <nombre_sa>

  -> Si el pod se desplegó con un DC:

	oc set serviceaccount dc <nombre_dc> <nombre_sa>

  -> Si el pod se desplegó directamente:

	oc set serviceaccount pod <nombre_pod> <nombre_sa>
