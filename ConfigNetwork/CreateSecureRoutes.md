RUTA EDGE
=========
La comunicación va encriptada del cliente hasta el router de OpenShift.

**Pasos:**

1.- Crear la ruta securizada edge:
```
$ oc create route edge <nombre-ruta> --service <nombre-servicio> --hostname <URL>
```
2.- Extraer el certificado del router de OpenShift del operator "openshift-ingress-operator":
```
$ oc extract secrets/router-ca --keys tls.crt -n openshift-ingress-operator
```
3.- Probar la ruta desde fuera del cluster con:
````
$  curl -I -v --cacert tls.crt https://<URL>
````
RUTA PASS-THROUGT
=================

La comunicación va encriptada del cliente hasta el pod de la app.

**Pasos:**

1.- Generar la clave privada (fichero .key) para el cerficado firmado CA:
````
$ openssl genrsa -out <fichero.key> 2048
````
2.- Generar el fichero .csr (petición de firma de certificado) usando el fichero .key del paso anterior:
````
$ openssl req -new -subj "/C=ES/ST=Madrid/L=Getafe/O=CFTIC/CN=<nombre-ruta>.<dominio-openshift>" -key <fichero.key> -out <fichero.csr>
````
3.- Generar el certificado firmado usando el fichero .key y el fichero .csr de los pasos anteriores:
````
$ openssl x509 -req -in <fichero.csr> -passin file:passphrase.txt -CA <fichero.pem> -CAkey <fichero.key> -CAcreateserial -out <fichero.crt> -days 1825 -sha256 -extfile <fichero.ext>
````
4.- Crear el secret para almacenar los certificados en el pod de la app con los ficheros .crt y .key de los pasos anteriores:
````
$ oc create secret tls <nombre-secret> --cert=<fichero.crt> --key=<fichero.key>
````
5.- Montar como volumen los ficheros del secret:
````
$ oc set volume dc/<nombre-dc> --add --type=secret --secret-name=<nombre-secret> --mount-path=/usr/local/etc/ssl/certs
````
6.- Crear la ruta pass-through:
````
$ oc create route passthrough <nombre-ruta> --service <nombre-servicio> --port <puerto> --hostname <URL>
````
7.- Probar la ruta desde fuera del cluster con:
````
$ curl -I -v --cacert <fichero.crt> https://<URL>
````
### ¿Cómo hacer que, si se tiene una ruta segura, al hacer http://<url-ruta-segura> se haga la redirección a https://<url-ruta-segura>?

Al crear la ruta segura con **"oc create route edge"** añadir el parámetro **--insecure-policy=Redirect**
Con esto, en el **"spec"** de la ruta veré, dentro del bloque **"tls"**, el atributo **"insecureEdgeTerminationPolicy"** con valor **"Redirect"**

> Refs.: oc explain route.spec.tls y
````
oc create route edge --help
````
