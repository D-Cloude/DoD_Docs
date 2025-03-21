---
layout: page
title: 미러 변경 방법
description: 우분투에 설정한 미러가 느린경우 변경하는 법을 설명합니다.
---

# Ubuntu 미러 변경 방법

Ubuntu에서 패키지 다운로드 속도를 높이거나 특정 미러를 사용해야 할 때, 패키지 미러를 변경할 수 있습니다.

## 1. 현재 사용 중인 미러 확인

터미널에서 다음 명령어를 실행하여 현재 사용 중인 미러를 확인할 수 있습니다.

```sh
cat /etc/apt/sources.list
```

또는, 아래 명령어를 실행하여 사용 중인 미러 서버를 출력할 수 있습니다.

```sh
grep -E '^deb ' /etc/apt/sources.list
```

## 2. 미러 변경 방법
### 1) 미러 리스트 확인

공식 Ubuntu 미러 목록은 [Ubuntu 공식 사이트](https://launchpad.net/ubuntu/+archivemirrors)에서 확인할 수 있습니다.

또는 아래 명령어를 사용하여 속도가 빠른 미러를 자동으로 찾을 수도 있습니다.

```sh
apt-get install netselect-apt -y
sudo netselect-apt
```

### 2) `/etc/apt/sources.list` 파일 수정

1. 기존 `sources.list` 파일을 백업합니다.
   ```sh
   sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
   ```
2. 편집기를 사용하여 파일을 수정합니다.
   ```sh
   sudo nano /etc/apt/sources.list
   ```
3. 기존 `http://archive.ubuntu.com/ubuntu/` 또는 `http://security.ubuntu.com/ubuntu/` 부분을 변경하고자 하는 미러로 교체합니다.

   예를 들어, 한국 KAIST 미러로 변경하려면 다음과 같이 수정합니다.

   ```plaintext
   deb http://ftp.kaist.ac.kr/ubuntu/ jammy main restricted universe multiverse
   deb http://ftp.kaist.ac.kr/ubuntu/ jammy-updates main restricted universe multiverse
   deb http://ftp.kaist.ac.kr/ubuntu/ jammy-security main restricted universe multiverse
   ```

4. 파일을 저장하고 종료합니다. (`Ctrl + X` → `Y` → `Enter`)

## 3. 변경된 미러 적용

미러 변경 후 다음 명령어를 실행하여 패키지 목록을 업데이트해야 합니다.

```sh
sudo apt update
sudo apt upgrade -y
```

## 4. 자동으로 빠른 미러 선택하기

Ubuntu는 `apt` 명령어를 통해 가장 빠른 미러를 자동으로 찾을 수도 있습니다.

```sh
sudo apt update
sudo apt install netselect-apt
sudo netselect-apt
```

## 5. 변경된 미러가 적용되었는지 확인

아래 명령어를 실행하여 패키지 다운로드 시 적용된 미러를 확인할 수 있습니다.

```sh
sudo apt update | grep Hit
```

## 추가 참고

- 변경 후 업데이트 시 오류가 발생하면 백업한 `sources.list.bak` 파일을 복구하세요.
  ```sh
  sudo cp /etc/apt/sources.list.bak /etc/apt/sources.list
  sudo apt update
  ```
- 미러 속도가 느리다면 다른 미러로 변경해보세요.

### 한국 우분투 미러 리스트

| 미러 이름 | URL |
|-----------|----------------------------------------|
| KAIST | `http://ftp.kaist.ac.kr/ubuntu/` |
|Jeonnam High School | `http://mirror.jeonnam.school/ubuntu/`|
|Yuki Network | `http://mirror.yuki.net.uk/ubuntu/`|
|Morgan|`https://mirror.morgan.kr/ubuntu/`|
| 카카오 | `http://mirror.kakao.com/ubuntu/` |
| 네이버 | `http://mirror.navercorp.com/ubuntu/` |

