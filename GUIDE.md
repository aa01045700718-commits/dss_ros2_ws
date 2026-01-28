# ROS2 DSS Bridge 사용 가이드

## 1. 리눅스 명령어 기초

### 환경 진입
```bash
# WSL Ubuntu 진입
wsl -d Ubuntu-22.04-DSS

# 홈 디렉토리로 이동
cd ~

# ROS2 워크스페이스로 이동
cd ros2_ws

# 현재 디렉토리 파일 목록 확인
ls
```

### 텍스트 에디터 설치
```bash
# gedit 설치 (GUI 텍스트 에디터)
sudo apt install gedit
```

---

## 2. ROS2 관련 명령어

### 빌드 및 환경 설정
```bash
# 워크스페이스로 이동
cd ros2_ws

# 빌드 디렉토리 초기화 (클린 빌드)
rm -rf build
rm -rf install
rm -rf log

# 패키지 빌드
colcon build

# 환경 설정 (빌드 후 필수)
source install/setup.bash
```

### ROS2 노드 및 토픽 확인
```bash
# 실행 중인 노드 목록 확인
ros2 node list

# 활성화된 토픽 목록 확인
ros2 topic list

# 특정 토픽의 메시지 내용 확인
ros2 topic echo /topic_name
```

### 패키지 실행
```bash
# dss_ros2_bridge 패키지 실행
ros2 launch dss_ros2_bridge launch.py
```

### 터미널 명령어
```bash
# 터미널 화면 초기화
clear

# 파일 목록 확인
ls
```

---

## 3. GitHub 사용법

### 설치 및 로그인
```bash
# Git CLI 설치
sudo apt install git

# GitHub CLI 설치
sudo apt install gh

# GitHub에 로그인
gh auth login
```

### Git 기본 명령어
```bash
# 사용자 정보 설정
git config --global user.name "이름"
git config --global user.email "이메일"

# 변경사항 확인
git status

# 변경사항 스테이징
git add .

# 커밋
git commit -m "커밋 메시지"

# 푸시 (원격 저장소에 업로드)
git push

# 풀 (원격 저장소에서 다운로드)
git pull

# 이력 확인
git log --oneline
```

### 브랜치 관리
```bash
# 브랜치 목록 확인
git branch -a

# 새 브랜치 생성
git branch 브랜치명

# 브랜치 전환
git checkout 브랜치명

# 브랜치 이름 변경
git branch -m 기존이름 새이름

# 브랜치 삭제
git branch -d 브랜치명

# 원격 브랜치 삭제
git push origin --delete 브랜치명
```

### 저장소 초기화 및 클론
```bash
# 저장소 초기화
git init

# 원격 저장소 추가
git remote add origin https://github.com/username/repo.git

# 원격 저장소 클론
git clone https://github.com/username/repo.git
```

### Tip
- **VS Code WSL 확장 설치 후**: GitHub Copilot에 대화창으로 git 명령어를 요청하면 자동으로 실행 가능

---

## 4. Visual Studio Code 사용법

### RViz2 시각화

#### 카메라/GPS/경로 표시
- **Sensor Viz**: 카메라, GPS, 경로 정보를 시각화하는 도구
- 로봇 센서 데이터를 실시간으로 확인 가능

#### LiDAR 시각화
- **LiDAR Viz**: 포인트 클라우드 데이터 표시
- **마우스 휠 클릭**: 수평 이동 가능

#### 재생 제어
- **Roof Play Back**: 기록된 데이터 재생
- **뒤로 가기**: 역방향 재생 가능
- **일시 정지/재생**: 재생 속도 제어

### 조작법
```
W: 앞으로 이동
A: 왼쪽 이동
S: 뒤로 이동
D: 오른쪽 이동

마우스 휠: 줌 인/아웃
마우스 드래그: 시점 회전
마우스 휠 클릭: 수평 이동
```

---

## 5. 프로젝트 구조

```
ros2_ws/
├── src/
│   └── dss_ros2_bridge/
│       ├── src/              # C++ 소스 파일
│       ├── launch/           # ROS2 런치 파일
│       ├── msg/              # ROS2 메시지 정의
│       ├── CMakeLists.txt     # CMake 빌드 설정
│       └── package.xml        # 패키지 메타데이터
├── build/                     # 빌드 산출물 (자동 생성)
├── install/                   # 설치 산출물 (자동 생성)
├── log/                       # 빌드 로그 (자동 생성)
└── .gitignore                 # Git 무시 파일
```

---

## 6. 자주 사용하는 워크플로우

### 개발 사이클
```bash
# 1. 워크스페이스 진입
cd ~/ros2_ws

# 2. 소스 코드 수정
# (src/ 디렉토리의 파일 편집)

# 3. 빌드
colcon build

# 4. 환경 설정
source install/setup.bash

# 5. 테스트 실행
ros2 launch dss_ros2_bridge launch.py

# 6. 결과 확인
ros2 node list
ros2 topic list

# 7. GitHub에 커밋
git add .
git commit -m "기능 추가: ..."
git push
```

### 클린 빌드
```bash
cd ~/ros2_ws
rm -rf build install log
colcon build
source install/setup.bash
```

---

## 7. 유용한 팁

### 환경 변수 자동 설정
bashrc에 다음을 추가하여 터미널 시작 시 자동 설정:
```bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### GitHub API 토큰 (Notion 연동 예시)
```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": [
        "-y",
        "@suekou/mcp-notion-server"
      ],
      "env": {
        "NOTION_API_TOKEN": "your_token_here"
      }
    }
  }
}
```

---

## 8. 문제 해결

### 빌드 실패
```bash
# 캐시 초기화 및 재빌드
rm -rf build install log
colcon build
```

### 라이브러리 찾을 수 없음
```bash
# 라이브러리 경로 등록
sudo ldconfig

# 또는 LD_LIBRARY_PATH 설정
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
```

### Git 충돌 해결
```bash
# 변경사항 되돌리기
git checkout -- .

# 또는 특정 파일만 되돌리기
git checkout -- 파일명
```

---

**마지막 업데이트**: 2026년 1월 28일
