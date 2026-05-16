# AI 시간표 생성기 - 배포 가이드

## 1. 로컬 네트워크 배포 (쉬운 방법)

로컬 네트워크의 다른 기기에서 접근할 수 있도록 배포합니다.

### 1.1 Windows에서 서버 실행

```bash
# 필요한 패키지 설치
pip install -r requirements.txt

# 서버 시작 (포트 8000에서 실행)
python -m uvicorn main:app --host 0.0.0.0 --port 8000
```

### 1.2 로컬 서버의 IP 주소 확인

```bash
# Windows PowerShell에서
ipconfig

# 또는 명령 프롬프트에서
ipconfig
```

**IPv4 주소 찾기:**
- `Ethernet 어댑터` 또는 `무선 LAN 어댑터` 항목 찾기
- `IPv4 주소: 192.168.x.x` 형식의 주소 확인

### 1.3 다른 기기에서 접근

다른 컴퓨터나 스마트폰의 브라우저에서:

```
http://<서버IP주소>:8000
```

**예시:**
- `http://192.168.1.100:8000`
- `http://192.168.0.50:8000`

---

## 2. 클라우드 배포 (무료 옵션)

영구적으로 온라인에서 접근 가능하게 배포합니다.

### 2.1 Render 배포 (권장 - 무료)

**사전 준비:**
- GitHub 계정 필요
- Git 설치

**단계:**

1. **GitHub에 업로드**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/YOUR_USERNAME/timetable-ai.git
   git push -u origin main
   ```

2. **Render 설정**
   - https://render.com 접속 및 가입
   - "New+" → "Web Service" 선택
   - GitHub 저장소 연결
   - 아래 설정 입력:
     - **Name:** timetable-ai
     - **Build Command:** `pip install -r requirements.txt`
     - **Start Command:** `uvicorn main:app --host 0.0.0.0 --port 8000`
     - **Port:** 8000
   - "Create Web Service" 클릭

3. **배포 완료**
   - 몇 분 후 `https://timetable-ai.onrender.com` 형식의 URL이 생성됨

### 2.2 Railway 배포

1. https://railway.app 접속 및 가입
2. "Create New Project" → "Deploy from GitHub"
3. 저장소 선택 및 연결
4. 배포 자동 진행

### 2.3 Heroku 배포 (월간 크레딧 필요)

```bash
# Heroku CLI 설치 후
heroku login
heroku create your-app-name
git push heroku main
```

---

## 3. 문제 해결

### 포트 8000이 이미 사용 중인 경우

```bash
# 다른 포트 사용 (예: 8001)
python -m uvicorn main:app --host 0.0.0.0 --port 8001
```

### 방화벽 차단

- Windows Defender 방화벽에서 Python 허용 필요
- 공유기 포트포워딩 설정 필요할 수 있음

### API 응답 없음

```bash
# CORS 확인 (이미 설정됨)
# 백엔드 로그에서 에러 메시지 확인
# 브라우저 개발자 도구 (F12) → Network 탭에서 API 호출 확인
```

---

## 4. 프로덕션 최적화 (선택)

### 4.1 Gunicorn 사용 (성능 향상)

```bash
pip install gunicorn
gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app --bind 0.0.0.0:8000
```

### 4.2 Nginx 리버스 프록시 (로드 밸런싱)

복잡한 설정이므로 Render나 Railway 같은 플랫폼 권장

---

## 5. 보안 고려사항

- 공개 배포 시 `internal_data.json` 보호 필요
- API 레이트 제한 추가 권장
- HTTPS 사용 (클라우드 플랫폼에서 자동 제공)

---

## 빠른 시작 명령어

```bash
# 1. 로컬 테스트
python -m uvicorn main:app --host 0.0.0.0 --port 8000 --reload

# 2. 프로덕션
python main.py

# 3. 포트 변경
python -m uvicorn main:app --host 0.0.0.0 --port 8001
```

---

## 자주 묻는 질문

**Q: 다른 사람도 웹사이트에 접근할 수 있나요?**
- A: 로컬 네트워크 내에서는 Yes (로컬 배포 사용)
- A: 인터넷 어디서나 접근하려면 클라우드 배포 필요

**Q: 데이터는 안전한가요?**
- A: `internal_data.json`은 서버에서만 읽혀서 노출되지 않음

**Q: 비용이 드나요?**
- A: Render 무료 플랜 (월 750시간)으로 충분
- A: Railway도 무료 크레딧 제공

---

**추가 도움이 필요하면 GitHub Issues에 등록하세요.**
