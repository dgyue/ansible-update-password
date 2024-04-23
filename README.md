# ansible-update-password
## 部署示例
清单文件如下所示:
```shell
#测试环境服务器组
[test_servers_group]
192.168.1.1 
192.168.1.2
[all:vars]
ansible_ssh_user=root
ansible_ssh_pass='password'
ansible_ssh_port=22
#ansible_become=true
#ansible_become_pass='password'
```
执行文件如下所示:
```shell
---
- hosts: test_servers_group #需要和清单文件中的名称对应
  gather_facts: false
  tasks:
    - name: update user passwd
      user:
        name: "{{ item.name }}"
        password: "{{ item.chpass | password_hash('sha512') }}"
        update_password: always
      with_items:
        - { name: 'test', chpass: 'password'} #根据实际情况进行修改
```
使用以下命令进行更新
```shell
ansible-playbook -i hosts update_password.yml
```