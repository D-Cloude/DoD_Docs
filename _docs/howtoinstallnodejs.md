---
layout: page
title: Nodejs 설정하는 방법
description: 우분투에 Nodejs를 설정히는 방법을 설명합니다.
--- 

# 1. Node.js 설치  

Node.js는 기본적으로 APT 패키지 관리자 또는 NodeSource PPA를 통해 설치할 수 있습니다.  

### 1.1 APT 패키지 관리자 이용 (권장되지 않음)  

Ubuntu의 기본 저장소에서 제공하는 Node.js 버전을 설치하는 방법입니다.  

```sh
sudo apt update
sudo apt install nodejs -y
sudo apt install npm -y
```

설치 후 버전을 확인합니다.  

```sh
node -v
npm -v
```

하지만 Ubuntu 기본 저장소에는 오래된 버전이 포함될 수 있으므로, **NodeSource PPA 또는 NVM을 사용하는 방법을 추천**합니다.  

---

### 1.2 NodeSource PPA 이용 (최신 LTS 설치)  

NodeSource 저장소를 추가하여 최신 Node.js LTS 버전을 설치할 수 있습니다.  

1. **NodeSource 저장소 추가 및 설치**  

```sh
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs -y
```

2. **설치 확인**  

```sh
node -v
npm -v
```

이 방법을 사용하면 최신 LTS 버전이 설치됩니다.  

---

### 1.3 NVM(Node Version Manager) 이용 (여러 버전 관리 가능)  

**NVM을 사용하면 여러 버전의 Node.js를 쉽게 관리할 수 있습니다.**  

1. **NVM 설치**  

```sh
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
source ~/.bashrc
```

2. **설치 확인**  

```sh
command -v nvm
```

3. **최신 LTS 버전 설치**  

```sh
nvm install --lts
```

4. **Node.js 버전 확인 및 변경**  

```sh
nvm list
nvm use <버전>
nvm alias default <버전>
```

---

# 2. npm (Node.js 패키지 관리자) 설정  

Node.js를 설치하면 기본적으로 `npm`이 포함됩니다.  

### 2.1 npm 최신 버전 업데이트  

```sh
npm install -g npm
```

### 2.2 npm 기본 전역 패키지 디렉터리 설정  

기본적으로 npm의 글로벌 패키지는 `/usr/local/`에 설치됩니다. 권한 문제를 방지하려면 별도의 디렉터리를 설정하는 것이 좋습니다.  

1. 디렉터리 생성  

```sh
mkdir "${HOME}/.npm-global"
```

2. npm 글로벌 패키지 경로 변경  

```sh
npm config set prefix "${HOME}/.npm-global"
```

3. 환경 변수 추가 (`.bashrc` 또는 `.zshrc` 편집)  

```sh
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

설정 후 npm 패키지를 전역으로 설치할 수 있습니다.  

```sh
npm install -g yarn pm2
```

---

# 3. Node.js 실행 테스트  

1. `node` 명령어로 직접 실행  

```sh
node
```

입력:  

```js
console.log("Hello, Node.js!");
```

출력:  

```plaintext
Hello, Node.js!
```

2. 파일 실행 테스트  

```sh
echo "console.log('Hello from Node.js');" > test.js
node test.js
```

---

# 4. Yarn 패키지 관리자 설치 (선택 사항)  

`npm`보다 빠른 **Yarn**을 설치하려면 다음을 실행합니다.  

```sh
npm install -g yarn
yarn -v
```

---

# 5. PM2 프로세스 관리자 설정  

**PM2**는 Node.js 애플리케이션을 백그라운드에서 실행하고 자동으로 재시작할 수 있도록 관리하는 툴입니다.  

1. PM2 설치  

```sh
npm install -g pm2
```

2. Node.js 앱 실행 및 백그라운드 관리  

```sh
pm2 start test.js --name "my-app"
```

3. PM2 프로세스 목록 확인  

```sh
pm2 list
```

4. 시스템 재부팅 시 PM2 자동 실행 설정  

```sh
pm2 startup
pm2 save
```

---

# 6. Node.js 제거  

**APT 패키지로 설치한 경우**  

```sh
sudo apt remove --purge nodejs npm -y
sudo apt autoremove -y
```

**NodeSource로 설치한 경우**  

```sh
sudo rm -rf /usr/local/lib/node_modules
sudo rm -rf ~/.npm
sudo rm -rf ~/.node-gyp
```

**NVM으로 설치한 경우**  

```sh
nvm deactivate
nvm uninstall <버전>
```

---

## 7. 요약  

| 명령어 | 설명 |
|--------|------|
| `sudo apt install nodejs npm -y` | 기본 저장소에서 Node.js 및 npm 설치 |
| `curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -` | 최신 Node.js LTS 설치 (NodeSource) |
| `curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash` | NVM 설치 |
| `nvm install --lts` | 최신 LTS 버전 Node.js 설치 |
| `npm install -g yarn pm2` | Yarn 및 PM2 설치 |
| `pm2 start test.js --name "my-app"` | PM2로 애플리케이션 실행 |
| `pm2 startup && pm2 save` | PM2 자동 실행 설정 |

