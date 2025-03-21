---
layout: page
title: 방화벽 설정하는 방법
description: 우분투에 방화벽을 설정히는 방법을 설명합니다.
---

# 1. UFW 설치 및 기본 설정  

### 1.1 UFW 설치 확인  

Ubuntu 24.04.1에서는 기본적으로 UFW가 설치되어 있습니다. 아래 명령어로 설치 여부를 확인할 수 있습니다.  

```sh
sudo ufw status
```

UFW가 설치되지 않았다면 다음 명령어로 설치할 수 있습니다.  

```sh
sudo apt update
sudo apt install ufw -y
```

### 1.2 UFW 활성화  

UFW를 활성화하려면 다음 명령어를 실행합니다.  

```sh
sudo ufw enable
```

⚠️ **주의:** UFW를 활성화하면 기본적으로 모든 **외부 연결이 차단**됩니다. SSH를 사용하는 경우 먼저 SSH 포트를 허용하세요.  

---

# 2. 포트 및 서비스 허용  

UFW를 활성화한 후, 필요한 포트를 허용해야 합니다.  

### 2.1 SSH(원격 접속) 허용  

원격에서 SSH로 접속하려면 포트 22를 열어야 합니다.  

```sh
sudo ufw allow 22/tcp
```

만약 SSH 포트를 변경했다면(예: `2222`), 해당 포트를 열어야 합니다.  

```sh
sudo ufw allow 2222/tcp
```

---

### 2.2 일반적인 서비스 및 포트 허용  

| 서비스 | 기본 포트 | 허용 명령어 |
|--------|---------|------------|
| SSH | 22 | `sudo ufw allow 22/tcp` |
| HTTP (웹 서버) | 80 | `sudo ufw allow 80/tcp` |
| HTTPS (SSL) | 443 | `sudo ufw allow 443/tcp` |
| MySQL | 3306 | `sudo ufw allow 3306/tcp` |
| PostgreSQL | 5432 | `sudo ufw allow 5432/tcp` |
| FTP | 21 | `sudo ufw allow 21/tcp` |

**예제:** Apache(HTTP, HTTPS)와 MySQL을 허용하는 경우  

```sh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 3306/tcp
```

---

### 2.3 특정 IP에만 포트 허용  

모든 IP가 아니라 특정 IP만 허용하려면 다음과 같이 설정할 수 있습니다.  

예를 들어, **192.168.1.100** IP에서만 SSH(포트 22) 접속을 허용하려면:  

```sh
sudo ufw allow from 192.168.1.100 to any port 22
```

특정 서브넷(예: `192.168.1.0/24`)에서만 MySQL(포트 3306)을 허용하려면:  

```sh
sudo ufw allow from 192.168.1.0/24 to any port 3306
```

---

# 3. 포트 및 서비스 차단  

기본적으로 모든 포트는 차단되어 있지만, 특정 포트를 수동으로 차단할 수도 있습니다.  

```sh
sudo ufw deny 3306/tcp  # MySQL 차단
sudo ufw deny from 203.0.113.100  # 특정 IP 차단
```

---

# 4. 방화벽 상태 확인 및 관리  

### 4.1 현재 방화벽 상태 확인  

설정된 방화벽 규칙을 확인하려면 다음 명령어를 실행합니다.  

```sh
sudo ufw status verbose
```

출력 예시:  

```plaintext
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
3306/tcp                   ALLOW       192.168.1.0/24
```

### 4.2 특정 규칙 삭제  

규칙을 삭제하려면 `sudo ufw delete` 명령어를 사용합니다.  

**예제:**  
- 포트 3306(MySQL) 허용 규칙 삭제  

  ```sh
  sudo ufw delete allow 3306/tcp
  ```

- 특정 IP(예: `203.0.113.100`) 차단 해제  

  ```sh
  sudo ufw delete deny from 203.0.113.100
  ```

- 설정된 규칙을 번호로 확인 후 삭제  

  ```sh
  sudo ufw status numbered
  sudo ufw delete <번호>
  ```

---

# 5. 기본 정책 변경  

기본적으로 UFW는 **모든 수신(ingress) 연결을 차단**하고 **모든 송신(egress) 연결을 허용**합니다. 이를 변경할 수도 있습니다.  

```sh
sudo ufw default deny incoming   # 모든 수신 요청 차단
sudo ufw default allow outgoing  # 모든 송신 요청 허용
```

특정 송신 포트도 차단하려면 다음과 같이 설정합니다.  

```sh
sudo ufw deny out 25/tcp  # SMTP(메일 발송) 차단
```

---

# 6. UFW 비활성화 및 재설정  

### 6.1 UFW 비활성화  

방화벽을 일시적으로 비활성화하려면:  

```sh
sudo ufw disable
```

### 6.2 UFW 초기화 (모든 규칙 삭제)  

UFW 설정을 초기 상태로 되돌리려면:  

```sh
sudo ufw reset
```

---

# 7. 요약  

| 명령어 | 설명 |
|--------|------|
| `sudo ufw enable` | 방화벽 활성화 |
| `sudo ufw disable` | 방화벽 비활성화 |
| `sudo ufw status verbose` | 방화벽 상태 확인 |
| `sudo ufw allow 80/tcp` | 특정 포트(80) 허용 |
| `sudo ufw deny 3306/tcp` | 특정 포트(3306) 차단 |
| `sudo ufw allow from 192.168.1.100 to any port 22` | 특정 IP(192.168.1.100)에서 SSH 허용 |
| `sudo ufw delete allow 22/tcp` | 특정 규칙(SSH 허용) 삭제 |
| `sudo ufw default deny incoming` | 기본적으로 모든 수신 요청 차단 |
| `sudo ufw reset` | 모든 방화벽 규칙 초기화 |

---

