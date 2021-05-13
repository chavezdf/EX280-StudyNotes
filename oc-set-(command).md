**OC SET** Ayuda a realizar cambios a recursos existentes
=========================================================

**oc set env** Agregar variables de entorno a un deployment o deployment-config

    oc set env {deployment ó dc} {key}={value} --prefix PREFIX
    oc set env {deployment ó dc} --from secret/{nom_secret} --prefix PREFIX
    oc set env {deployment ó dc} --from configmap/{nom_config_map} --prefix PREFIX

**oc set volume** Agregar solicitud de volumen en deplyment ó deployment config

    oc set volume {deployment ó dc} --add --type pvc --name {pvc_name} --claim-name {nom_claim}  --claim-size {pvc_size} --claim-mode {mode} --mount-path {path}

**oc set data** Agregar data a un secret ya creado.

    oc set data secret/{nom_secret} --from-file htpasswd={path_secret_file} -n openshift-config
    oc set data secret/{nom_secret} {key}={value} -n openshift-config

