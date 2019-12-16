# Amazon linux 2에서 nginx, tomcat 연동

### nginx 설치

yum install nginx가 아니라 
amazon-linux-extras install nginx1.12 명령어를 통해서 다운 받는다.

service nginx start로 기동
service nginx status로 상태 확인

### tomcat 설치

tomcat은 java 기반이라 자바 설치가 필수! 

ec2 linux 내부에 설치 확인 java -version

없으면 sudo yum install -y java-1.8.0-openjdk-devel.x86_64

sudo /usr/sbin/alternatives --config java 으로 확인

기존 버젼 있으면 sudo yum remove java-1.7.0-openjdk 로 삭제한다.

재설정후 java -version 로 버젼 확인

자바 설치 완료 !

tomcat 홈페이지에서 8.5 버젼을다운 받는다 >>> http://tomcat.apache.org/

리눅스에서 wget으로 링크를 활용하여 다운로드 ! (다운로드 주소 cd /etc/usr)

tar.gz이니깐 tar -zxvf 로 파일압축 풀기

apache-tomcat-8.5.50/bin/startup.sh 를 실행시킨다.

AWS SG에서도 톰캣을 열기 위해 8080포트 열어준다.

### nginx, tomcat 연동

root 권한으로 vi /etc/nginx/nginx.conf
에서 /server로 이거 찾고 location 아래에

proxy_pass http://localhost:8080;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Host $http_host;
을 추가 한다.

## 정상적 종료

웹서버(nginx) 종료 : service nginx stop
WAS(tomcat) 종료 : apache-tomcat-8.5.50/bin/startup.sh
운영체제 종료 : shutdown -h now   <- now는 지금 종료한다는뜻... 이상

