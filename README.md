hosts.ini
[web]
localhost ansible_connection=local

[db]
localhost ansible_connection=local

[lb]
localhost ansible_connection=local

setup.yml
---
- name: Print message on web server
  hosts: web
  tasks:
    - debug:
        msg: "Hello from Web Server"

- name: Print message on DB server
  hosts: db
  tasks:
    - debug:
        msg: "Hello from Database Server"

- name: Print message on Load Balancer
  hosts: lb
  tasks:
    - debug:
        msg: "Hello from Load Balancer"
output:
ansible-playbook -i hosts.ini setup.yml
TASK [debug]
ok: [localhost] => {
    "msg": "Hello from Web Server"
}


sudo apt update
sudo apt install ansible -y

nano hosts.ini

[webservers]
localhost ansible_connection=local

nano setup-web.yml 

---
- name: Setup web server
  hosts: webservers
  become: yes  # run as root
  tasks:
    - name: Install Apache web server
      apt:
        name: apache2
        state: present
      when: ansible_os_family == "Debian"

    - name: Start Apache service
      service:
        name: apache2
        state: started
        enabled: yes
      when: ansible_os_family == "Debian"


ansible-playbook -i hosts.ini setup-web.yml
