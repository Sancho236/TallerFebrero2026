# TallerFebrero2026 - Infraestructura con automatizacion Ansible

El repositorio contiene playbooks de Ansible para desplegar y configurar:

- Hardening (Actualizaciones de paquetes)
- Servidor NFS (CentOS)
- Cliente NFS (Ubuntu)
- Configuracion de Firewall en Ubuntu (UFW)
- Aplicacion de servidor WEB

En el repositorio estan incluidos cuatro nodos y un host para el manejo de Ansible (Bastion)

Bastion (CentOS con GUI)
- Rol: Equipo de control de Ansible
- IP: 192.168.10.1

Centos01 (CentOS Stream 9 con instalacion minima)
- Rol: Servidor NFS
- IP: 192.168.10.11

Centos02 (CentOS Stream 9 con instalacion minima)
- Rol: Secundario
- IP: 192.168.10.12

Ubuntu01 (Ubuntu 24.04.4 LTS)
- Rol: Cliente NFS
- IP: 192.168.10.21

Ubuntu02 (Ubuntu 24.04.4 LTS)
- Rol: Secundario
- IP: 192.168.10.22

Todas las maquinas cuentan ademas con conexion directa a internet.

PRE-REQUISITOS:

- Ansible instalado en host
  + dnf install ansible-core
- Git instalado en host
  + dnf install git
- Servicio SSH instalado en host
- Servicios SSH instalados en los nodos
- Privilegios SUDO en los nodos
- Llave publica del host creada, copiada en todoslos nodos y subida a GitHub
  + ssh-keygen -t ed25519
  + ssh-copy-id user@host

INSTALACION:

- Clonacion de repositorio
  + git clone < URL de GitHub (SSH)>
  + cd TallerFebrero2026/

Estructura del repositorio:

.
├── inventories/
│   └── hosts.ini
├── playbooks/
│   ├── hardening.yml
│   ├── nfs-server.yml
│   ├── nfs-client.yml
│   ├── ubuntu-ufw.yml
│   └── webserver.yml
├── collections/
│   └── requirements.yaml
├── templates/
│   └── README-NFS.j2
├── files/
│   ├── auto.nfs
│   ├── nfs.autofs
│   └── shared-http.service
├── LICENSE
├── README.md
├── ansible
└── site.yaml

- Instalacion de modulos de ansible requeridos:
  + ansible-galaxy collection install -r collections/requirements.yaml

- Ejecucion de playbooks:
  + ansible-playbook -i inventories/hosts.ini site.yaml --ask-become-pass
  + < Contrasena para escalado de permisos >

Todos los playbooks son idempotentes y requieren de autenticacion SSH.

- Si su entorno cuenta con direcciones y cantidad de nodos diferentes, editar "hosts.ini" y reemplazar los valores que considere necesarios:
  + vim inventories/hosts.ini
