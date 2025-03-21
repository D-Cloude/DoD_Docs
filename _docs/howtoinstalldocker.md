---
layout: page
title: 도커 설치방법
description: 우분투에 도커를 설치하는 방법을 설명합니다.
---

# 1. 기존 Docker 패키지 제거

먼저, 시스템에 기존의 Docker 관련 패키지가 설치되어 있다면 제거합니다.

```sh
sudo apt remove docker docker-engine docker.io containerd runc -y
```

# 2. 필수 패키지 설치

Docker를 설치하기 전에 필요한 패키지를 먼저 설치합니다.

```sh
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
```

# 3. Docker 공식 GPG 키 추가

Docker 공식 패키지를 다운로드할 수 있도록 GPG 키를 추가합니다.

```sh
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.gpg > /dev/null
```

# 4. Docker 저장소 추가

Docker 패키지를 다운로드할 수 있도록 공식 저장소를 추가합니다.

```sh
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

# 5. Docker 패키지 설치

Docker 엔진과 필수 구성 요소를 설치합니다.

```sh
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# 6. Docker 서비스 확인 및 실행

설치가 완료되면 Docker 서비스가 정상적으로 실행되고 있는지 확인합니다.

```sh
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

# 7. Docker 실행 테스트

다음 명령어를 실행하여 Docker가 정상적으로 동작하는지 확인합니다.

```sh
sudo docker run hello-world
```

출력 결과에 "Hello from Docker!"가 표시되면 정상적으로 설치된 것입니다.

# 8. 일반 사용자로 Docker 사용 (선택 사항)

Docker 명령어를 실행할 때마다 `sudo`를 입력하는 것이 번거롭다면, 현재 사용자를 `docker` 그룹에 추가할 수 있습니다.

```sh
sudo usermod -aG docker $USER
```

이후 로그아웃 후 다시 로그인하면 변경 사항이 적용됩니다. 적용된 후 아래 명령어를 실행하여 정상적으로 동작하는지 확인합니다.

```sh
docker run hello-world
```

# 9. Docker 자동 시작 설정 (선택 사항)

서버가 재부팅될 때 Docker가 자동으로 실행되도록 설정합니다.

```sh
sudo systemctl enable docker
```

# 10. Docker Compose 설치 (선택 사항)

Docker Compose도 함께 사용하려면 아래 명령어를 실행하여 설치합니다.

```sh
sudo apt install -y docker-compose-plugin
```

설치가 완료되면 아래 명령어로 정상적으로 동작하는지 확인할 수 있습니다.

```sh
docker compose version
```

# 11. Docker 제거 방법 (선택 사항)

만약 Docker를 제거하고 싶다면 아래 명령어를 실행합니다.

```sh
sudo apt remove -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```


