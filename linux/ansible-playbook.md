## yaml basic
```yaml
#pipe
include_newlines: |
  tpe some text
  second line
fold_newLInes: >
  type some text
  seconds line
```

## ansible-playbook command
```
# -i inventry
# -C dry-run check mode
ansible-playbook -i inv playbook.yaml -C
```

## understanding variables
vars in inventry
```
# file inv
localhost
# remote group
[remote]
# host server1
server1 ansible_host=<dns name or IP>
```

variables for remote group
```yaml
# group_vars/remote , similar for host_vars
work_dir: /home/ansible/working

service_list:
- httpd
- nfs
- maria_db

share_paths:
  nfs: /mnt/nfs
  difs: /mnt/cifs
```

using vars in playbooks
```yaml
---
- hosts: localhost
  tasks:
    - name: test
      file:
        name: "{{ working_dir }}"
        state: directory

  # for dict it is {{ share_paths['nfs'] }}
```

explicitly invoke with a var yaml file
```
ansible-playbook vars_play.yml -e @vars.yml
```

register var can hold the result
```yaml
- name: comand result
  command: ls -la /home/nfs
  register: cmd_output
```

variables can also be directly define in playbook
```yaml
# explicitly list the vars
vars:
  var1: xxx
# or provide the yaml file
vars_files:
  - vars.yml
tasks:
```


## templates
```yaml
# config.j2
name={{ code_name }}
version={{ version }}
```

typically used with template module
```yaml
tasks:
  - name: template example
    template:
      src: config.j2
      dest: /opt/config
```
notice there is a -validate option for template

## ansible facts
variables like below are facts vars
- ansible_hostname
- hostvars['localhost']

how facts are attained is configured in /etc/ansible/ansible.cfg
fact_caching = memory # by default
fact_caching = jsonfile/redis # can change to a file
note that it will create quite a lot of data, redis is the best practice

ansible setup module can build the facts, can run in a cron job periodically
```
ansible -i inv all -m setup
```

## conditional in ansible
when
## loops in ansible
example, item is the variable working with the loop
```yaml
tasks:
  - name: demo loop
    yum: 
      name: "{{ item }}"
      state: absent
    loop:
      - elinks
      - nmap-ncat
      - bind-utils
```

## handler in ansible
常用于改完配置文件，然后重启service用. 注意handler可以被多次notify (但实际handler只会跑一次，应该在最后)。 只有注册项有change了， handler才会被驱动
```yaml
---
- hosts: test
  become: yes
  tasks:
    - name: update configuration
      template:
        src: htppd.conf.j2
        dest: /etc/httpd/conf/httpd.conf
      notify: httpd service
  handlers:
    - name: httpd service
      service: 
        name: httpd
        state: restart
      listen: httpd service
```

## advance ansible inventry configuration
basic
```
[group_name]
server1
server2
```

dynamic inventry(bash / python) is executable, it outputs json. many of htem comes from cloud provider
**best practice**
生产用一个inv文件， 测试用另一个文件

## executing selective parts of ansible playbook
每一个task 都可以加 tags: 来标签

```
ansible-playbook <yaml file> --tags software,files
## opposite tags
ansible-playbook <yaml file> --skip-tags software
```
## working with sensitive data with Ansible Vault

decrypt
```
ansible-vault decrypt <file>
```
encrypt
```
ansible-vault encrypt <file>
```

## error handling
--limit + retyfile 会只在失败的机器上面跑
retryfile 只是一些server的列表，也可以用此方法执行对于部分server的运行

ignore_errors: yes 会让失败task继续往下走
