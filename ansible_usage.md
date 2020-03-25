# ansible 명령

## 주요 옵션

| option  |  full name | 내용 | 
|---|---|---|
| -i  | --inventory-file  | 적용될 노드를 선택|
| -m | --module-name | 사용하는 모듈 |
| -k | --ask-pass | 암호를 물어보도록 설정 |
|    | --list-hosts |적용되는 노드들을 확인 |

### `-i` 옵션

적용될 노드를 선택하는 옵션 이다.

```bash
# 파일에 호스트 를 넣음
cat customized_inven.lst
# centos-datanode-1
# centos-datanode-2
```

```bash
ansible -i customized_inven.lst all -m ping
# centos-datanode-2 | SUCCESS => {
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
```

`customized_inven.lst`에 설정된 호스트에만 응답을 받는다.

`all` 대신에 `customized_inven.lst`에 설정한 host를 넣어도 된다.

```bash
ansible -i customized_inven.lst centos-datanode-1 -m ping
```

### `-k` 옵션

```bash
ansible all -m ping -k
# SSH password:
```

비밀번호를 요구하게 된다.

### `--list-hosts` 옵션

**해당 ansible 명령이 적용되는 노드들을 확인**할 수 있다.

```bash
ansible -i customized_inven.lst all -m ping --list-hosts
#   hosts (2):
#     centos-datanode-1
#     centos-datanode-2
```

### `-m` 옵션

사용할 module을 선택하는 옵션이다. [전체모듈](https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html)

#### `ping` : 서버와 노드간에 통신상태를 확인한다. `ansible all -m ping`

#### `shell` 

쉘 명령을 전달한다. `-a` 옵션으로 필요한 인자 값을 넣는다.

```bash
ansible all -m shell -a "uptime"
# centos-datanode-3 | CHANGED | rc=0 >>
#  17:06:51 up 3 days, 23:48,  1 user,  load average: 0.03, 0.08, 0.07
# centos-datanode-1 | CHANGED | rc=0 >>
#  17:06:51 up 3 days, 23:47,  1 user,  load average: 0.00, 0.01, 0.05
# centos-datanode-2 | CHANGED | rc=0 >>
#  17:06:52 up 3 days, 23:47,  1 user,  load average: 0.81, 0.55, 0.37
# centos-namenode-2 | CHANGED | rc=0 >>
#  17:06:52 up 3 days, 17:34,  1 user,  load average: 0.02, 0.11, 0.10
```

`-m shell` 은 생략해도 동작한다.

```bash
ansible all -a "free -h"
```

## module 활용하기

### user 모듈

계정 추가 :

```bash
ansible all -m user -a "name=foo"
# centos-namenode-2 | CHANGED => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": true,
#     "comment": "",
#     "create_home": true,
#     "group": 1006,
#     "home": "/home/foo",
#     "name": "foo",
#     "shell": "/bin/bash",
#     "state": "present",
#     "system": false,
#     "uid": 1006
# }
...생략
```

계정 삭제 :

```bash
ansible all -m user -a "name=foo state=absent"
# centos-namenode-2 | CHANGED => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python"
#     },
#     "changed": true,
#     "force": false,
#     "name": "foo",
#     "remove": false,
#     "state": "absent"
# }
... 생략
```

### yum 모듈

`yum` 모듈로 패키지 설치

```bash
# 모든 호스트에 httpd 설치
ansible all -m yum -a "name=httpd state=present"
```
`name=<패키지명>`

`state=<상태>` , present는 설치, absent는 삭제
 
### copy 모듈

`copy` 모듈로 파일 복사, 전송이 가능하다.

`src=<보낼파일>` 
`desc=<받을위치>`

```bash
ansible all -m copy -a "src=index.html dest=/var/www/html/index.html"
```

### service 모듈

각 노드에서 서비스들에 대한 시작/종료/재시작 등의 기능을 제공한다.

```bash
# httpd 서비스 시작
ansible all -m service -a "name=httpd state=started"
```

