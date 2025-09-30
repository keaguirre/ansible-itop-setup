# itop-ansible

Este proyecto es una configuración de Ansible para implementar y gestionar la aplicación iTop. A continuación se describen los archivos y su propósito:

## Estructura del Proyecto

- **inventory.ini**: Contiene la configuración del inventario de Ansible, donde se definen los hosts y grupos de hosts que se gestionarán.

- **site.yml**: Es el playbook principal de Ansible. Define los plays que se ejecutarán en los hosts especificados en el inventario.

- **group_vars/itop.yml**: Contiene variables específicas para el grupo de hosts denominado "itop". Estas variables se pueden utilizar en los playbooks y roles.

- **roles/itop/tasks/main.yml**: Define las tareas que se ejecutarán en el rol "itop". Es el punto de entrada para las tareas de este rol.

- **roles/itop/templates/itop-apache-vhost.conf.j2**: Plantilla Jinja2 utilizada para generar la configuración del virtual host de Apache para la aplicación iTop.

- **roles/itop/templates/php.ini.d-itop.ini.j2**: Otra plantilla Jinja2 utilizada para generar un archivo de configuración de PHP específico para iTop.

- **roles/itop/handlers/main.yml**: Define los handlers que se pueden utilizar en el rol "itop". Los handlers son tareas que se ejecutan cuando son notificados por otras tareas.

### Estructura del proyecto en formato árbol

```
.
├── README.md
├── group_vars
│   └── itop.yml
├── inventory.ini
├── roles
│   └── itop
│       ├── handlers
│       │   └── main.yml
│       ├── tasks
│       │   └── main.yml
│       └── templates
│           ├── itop-apache-vhost.conf.j2
│           └── php.ini.d-itop.ini.j2
└── site.yml
```

### Requisitos
```
sudo apt install -y pipx git ansible-core
pipx install --include-deps ansible
pipx ensurepath
```

## Instrucciones de Uso

1. **Configuración del Inventario**: Modifique el archivo `inventory.ini` para definir sus hosts y grupos.

2. **Definición de Variables**: Ajuste las variables en `group_vars/itop.yml` según sea necesario para su entorno.

3. **Ejecución del Playbook**: Ejecute el playbook principal con el siguiente comando:
   ```bash
   ansible-playbook -i inventory.ini site.yml
   ```
   o
   ```bash
   sudo ansible-pull \
     -U https://github.com/keaguirre/ansible-test-itop.git \
     -C main \
     -d /etc/ansible-itop \
     -i /etc/ansible-itop/inventory.ini \
     /etc/ansible-itop/site.yml

   ```
   

4. **Personalización de Plantillas**: Modifique las plantillas en `roles/itop/templates/` según sus necesidades.

5. **Handlers**: Asegúrese de que los handlers en `roles/itop/handlers/main.yml` estén configurados para notificar las tareas adecuadas.

Este proyecto proporciona una base sólida para implementar y gestionar iTop utilizando Ansible.

## Fast ssh configuration for testing
```
# Instalar el servidor SSH
apt-get update && apt-get install -y openssh-server

# Habilitar y arrancar el servicio SSH
systemctl enable --now ssh

# Hacer un backup del archivo de configuración antes de modificarlo
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

# Descomentar y habilitar la autenticación por contraseña
sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# En caso de que la línea no exista, añadirla explícitamente
grep -q '^PasswordAuthentication' /etc/ssh/sshd_config || echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config

# Reiniciar el servicio para aplicar los cambios
systemctl restart ssh
hostname -I
```

# To connect external mysql db from railway
## El texto posterior a @ es el host y puerto
```bash
switchback.proxy.rlwy.net:45665
```
## Luego el user root y la password que te den
