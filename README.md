# Amazon linux 2에서 nginx, tomcat 연동

### nginx 설치

그냥 yum instal nginx가 아니라 
amazon-linux-extras install nginx1.12 명령어를 통해서 다운 받는다.

service nginx start 로 기동  

웹서버 잘되는지 확인

### tomcat 설치

자바 깔려있는지 확인 java -version

없으면 sudo yum install -y java-1.8.0-openjdk-devel.x86_64

sudo /usr/sbin/alternatives --config java 으로 확인

기존 버젼 있으면 sudo yum remove java-1.7.0-openjdk 로 삭제한다.

재설정후 java -version 로 버젼 확인

wget 으로 tomcat을 받는다.

tar.gz이니깐 tar -zxvf 로 파일압축 풀기를 한다.

apache tomcat/bin/startup.sh 를 실행시킨다.

아 여기서 AWS SG도 8080포트 열어준다.

### nginx, tomcat 연동

root 권한으로 vi /etc/nginx/nginx.conf
에서 /server로 이거 찾고 location 아래에

proxy_pass http://localhost:8080;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Host $http_host;
을 추가 한다.

완성
