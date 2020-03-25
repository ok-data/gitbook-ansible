# 설정

|host   | ip  | 역할|
|---|---|---|
| centos-namenode-1   |  10.10.30.101 | ansible-server |
| centos-namenode-2 | 10.10.30.102|ansible-node|
| centos-datanode-1 | 10.10.30.103|ansible-node|
| centos-datanode-2 | 10.10.30.104|ansible-node|
| centos-datanode-3 | 10.10.30.105|ansible-node|



## 호스트 추가

```bash
vim /etc/ansible/hosts
```
```
...
centos-namenode-2
centos-datanode-1
centos-datanode-2
centos-datanode-3
```

> `platform_system is no defined` 에러 발생 시 `pip install -U setuptools`을 실행한다.

## ping 테스트

호스트간의 ssh 인증키는 교환된 상태이다.

```bash
ansible all -m ping
# centos-datanode-1 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
# centos-datanode-2 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
# centos-datanode-3 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
# centos-namenode-2 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
```

## hosts에 alias

```bash
vim /etc/ansible/hosts
```
```
...
[nginx]
centos-datanode-1
centos-datanode-2
centos-datanode-3
```

`nginx` 호스트에만 ping

```bash
ansible nginx -m ping
# centos-datanode-3 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
# centos-datanode-1 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
# centos-datanode-2 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": false,
#     "ping": "pong"
# }
```