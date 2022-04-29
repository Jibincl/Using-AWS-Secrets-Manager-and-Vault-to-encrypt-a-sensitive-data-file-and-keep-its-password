# Using-AWS-Secrets-Manager-and-Ansible-Vault-to-encrypt-an-ansible-yaml-file-and-keep-its-password
 

## Description

In this tutorial, we are going to discuss how to use "AWS Secrets Manager" and "Ansible vault" to encrypt an ansible yaml file which contains sensitive data like passwords. 


So, here im going to encrypt a file named main.yml. Lets get into it.

main.yml

~~~
---

- name: "create docker image "
  hosts: build
  become: true
  vars:
    packages:
      - git
      - pip
      - docker
    repo_url: "https://github.com/Jibincl/devops-flask.git"
    repo_dir: "/home/ec2-user/flaskapp/"
    docker_user: "jibincl"
    docker_password: "dEe5SH***B58ezt"
    image_name: "jibincl/flaskrep"

  tasks:
    - name: " installing packages"
      yum:
        name: "{{ packages }}"
        ..
        ..
        
~~~

### Encrypting the file using ansiblevault

~~~
# ansible-vault encrypt main.yml
New Vault password:
Confirm New Vault password:
Encryption successful
~~~

I have encrypted the file with password "jibin@123"

Now, the file will be encrypted and wont be able to visible.

~~~
cat main.yml
$ANSIBLE_VAULT;1.1;AES256
65633835316634636433306439306563666534383636393838666436633866623165313735646231
3436666238373733353635616435373639633463633961370a666139313333643365366166653961
63346131343065616133376335643330353938653665616231363662643338636561396265656239
3966613831323864330a353963653230386164316262653861393461353166653638616139633230
39343661313931376533636431336139303563303530396232336364353534386135613164376564
3832323439633638373264653831656332303536313037626665
~~~

We wont be able to run the ansible playbook without the password now.

~~~
ansible-playbook -i hosts main.yml
ERROR! Attempting to decrypt but no vault secrets found
~~~

Now, we need to keep the password safe in AWS Secrete Manager.
