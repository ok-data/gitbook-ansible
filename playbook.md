# playbook

## 작성하기

nginx 웹 서버 설치 예시 `nginx_install.yaml` :

```yaml
---
- name: Install nginx on linux
  hosts: nginx
  gather_facts: no

  tasks:
    - name: install epel-release
      yum: name=epel-release  state=latest
    - name: install nginx web server
      yum: name=nginx state=present
    - name: upload default index.html for web server
      get_url: url=https://www.nginx.com dest=/usr/share/nginx/html/ mode=0644
    - name: start nginx web server
      service: name=nginx state=started
```

처음은 항상 `---`로 시작한다.

```yaml
- name: Install nginx on linux ## 현재 파일의 목적을 적는게 좋음
  hosts: nginx ## 호스트의 alias를 지정한다. /etc/ansible/hosts
```
`gather_facts` : 필요하지 않은 내용을 수집하지 않는 옵션

실제 수행하는 부분은 `tasks` 부터이다.

```yaml
  tasks:
    - name: install epel-release
      yum: name=epel-release  state=latest 
```

yum 모듈로 `epel-release`를 설치한다. `state=latest`는 실행시마다 최신으로 업데이트한다.

```yaml
    - name: install nginx web server
      yum: name=nginx state=present
```
nginx 설치하는 구문이다.

```yaml
    - name: upload default index.html for web server
      get_url: url=https://www.nginx.com dest=/usr/share/nginx/html/ mode=0644
```

curl로 웹페이즈를 받아오고 기본 웹페이즈를 변경한다.

`url=<url>`

`dest=<저장할 위치>`

`mode=<chmod 옵션>`

```yaml
    - name: start nginx web server
      service: name=nginx state=started
```

nginx 서비스를 시작시킨다.

## 실행

```bash
ansible-playbook nginx_install.yaml
```