# 🚀 Obligatorio 2025 - Taller de Servidores Linux

Este repositorio contiene la configuración automatizada con Ansible para desplegar y configurar servidores web en entornos Linux (Ubuntu y CentOS).

## 📋 Requisitos previos

Antes de comenzar, asegúrate de tener:

- ✅ Ansible instalado (versión 2.9 o superior)
- ✅ Acceso SSH a los servidores de destino
- ✅ Colección ansible.posix instalada
- ✅ Permisos de sudo en los servidores de destino

## 🛠️ Estructura del proyecto

```
obligatorio2025/
├── collections/         # Colecciones requeridas
├── files/               # Archivos estáticos y documentación
│   └── images/          # Imágenes para documentación
├── inventories/         # Archivos de inventario
├── playbooks/           # Playbooks específicos
├── templates/           # Plantillas para configuraciones
└── README.md            # Este archivo
```

## ⚙️ Configuración

1. Clona este repositorio:
   ```bash
   git clone https://github.com/MV288481/obligatorio2025.git
   cd obligatorio2025
   ```

2. Instala las colecciones requeridas:
   ```bash
   ansible-galaxy collection install -r collections/requirements.yml
   ```

3. Verifica la conectividad con los servidores:
   ```bash
   ansible -i inventories/inventory.ini all -m ping
   ```

## 🚀 Ejecución

### Comandos ad-hoc

```bash
# Verificar uptime en todos los servidores
ansible -i inventories/inventory.ini all -m command -a "uptime"

# Instalar apache en servidores web
ansible -i inventories/inventory.ini webserver -m dnf -a "name=httpd state=present" --become --ask-become-pass

# Verificar espacio en disco en servidores Ubuntu
ansible -i inventories/inventory.ini ubuntu -m command -a "df -hT"
```

### Playbooks

```bash
# Configurar servidor web Apache
ansible-playbook -i inventories/inventory.ini playbooks/web_setup.yml --become --ask-become-pass

# Aplicar medidas de seguridad en Ubuntu
ansible-playbook -i inventories/inventory.ini playbooks/hardening.yml --become --ask-become-pass
```

## 📝 Funcionalidades

- 🔒 **Hardening de seguridad**: Configuración de UFW en Ubuntu
- 🌐 **Servidor web**: Instalación y configuración de Apache
- 🔄 **Virtualhost**: Configuración de virtualhost para www.ejemplo.com.uy
- 📄 **Página personalizada**: Despliegue de página HTML con información del servidor

## 👥 Autores

- Sol Varietti - 288481
- Caetano Rodriguez - 241102

## 📄 Licencia

Este proyecto está bajo la Licencia Creative Commons CC0 1.0 Universal.
