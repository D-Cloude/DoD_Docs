---
layout: page
title: PHP 설정하는 방법
description: 우분투에 PHP를 설정히는 방법을 설명합니다.
---


# 1. PHP 설치  

### 1.1 PHP 패키지 업데이트 및 설치  

Ubuntu 24.04.1(기준)에서는 기본적으로 PHP 8.1 이상이 지원됩니다. 최신 PHP 버전을 설치하려면 다음 명령어를 실행합니다.  

```sh
sudo apt update
sudo apt install php -y
```

설치 후 버전을 확인합니다.  

```sh
php -v
```

출력 예시:  

```plaintext
PHP 8.1.2 (cli) (built: Jan 12 2024 08:43:21) ( NTS )
```

### 1.2 추가 PHP 확장 패키지 설치  

일반적으로 필요한 PHP 확장 모듈을 함께 설치할 수 있습니다.  

```sh
sudo apt install php-cli php-fpm php-mysql php-curl php-xml php-mbstring php-zip php-bcmath php-intl -y
```

특정 패키지가 필요한 경우 `php-<패키지명>` 형식으로 추가 설치할 수 있습니다.  

---

# 2. PHP 설정 변경  

설정 파일 `php.ini`를 수정하여 PHP 환경을 조정할 수 있습니다.  

### 2.1 PHP 설정 파일 찾기  

설정 파일 위치를 찾으려면 아래 명령어를 실행합니다.  

```sh
php --ini
```

출력 예시:  

```plaintext
Configuration File (php.ini) Path: /etc/php/8.1/cli
Loaded Configuration File: /etc/php/8.1/cli/php.ini
```

### 2.2 `php.ini` 파일 편집  

파일을 수정하려면 `nano` 또는 `vim`을 사용할 수 있습니다.  

```sh
sudo nano /etc/php/8.1/cli/php.ini
```

또는 웹 서버용 PHP 설정을 변경하려면:  

```sh
sudo nano /etc/php/8.1/fpm/php.ini
```

변경할 수 있는 주요 설정:  

| 설정 | 기본값 | 추천값 | 설명 |
|------|------|------|------|
| `memory_limit` | `128M` | `512M` | PHP가 사용할 최대 메모리 |
| `upload_max_filesize` | `2M` | `50M` | 업로드 가능한 최대 파일 크기 |
| `post_max_size` | `8M` | `50M` | POST 요청의 최대 크기 |
| `max_execution_time` | `30` | `300` | 스크립트 최대 실행 시간(초) |

설정 변경 후 저장(`Ctrl + X` → `Y` → `Enter`)하고 PHP를 다시 시작해야 적용됩니다.  

```sh
sudo systemctl restart php8.1-fpm
```

---

# 3. PHP-FPM 설정  

PHP-FPM(FastCGI Process Manager)은 웹 서버(Nginx, Apache)와 PHP 간의 성능을 최적화하는 데 사용됩니다.  

### 3.1 PHP-FPM 서비스 확인  

```sh
sudo systemctl status php8.1-fpm
```

출력 예시:  

```plaintext
● php8.1-fpm.service - The PHP 8.1 FastCGI Process Manager
   Loaded: loaded (/lib/systemd/system/php8.1-fpm.service; enabled; vendor preset: enabled)
   Active: active (running)
```

### 3.2 PHP-FPM 풀 설정  

설정 파일을 수정하려면:  

```sh
sudo nano /etc/php/8.1/fpm/pool.d/www.conf
```

변경할 수 있는 주요 설정:  

| 설정 | 기본값 | 추천값 | 설명 |
|------|------|------|------|
| `pm` | `dynamic` | `ondemand` | 프로세스 관리 방식 |
| `pm.max_children` | `5` | `10` | 최대 동시 실행 가능 프로세스 수 |
| `pm.start_servers` | `2` | `5` | 초기 프로세스 개수 |

설정 변경 후 적용:  

```sh
sudo systemctl restart php8.1-fpm
```

---

# 4. Apache 또는 Nginx와 PHP 연동  

### 4.1 Apache에서 PHP 사용  

Apache에 PHP 모듈을 추가하려면 다음을 실행합니다.  

```sh
sudo apt install libapache2-mod-php -y
sudo systemctl restart apache2
```

### 4.2 Nginx에서 PHP-FPM 사용  

Nginx에서는 PHP-FPM을 사용해야 합니다.  

설정 파일을 편집합니다.  

```sh
sudo nano /etc/nginx/sites-available/default
```

다음 내용을 추가하거나 수정합니다.  

```nginx
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
```

변경 사항 적용 후 Nginx 재시작:  

```sh
sudo systemctl restart nginx
```

---

# 5. PHP 테스트  

PHP가 정상적으로 작동하는지 확인하려면 웹 루트 디렉터리에 테스트 파일을 생성합니다.  

```sh
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

웹 브라우저에서 `http://서버_IP/info.php` 에 접속하여 PHP 정보 페이지가 출력되는지 확인합니다.  

테스트 후 보안상 이유로 파일을 삭제하는 것이 좋습니다.  

{% include alert.html type="warning" title="주의사항" content="DCP같은 경우 따로 설정해 드린 도메인으로 접속하고 ASW-PSMT는 포트로 접속 부탁드림니다." %}
```sh
sudo rm /var/www/html/info.php
```

---

# 6. PHP 확장 모듈 목록 확인  

설치된 PHP 확장 모듈을 확인하려면 다음 명령어를 실행합니다.  

```sh
php -m
```

특정 확장 모듈이 포함되어 있는지 확인하려면:  

```sh
php -m | grep mysqli
```

---

## 7. PHP 로그 확인  

PHP 실행 중 오류가 발생하면 로그를 확인할 수 있습니다.  

```sh
sudo tail -f /var/log/php8.1-fpm.log
```

Apache 사용 시 PHP 오류 로그는 다음과 같습니다.  

```sh
sudo tail -f /var/log/apache2/error.log
```

Nginx 사용 시 PHP 오류 로그는 다음과 같습니다.  

```sh
sudo tail -f /var/log/nginx/error.log
```

---

## 8. PHP 제거  

PHP를 완전히 제거하려면 다음 명령어를 실행합니다.  

```sh
sudo apt remove --purge php php-cli php-fpm php-mysql -y
sudo apt autoremove -y
```

설정 파일까지 모두 삭제하려면 `/etc/php/` 디렉터리를 제거합니다.  

```sh
sudo rm -rf /etc/php/
```

---

## 9. 요약  

| 명령어 | 설명 |
|--------|------|
| `sudo apt install php -y` | PHP 설치 |
| `php -v` | PHP 버전 확인 |
| `php --ini` | php.ini 경로 확인 |
| `sudo nano /etc/php/8.1/cli/php.ini` | PHP 설정 수정 |
| `sudo systemctl restart php8.1-fpm` | PHP-FPM 재시작 |
| `sudo apt install libapache2-mod-php -y` | Apache에서 PHP 활성화 |
| `php -m` | 설치된 PHP 확장 모듈 확인 |
| `sudo tail -f /var/log/php8.1-fpm.log` | PHP 오류 로그 확인 |
| `sudo apt remove --purge php -y` | PHP 제거 |

