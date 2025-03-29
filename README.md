# update_k3s_api_vip_ansible

Este repositorio contiene un playbook de Ansible para actualizar la IP del endpoint del API de K3s en los nodos master de un clúster de Kubernetes.

## Requisitos

- Tener Ansible instalado en el sistema donde se ejecutará el playbook.
- Acceso SSH a los nodos del clúster con privilegios adecuados.
- Clúster de Kubernetes K3s en funcionamiento.

## Uso

1. Clona el repositorio:
    ```bash
    git clone https://github.com/vhgalvez/update_k3s_api_vip_ansible.git
    cd update_k3s_api_vip_ansible
    ```

2. Ejecuta el playbook:
 
    ```bash
    ansible-playbook -i inventory.ini playbook_update_k3s_api_vip.yml
    ```

# Configuración manual

Si prefieres no usar Ansible, puedes actualizar la IP del endpoint del API de K3s manualmente en cada nodo master. Para ello, sigue estos pasos:

```bash
sudo sed -i 's|server: https://127.0.0.1:6443|server: https://10.17.5.10:6443|' /etc/rancher/k3s/k3s.yam
```



Esto actualizará el archivo `k3s.yaml` en los nodos master y cambiará el endpoint del API por la nueva IP VIP.

## Contribuciones

Si deseas contribuir, por favor haz un fork del repositorio y envía un pull request.
