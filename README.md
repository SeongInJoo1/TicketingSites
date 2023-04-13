# MOVIE-Tickets-Booking-Website
Movie tickets booking website using Django (Python Web framework).  

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

##########################################################
 아파치 및 wsgi 설치
  

sudo apt-get install apache2 
 mod-wsgi 설치
sudo apt install libapache2-mod-wsgi-py3

##########################################################
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
