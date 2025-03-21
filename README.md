Despliegue Automatizado de Docker y Mario Bros con Ansible
Este proyecto utiliza Ansible para automatizar la instalación de Docker y el despliegue de un contenedor con el juego Mario Bros en una máquina virtual remota en Azure.

1️⃣ Estructura del Proyecto
📂 training-ansible/
├── inventory/ → Archivo hosts.ini con la IP de la VM
├── playbooks/
│ ├── install_docker.yml → Instala Docker
│ ├── run_container.yml → Ejecuta el contenedor de Mario Bros
├── roles/
│ ├── docker_install/ → Contiene tareas para instalar Docker
│ ├── docker_container/ → Contiene tareas para ejecutar el contenedor
└── ansible.cfg → Configuración de Ansible

2️⃣ Configuración de la Máquina Virtual
En el archivo inventory/hosts.ini, se define la VM donde se ejecutarán los playbooks:


[azure_vm]
vm_ip ansible_user=admin ansible_ssh_pass=pass
📌 Nota: vm_ip debe reemplazarse con la dirección IP real de la VM.

3️⃣ Instalación de Docker con Ansible
El playbook install_docker.yml instala Docker en la VM:


- hosts: azure_vm
  become: yes
  roles:
    - docker_install
📌 Dentro del rol docker_install, se realizan los siguientes pasos:
✅ Instalación de dependencias
✅ Agregar la clave GPG de Docker
✅ Configuración del repositorio de Docker
✅ Instalación del paquete docker-ce

4️⃣ Despliegue del Contenedor de Mario Bros
El playbook run_container.yml ejecuta el contenedor del juego:


- hosts: azure_vm
  become: yes
  roles:
    - docker_container
📌 Dentro del rol docker_container, se usa el módulo docker_container para:
✅ Descargar la imagen pengbai/docker-supermario:latest
✅ Ejecutar el contenedor en el puerto 8787

5️⃣ Ejecución de los Playbooks
Para ejecutar los playbooks, se usaron los siguientes comandos:

1️⃣ Instalar Docker en la VM


ansible-playbook -i inventory/hosts.ini playbooks/install_docker.yml
2️⃣ Ejecutar el contenedor de Mario Bros


ansible-playbook -i inventory/hosts.ini playbooks/run_container.yml
6️⃣ Configuración de Seguridad en Azure
Para acceder al juego, se configuró una regla de seguridad en Azure para permitir tráfico en el puerto 8787:


security_rule {
    name                       = "mario_bros_rule"
    priority                   = 110
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "8787"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
}
7️⃣ Acceder al Juego
Una vez que el contenedor está en ejecución y la regla de seguridad está configurada, se puede acceder al juego desde un navegador web con la URL:

http://<VM_IP>:8787
🚀 ¡Listo! Ahora puedes jugar Mario Bros en tu máquina virtual desplegada con Ansible. 🎮#   S O F T 5  
 