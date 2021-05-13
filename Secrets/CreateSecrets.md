**Objectivo:** Crear un Secret para agregar variable key=value

    oc create secret {tipo} {nom_secret} --from-literal {key}={value}
    
   > tipo: docker-registry, generic, tls
 

**Objetivo:** Usar key=value de un secret en un deployment o deployment-config

    oc set env <deployment o dc> --prefix PREFIX_ --from secret/<nom_secret>
    
   > --prefix: Para agregar a la variable el prefijo deseado.
