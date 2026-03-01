# TallerFebrero2026 - Infraestructura con automatizacion Ansible

El repositorio contiene playbooks de Ansible para desplegar y configurar:

- Hardening (Actualizaciones de paquetes)
- Servidor NFS (CentOS)
- Cliente NFS (Ubuntu)
- Configuracion de Firewall en Ubuntu (UFW)
- Aplicacion de servidor WEB

En el repositorio estan incluidos cuatro nodos y un host para el manejo de ansible:


|   HOST   |                   OS                   |             Rol              |        IP        |
|----------|----------------------------------------|------------------------------|------------------|
| Bastion  | CentOS Stream 9 con GUI                | Equipo de control de Ansible | 192.168.10.1/24  |
| Centos01 | CentOS Stream 9 con instalacion minima | Servidor NFS                 | 192.168.10.11/24 |
| Centos02 | Centos Stream 9 con instalacion minima | Secundario                   | 192.168.10.12/24 |
| Ubuntu01 | Ubuntu 24.04.4 LTS                     | Cliente NFS                  | 192.168.10.21/24 |
| Ubuntu02 | Ubuntu 24.04.4 LTS                     | Secundario                   | 192.168.10.22/24 |

Todas las maquinas cuentan ademas con conexion directa a internet.

## PRE-REQUISITOS:

- Python3 instalado en el host y en los nodos
```bash
ssh user@host
```
```bash
dnf install python3
```
```bash
exit
```
- Ansible instalado en host
```bash
dnf install ansible-core
```
- Git instalado en host
```bash
dnf install git
```
- Servicio SSH instalado en host
- Servicios SSH instalados en los nodos
- Privilegios SUDO en los nodos
- Llave publica del host creada, agregada en "authorized_keys" en todos los nodos y agregada en GitHub (GitHub account settings -> SSH and GPG keys)
```bash
ssh-keygen -t ed25519
```
```bash
ssh-copy-id user@host
```

## INSTALACION:

- Clonacion de repositorio
```bash
git clone < URL de GitHub (SSH) >
```
```bash
cd TallerFebrero2026/
```

### Estructura del repositorio:

```Estructura del repositorio
.
├── inventories/
│   ├── hosts.ini
│   └── group_vars
│       └── linux.yaml
├── playbooks/
│   ├── hardening.yaml
│   ├── nfs-server.yaml
│   ├── nfs-client.yaml
│   ├── ubuntu-ufw.yaml
│   └── webserver.yaml
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
└── site.yaml
```

### Instalacion de modulos de ansible requeridos:
```bash
ansible-galaxy collection install -r collections/requirements.yaml
```

### Ejecucion de playbooks:
```bash
ansible-playbook -i inventories/hosts.ini site.yaml --ask-become-pass
```
```bash
< Contrasena para escalado de permisos >
```

El archivo "site.yaml" ejecutara los playbooks en el siguiente orden:
1. hardening.yaml
2. nfs-server.yaml
3. nfs-client.yaml
4. ubuntu-ufw.yaml
5. webserver.yaml

Todos los playbooks son idempotentes y requieren de autenticacion SSH.

Si su entorno cuenta con direcciones y cantidad de nodos diferentes, editar "hosts.ini" y reemplazar los valores que considere necesarios:
```bash
vim inventories/hosts.ini
```

---
Autor: Santiago Casavalle (334480)
---
Profesor: Enrique Verdes
---
Fecha: 01/03/2026
---
