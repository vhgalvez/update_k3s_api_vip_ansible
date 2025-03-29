# update_k3s_api_vip_ansible 

Este repositorio contiene un playbook de Ansible para actualizar la IP del endpoint del API de K3s en los nodos master de un clúster de Kubernetes, permitiendo la configuración de un Virtual IP (VIP) para alta disponibilidad.

## Requisitos

- Tener **Ansible** instalado en el sistema donde se ejecutará el playbook.
- Acceso **SSH** a los nodos del clúster con privilegios adecuados (root o sudo).
- Un clúster de **Kubernetes K3s** en funcionamiento.
- Tener configurada la **IP Virtual (VIP)** que se utilizará como endpoint del API de K3s.

## Uso

### 1. Clona el repositorio:

```bash
git clone https://github.com/vhgalvez/update_k3s_api_vip_ansible.git
cd update_k3s_api_vip_ansible
```

### 2. Edita la configuración (opcional):

Abre el archivo `inventory.ini` y personaliza las IPs y nodos según tu infraestructura. Si necesitas cambiar las rutas de los archivos de configuración de K3s, puedes modificar las siguientes variables en el playbook:

- `k3s_service_path`: Ruta del archivo de servicio de K3s (`/etc/systemd/system/k3s.service`).
- `agent_service_path`: Ruta del archivo de servicio de los agentes de K3s (`/etc/systemd/system/k3s-agent.service`).
- `kubeconfig_path`: Ruta del archivo de configuración de Kubeconfig (`/etc/rancher/k3s/k3s.yaml`).

### 3. Ejecuta el playbook:

```bash
ansible-playbook -i inventory.ini playbook_update_k3s_api_vip.yml
```

Este playbook actualizará la configuración de K3s en los nodos master y agentes, asegurando que todos apunten al nuevo VIP como endpoint del API.

### Tareas principales del playbook:

1. **Asignar la IP VIP en el primer nodo master**: Si la IP VIP no está configurada en el primer nodo master, se agrega a la interfaz `eth0`.
2. **Actualizar la configuración TLS (SAN)**: Se añade la IP VIP a los parámetros `--tls-san` en el servicio de K3s para asegurar que el certificado TLS sea válido para esa IP.
3. **Limpiar datos previos**: Se detiene el servicio de K3s y se eliminan los datos antiguos de certificados y etcd para evitar posibles conflictos.
4. **Actualizar el archivo de configuración `k3s.yaml`**: Se actualiza la configuración de K3s para que apunte al nuevo VIP en el archivo `k3s.yaml`.
5. **Actualizar el endpoint de los agentes**: Se actualiza el archivo de servicio de los agentes de K3s para que apunten al nuevo VIP.
6. **Reiniciar los servicios de K3s**: Se reinician los servicios de K3s y `k3s-agent` en los nodos correspondientes, con verificación para asegurar que los servicios se reinicien correctamente.
7. **Validar la configuración**: Se verifica que los certificados se hayan regenerado correctamente y que el nuevo VIP esté funcionando al consultar el API.

## Configuración manual

Si prefieres no usar Ansible, puedes actualizar la IP del endpoint del API de K3s manualmente en cada nodo master. Para ello, sigue estos pasos:

1. **Actualiza el archivo `k3s.yaml` en los nodos master**:

```bash
sudo sed -i 's|server: https://127.0.0.1:6443|server: https://10.17.5.10:6443|' /etc/rancher/k3s/k3s.yaml
```

2. **Actualiza el archivo de servicio de K3s para que utilice el nuevo VIP**:

```bash
sudo sed -i 's|--server https://127.0.0.1:6443|--server https://10.17.5.10:6443|' /etc/systemd/system/k3s.service
```

3. **Reinicia el servicio de K3s**:

```bash
sudo systemctl daemon-reload
sudo systemctl restart k3s
```

Esto actualizará el archivo `k3s.yaml` y el servicio de K3s en los nodos master, cambiando el endpoint del API por la nueva IP VIP.

## Contribuciones

Si deseas contribuir al proyecto, por favor haz un fork del repositorio y envía un pull request. Asegúrate de seguir las mejores prácticas de codificación y de documentar tus cambios adecuadamente.

## Licencia

Este proyecto está bajo la licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.
