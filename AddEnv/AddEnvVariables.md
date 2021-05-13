Mostrar variables que aparecen en la configuracion

    oc set env <d ó dc> <nom_deplyment> --list
    

Las variables de entorno se inyectan en este orden:

* Las de la IMAGE del Contenedor
* Las generadas por Open Shift
* Las ingresadas en el Deployment ó Deployment Config
> OJO: Esto a considerar cuando existen varias variables con el mismo nombre.
