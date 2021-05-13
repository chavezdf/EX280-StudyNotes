**OC SET** Ayuda a realizar cambios a recursos existentes

    oc set env {deployment ó dc} --from secret/{nom_secret} --prefix PREFIX
> Agrega varaibles de entorno a un deployment o deployment-config

    oc set volumes {deployment ó dc} --add --name {pvc_name} --type pvc --claim-size {pvc_size} --claim-mode {mode} --mount-path {path}
> Agrega configuracion de solicitud de volumenes en un deployment o deployment-config

    oc set data secret/{nom_secret} --from-file htpasswd={path_secret_file} -n openshift-config
> Modifica la data de un secret
