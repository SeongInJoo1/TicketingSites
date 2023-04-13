# MOVIE-Tickets-Booking-Website
Movie tickets booking website using Django (Python Web framework).  
오픈소스 활용

# 장고 & 아파치 설치 및 연동, ubuntu 20.04 버전
1. #환경설정

sudo passwd root  우분투 루트 패스워드 설정.
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install python3

sudo apt install python3-venv

sudo apt-get install vim

 새 가상환경 설정. /home/sk/my_django_app 경로에서 함
python3 -m venv venv
source venv/bin/activate

-------------------------------------------------------
 장고 설치


pip install django
python -m django --version

사용할 장고프로젝트 우분투로 가지고 오기. /home/sk 경로에서 함
(venv) sk@ubuntu:~$ ls
Ticketing-master

장고 환경 설정 및 장고 실행
pip install Pillow
py manage.py migrate
py manage.py runserver

호스트 허용 설정
setting.py에서 ALLOWED_HOSTS = ['*'] 

-------------------------------------------------------
 아파치 및 wsgi 설치
  

sudo apt-get install apache2 
 mod-wsgi 설치
sudo apt install libapache2-mod-wsgi-py3

-------------------------------------------------------------
 conf파일 설정


cd /etc/apache2/sites-available
sudo vim 000-default.conf

<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

        alias /static /home/sk/Ticketing-master/movieticket/assets
        alias /media /home/sk/Ticketing-master/movieticket/media

        <Directory /home/sk/Ticketing-master/movieticket/assets/>
                Require all granted
        </Directory>

        <Directory /home/sk/Ticketing-master/movieticket/media/>
                Require all granted
        </Directory>
        <Directory /home/sk/Ticketing-master/movieticket/movieticket/>
                <Files wsgi.py>
                        Require all granted
                </Files>
        </Directory>

        WSGIDaemonProcess movieticket python-path=/home/sk/Ticketing-master/movieticket python-home=/home/sk/my_django_app/venv
        WSGIProcessGroup movieticket
        WSGIScriptAlias / /home/sk/Ticketing-master/movieticket/movieticket/wsgi.py

</VirtualHost>


sudo service apache2 restart

# 서버 배포

1. 가상머신을 bridge 방식으로 설정. 

 bridge방식 = ip 주소를 호스트 pc 에게서 받는게 아닌 

공유기로부터 직접 받아 호스트 pc와 동등한 ip 주소를 할당 받는 방식

[https://tristan91.tistory.com/238#recentComments](https://tristan91.tistory.com/238#recentComments)

1. 포트포워딩
- 주소창에 기본 게이트웨이(난 192.168.35.1) 입력 후 공유기 설정 화면으로 이동
- ID/PW 는 공유기마다 다름(구글링)
- NAT/라우터 관리 → 포트포워드 메뉴로 이동

**외부포트** = 80 (80포트 설정시 주소창에 포트번호 입력 불필요)

 **IP 주소** = 우분투IP주소(bridge로 할당받은 주소)

**내부포트** = 8080 (아파치에서 설정한 포트) 로 입력 후 추가

1. cafe24에서 구매한 도메인과 나의 외부 IP 주소 연결

1. 아파치 conf파일에서 ServerName [tornadooo.store](http://tornadooo.store) 로  설정

외부에서 접속 성공!
