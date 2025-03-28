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

2. install pypy Python si no lo tienes instalado:

    ```bash
    ansible-playbook -i inventory.ini install_pypy.yaml
    ```

3. Ejecuta el playbook:
 
    ```bash
    ansible-playbook -i inventory.ini playbook_update_k3s_api_vip.yml
    ```



Esto actualizará el archivo `k3s.yaml` en los nodos master y cambiará el endpoint del API por la nueva IP VIP.

## Contribuciones

Si deseas contribuir, por favor haz un fork del repositorio y envía un pull request.
