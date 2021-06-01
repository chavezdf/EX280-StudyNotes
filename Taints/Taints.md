NODE TAINTS					Last Updated: 10-Mayo-2021
============

TAINTS
------
Permiten a un nodo rechazar pods. El nodo evita con esto que se le puedan programar (schedule) ciertos pods.
Se especifican en el "NodeSpec" del nodo

Un  taint está compuesto de:
	- key: string de 253 caracteres. Debe empezar por letra o nº y luego los caracteres: letra, nº, punto, guión y guión bajo. 

	- value: string de 63 caracteres. Debe empezar por letra o nº y luego los caracteres: letra, nº, punto, guión y guión bajo. 
		
	- effect: puede ser uno de los 3 siguientes:

		* NoSchedule: Los nuevos pods no se programan en el nodo. Los que ya estaban antes de poner el taint, se quedan.

		* PreferNoSchedule: Los nuevos pods se prefieren no programar en el nodo, se intentan en otro nodo. Los que ya estaban 
			antes de poner el taint, se quedan.

		* NoExecute: Los nuevos pods no se programan en el nodo. Los que ya estaban antes de poner el taint, se evacúan.

* Poner un taint a un nodo:

	oc adm taint nodes <nodo> <key>=<value>:<effect>

* Quitar un taint a un nodo:

	oc adm taint nodes <nodo> <key>-


TOLERATIONS
-----------
Forma que tiene un pod de "saltarse" un taint de un nodo.
Se especifican en el "PodSpec" del pod.

* Añadir un toleration a un pod:

 Se edita la especifición del pod y se incluye un bloque "tolerations" que llevará key, value, effect y operator;
 donde operator puede ser:
	- Equal: (valor por defecto). Los parámetros key, value y effect deben coincidir.

	- Exists: Los parámetros de key y effect deben coincidir. El valor de value se deja en blanco para que encaje con todos.

 Ejemplo de un toleration en un pod:

tolerations:
- key: "key1" 
  operator: "Equal" 
  value: "value1" 
  effect: "NoExecute" 
  tolerationSeconds: 3600 

* Como un toleration encaja con un taint:

	- Si el parámetro operator es "Equal": los parámetros key, value y effect deben ser los mismos

	- Si el parámetro operator es "Exists": los parámetors key y effect deben ser los mismos.

* Eliminar un toleration de un pod:

  Se edita la especifición del pod y se elimina el bloque "tolerations".



REFs: 
-----

 Taints: Expectations vs. Reality:

	https://www.openshift.com/blog/taints-expectations-vs.-reality

 Controlling pod placement using node taints:

	https://docs.openshift.com/container-platform/4.5/nodes/scheduling/nodes-scheduler-taints-tolerations.html

	https://docs.openshift.com/container-platform/4.5/cli_reference/openshift_cli/administrator-cli-commands.html#taint

 Pod Evictions based on Taints/Tolerations:

	https://www.openshift.com/blog/pod-evictions-based-on-taints-tolerations
