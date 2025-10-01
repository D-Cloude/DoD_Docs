---
layout: page
title: PhpAdmin 설치 및 접속
description:  PhpAdmin 설치 및 접속 만드는 방법을 설명합니다.
---

# 서버 환경 준비

먼저, PHP와 웹 서버(Apache/Nginx), 그리고 데이터베이스(MySQL/MariaDB)가 설치되어 있어야 합니다.

## 시스템 패키지 업데이트
```bash
sudo apt update && sudo apt upgrade -y
```
## Apache, MySQL, PHP 설치
```bash
sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php -y
```

설치 후 서비스 상태 확인:
```bash
sudo systemctl status apache2
sudo systemctl status mysql
```
# phpMyAdmin 설치

CLI에서 설치:
```bash
sudo apt install phpmyadmin -y
```
설치 과정에서 다음 옵션 선택: <br/>

웹 서버: apache2 선택 <br/>
dbconfig-common을 사용하여 데이터베이스 구성 허용 <br/>
MySQL root 패스워드 입력 (또는 새 사용자 생성) <br/>

# Apache 설정 (수동 연결 필요 시)
phpMyAdmin이 Apache에 자동 연결되지 않은 경우:

```bash
sudo nano /etc/apache2/apache2.conf
```
파일 마지막에 다음 추가:

```bash
Include /etc/phpmyadmin/apache.conf
```

Apache 재시작:
```bash
sudo systemctl restart apache2
```
# 방화벽 설정

HTTP/HTTPS 포트 열기:

```bash
sudo ufw allow in "Apache Full"
sudo ufw status
```
# 웹 접속 확인

브라우저에서 다음 URL 접속:<br/><br/>
http://서버_IP:포트/phpmyadmin
<br/><br/>
로그인: MySQL root 계정 또는 설치 시 생성한 계정 사용
