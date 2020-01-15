

# ansible-playbooks


Personal Ansible Playbooks (<i><small>Work in progress</small></i>).

Playbooks included:

- Docker: Installation and service enabling of Docker
- Kubernetes: Installation and service enabling of Kubernetes Master/Worker
- NTP: Installation, configuration of a NTP (Network Time Protocol) server
- Swap: Enable or Disable swap space

## Executing a Playbook

### Configuration for a node

#### Variables for all playbooks:

- <b>with_ntp (boolean):</b>  Indicates if NTP should be installed and enabled
- <b>with_swap (boolean):</b>  Indicates if SWAP tasks should be considered
- <b>swap_status ("off"/"on"):</b>  Indicates if SWAP should be enabled or disabled
- <b>with_docker (boolean):</b>  Indicates if DOCKER should be installed and enabled
- <b>with_kubernetes (boolean):</b>  Indicates if KUBERNETES should be installed
- <b>is_k8_master (boolean):</b>  Indicates if the node should act as a KUBERNETES master
- <b>is_k8_worker (boolean):</b>  Indicates if the node should act as a KUBERNETES worker

#### Example environment configuration:
staging.yml:
```yaml
[all]
master ansible_host=192.168.0.10 ansible_user=YOUR_USER ansible_python_interpreter=/usr/bin/python3
node ansible_host:192.168.0.11 ansible_user=YOUR_USER ansible_python_interpreter=/usr/bin/python3
```

the master host should be the first in the list, worker nodes should proceed.


<br />

master.yml
```yaml
- hosts: master
  roles:
  - docker
  - ntp
  - swap
  - kubernetes
  vars:
  - with_ntp: true
  - with_swap: true
  - swap_status: "off"
  - with_docker: true
  - with_kubernetes: true
  - is_k8_master: true
  - is_k8_worker: false
```


node.yml
```yaml
- hosts: master
  roles:
  - docker
  - ntp
  - swap
  - kubernetes
  vars:
  - with_ntp: true
  - with_swap: true
  - swap_status: "off"
  - with_docker: true
  - with_kubernetes: true
  - is_k8_master: false
  - is_k8_worker: true
```

#### Execute the paybook with

``` ansible-playbook -i staging site.yml -K ```

<i><small>** -K indicates to ask for sudo password</small></i>


Executing the playbook will create a master node with <b>pod scheduling enabled</b> this is not recommended for security reasons.
