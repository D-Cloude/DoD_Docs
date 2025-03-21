---
layout: page
title: mysql 설치방법
description: 우분투에 DB를 설치하는 방법을 설명합니다.
---

# 1. MySQL 설치  

### 1.1 패키지 목록 업데이트  
먼저, 시스템 패키지 목록을 업데이트합니다.  

```sh
sudo apt update
```

### 1.2 MySQL 서버 설치  
MySQL 서버를 설치합니다.  

```sh
sudo apt install mysql-server -y
```

### 1.3 MySQL 서비스 상태 확인  
설치가 완료되면 MySQL 서비스가 정상적으로 실행되고 있는지 확인합니다.  

```sh
sudo systemctl status mysql
```

실행 중이라면 `active (running)` 상태가 출력됩니다.  

---

# 2. MySQL 보안 설정 (`mysql_secure_installation`)  

MySQL 설치 후 보안 강화를 위해 초기 설정을 진행합니다.  

```sh
sudo mysql_secure_installation
```

여기에서 설정할 주요 항목은 다음과 같습니다.  

1. **VALIDATE PASSWORD 플러그인 사용 여부**  
   - 강력한 비밀번호 정책을 원하면 `Y`를 입력 후 `LOW/MEDIUM/STRONG` 중 선택  
   - 원하지 않으면 `N` 입력  

2. **루트 비밀번호 설정 (Ubuntu 기본 설정에서는 비밀번호 없이 root 접속 가능)**  
   - 새 비밀번호를 입력하고 확인  

3. **익명 사용자 삭제**  
   - `Y` 입력  

4. **원격 root 로그인 차단**  
   - `Y` 입력 (보안 강화를 위해 권장)  

5. **테스트 데이터베이스 및 접근 권한 삭제**  
   - `Y` 입력  

6. **권한 테이블 다시 로드**  
   - `Y` 입력  

---

# 3. MySQL 원격 접속 허용 (`bind-address` 설정)  

기본적으로 MySQL은 `127.0.0.1`(로컬호스트)에서만 접근 가능하도록 설정되어 있습니다. 이를 변경하려면 `bind-address` 값을 수정해야 합니다.  

### 3.1 MySQL 설정 파일 수정  

```sh
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

`mysqld` 섹션에서 아래 부분을 찾아 수정합니다.  

```ini
bind-address = 0.0.0.0
```

💡 `0.0.0.0`으로 설정하면 모든 IP에서 접근할 수 있습니다. 특정 IP만 허용하려면 해당 IP 주소로 변경하세요.  

### 3.2 MySQL 서비스 재시작  

설정을 적용하려면 MySQL 서비스를 재시작해야 합니다.  

```sh
sudo systemctl restart mysql
```

---

# 4. 원격 접속을 위한 사용자 권한 부여  

MySQL 원격 접속을 허용하려면 특정 사용자에게 권한을 부여해야 합니다.  

### 4.1 MySQL에 root로 접속  

```sh
sudo mysql -u root -p
```

### 4.2 원격 접속 가능한 계정 생성 및 권한 부여  

예를 들어, `remote_user`라는 사용자를 만들고 원격 접속을 허용하려면 다음 명령어를 실행합니다.  

```sql
CREATE USER 'remote_user'@'%' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

💡 `your_secure_password` 부분을 원하는 비밀번호로 변경하세요.  

특정 IP에서만 접근을 허용하려면 `%` 대신 해당 IP 주소를 입력합니다.  

```sql
CREATE USER 'remote_user'@'192.168.1.100' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'192.168.1.100' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

---

# 5. 방화벽 (UFW) 설정  

Ubuntu에서 `ufw`(Uncomplicated Firewall)를 사용하는 경우, MySQL 포트(기본 3306)를 열어야 합니다.  

```sh
sudo ufw allow 3306/tcp
sudo ufw reload
```
{% include alert.html type="warning" title="안내사항" content="방화벽을 설정할때 이 명령어도 입력해주세요. `sudo ufw allow 22/tcp` 하지 않을경우 SSH연결이 안될수 있습니다." %}

방화벽이 활성화되지 않은 경우 아래 명령어로 활성화할 수 있습니다.  

```sh
sudo ufw enable
```

설정된 방화벽 규칙을 확인하려면 다음을 실행합니다.  

```sh
sudo ufw status
```

---

## 6. MySQL 원격 접속 테스트  

설정이 완료되었으면 원격에서 MySQL 접속을 테스트할 수 있습니다.  

로컬 머신 또는 다른 서버에서 다음 명령어를 실행합니다.  

```sh
mysql -u remote_user -h <서버 IP> -p
```

💡 `<서버 IP>` 부분을 MySQL이 설치된 서버의 IP 주소로 변경하세요.  

만약 접속이 안 된다면 아래 사항을 다시 확인하세요.  

- `bind-address` 설정이 `0.0.0.0`인지 확인  
- 사용자 계정이 원격 접속을 허용하는지 확인 (`%` 또는 특정 IP)  
- 방화벽(`ufw`)이 3306 포트를 열었는지 확인  

