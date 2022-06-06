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