
# Update K3s API VIP with Ansible
This Ansible playbook updates the K3s API VIP address in a K3s cluster. It is designed to be run on a control node that has access to the K3s cluster.

## clone the repository



```bash
git clone https://github.com/vhgalvez/update_k3s_api_vip_ansible.git
cd update_k3s_api_vip_ansible

```
## Install Ansible


```bash
sudo ansible-playbook -i inventory.ini update_k3s_api_vip.yaml
```
