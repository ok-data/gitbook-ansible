# role

- `tasks` : 실제 playbook이 실행할 명령어 모음
- `handlers` : systemd 등록이나 service를 처리하기 위해 사용
- `defaults` : 기본 변수
- `vars` : 작업 수행 시 사용할 변수 정의
- `files` : 각 노드에 배포할 파일의 위치
- `templates` : 각 노드에 배포할 템플릿 파일 위치
- `meta` : roles에 관한 정보, 메타데이터정의


`become` : sudo대신 씀