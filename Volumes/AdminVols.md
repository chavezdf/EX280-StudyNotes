Que es un Volumen Persistente?


Que es una Solicitud de Volumen Persistente?


**Gestion de Volumes Persistentes:**

    oc get pv
    oc get pv <nom_pv> - o yaml
    oc create -f <nom_pv_file>.yaml

**Gestion de Volumenes Persistente:**

    oc get pvc
    oc create -f <nom_pvc_file>.yaml
    oc delete pvc/<nom_pvc>

**Objetivo: Crear un PV.**
  
    oc create -f <nom_pv_file>.yaml
  
**Objetivo: Crear un PVC (Persisten Volume Claim).**
  
    oc set volumes <deployment o dc> 
    --add 
    --type pvc 
    --claim-name <nom_claim> 
    --claim-size <pvc_size> 
    --claim-mode <pvc_mode> 
    --mount <mount_path>

> **pvc_mode**: (RWX "Read **R** Write **W** Many **X**",ROX "Read **R** Only **O** Many **X**",RWO Read **R** Write **W** Many **X**) 
