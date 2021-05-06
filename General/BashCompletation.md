ACTIVAR BASH COMPLETION PARA EL COMANDO OC
=============================================

**En un momento puntual:**
````
$ oc completion bash > oc_completion.sh

$ source oc_completion.sh
````

**Disponible para todos los usuarios del sistema:**

````
$ oc completion bash > /etc/profile.d/oc_completion

$ echo 'source oc_completion' > /etc/profile.d/oc_completion.sh

$ chmod +x /etc/profile.d/oc_completion.sh
````

**Disponible sólo para un usuario del sistema:**

````
$ oc completion bash > ~/oc_completion.sh

$ chmod +x ~/oc_completion.sh

$ echo "source ~/oc_completion.sh" >> ~/.bashrc

$ source ~/.bashrc
````
