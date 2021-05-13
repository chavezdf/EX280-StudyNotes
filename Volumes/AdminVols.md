Objetivo: Crear un PV.
  
  Que es un PV (Persistent Volume) 
  
Objetivo: Enlazar un PV con un PVC (Persisten Volume Claim).
  
  oc set volumes <deployment o dc> --add --type pvc --claim-size --claim-mode <rwo> --claim-name <nombre> --mount <path>
