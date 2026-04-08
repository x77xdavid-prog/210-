# 210 Orthopedics 병동관리시스템

<p align="center">
  <img src="apple-touch-icon.png" width="120" alt="210 Orthopedics Logo"/>
</p>

<p align="center">
  <b>210 정형외과 병동의 실시간 병상 현황 관리 웹앱</b><br/>
  Firebase 기반 멀티디바이스 동기화 · PWA 지원 · 무료 운영
</p>

<p align="center">
  <a href="https://x77xdavid-prog.github.io/210-/">🔗 앱 바로가기</a>
</p>

---

## 📋 개요

210 정형외과 병동의 병상 현황을 실시간으로 관리하는 웹 기반 시스템입니다.  
별도 앱 설치 없이 브라우저에서 바로 사용하며, PWA(Progressive Web App)로 홈화면에 추가해 앱처럼 실행할 수 있습니다.  
여러 기기에서 동시에 접속해도 30초마다 자동 동기화됩니다.

---

## ✨ 주요 기능

### 🛏 병상 관리
- 병상별 상태를 한눈에 파악 (총 20병상 기본값, 설정 변경 가능)
- 병상 클릭으로 환자 정보 입력/수정
- 빠른 퇴원 처리 버튼

### 📊 병상 상태 7종
| 상태 | 색상 | 설명 |
|------|------|------|
| 비어 있음 | 회색 | 빈 병상 |
| 입원 중 | 파랑 | 현재 입원 환자 |
| 오전 퇴원 예정 | 노랑 | 12:00 이전 퇴원 |
| 오후 퇴원 예정 | 주황 | 18:00 이전 퇴원 |
| 오후 입원 예정 | 초록 | 당일 오후 입원 예정 |
| 다음날 퇴원 예정 | 보라 | 익일 퇴원 예정 |
| 가퇴원 | 빨강 | 임시 퇴원 상태 |

### 🔔 자동 알림
- **D-1 알림** (빨강): 내일 퇴원 예정 환자 자동 감지
- **장기입원 알림** (노랑): 7일 이상 입원 환자 표시
- **시간 초과 알림** (파랑): 오전 퇴원 예정인데 12시 초과 시 경고
- 알림 발생 시 **소리 알림** (Web Audio API) 자동 재생

### 📋 부가 기능
- **공지사항**: 병동 내 공지 등록 및 긴급 공지 표시
- **수술 일정**: 당일 수술 일정 관리 및 완료 처리
- **통계**: 병상 가동률, 입원 환자 수, 평균 재원일 현황
- **인쇄**: 현재 병동 현황 인쇄
- **퇴원 체크리스트**: 처방전 발행, 보호자 연락, 약 수령 등 6개 항목

### ☁️ 동기화 & 저장
- Firebase Firestore 실시간 동기화 (30초 간격 폴링)
- 화면 복귀 시 즉시 동기화
- 오프라인 상태에서도 로컬 저장 유지
- 저장 상태 표시 (저장 중 / 저장됨 / 저장 실패 / 오프라인)

---

## 📱 PWA 설치 방법

### PC (Chrome / Edge)
1. [앱 주소](https://x77xdavid-prog.github.io/210-/) 접속 후 로그인
2. 주소창 오른쪽 설치 아이콘 클릭
3. "설치" 버튼 클릭 → 바탕화면 아이콘 생성

### 안드로이드 (Chrome)
1. 앱 주소 접속 후 로그인
2. 상단 "홈 화면에 추가" 배너 탭 (또는 ⋮ → 홈 화면에 추가)
3. "추가" 탭

### 아이폰 / 아이패드 (Safari 필수)
1. **Safari** 앱으로 앱 주소 접속 후 로그인
2. 하단 공유 버튼(□↑) 탭
3. "홈 화면에 추가" → "추가" 탭

> ⚠️ iOS에서는 반드시 **Safari**를 사용해야 합니다. Chrome 앱으로는 홈화면 추가가 불가합니다.

---

## 🔧 기술 스택

| 분류 | 기술 |
|------|------|
| Frontend | React 18 (CDN), Tailwind CSS |
| 인증 | Firebase Authentication (이메일/비밀번호) |
| 데이터베이스 | Firebase Firestore |
| 호스팅 | GitHub Pages |
| PWA | Web App Manifest, Service Worker |
| 알림음 | Web Audio API (외부 파일 없음) |

---

## 🔐 보안

### Firebase Security Rules
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /wardData/{userId} {
      allow read, write: if request.auth != null
                         && request.auth.uid == userId;
    }
  }
}
```

- 로그인한 사용자는 **본인 데이터에만** 접근 가능
- 타 계정 데이터 읽기/쓰기 불가
- Firebase 웹 API 키는 클라이언트 공개가 Google의 설계 의도이며, Security Rules가 실질적 보안을 담당

---

## 💰 운영 비용

Firebase **Spark (무료)** 요금제로 운영됩니다.

| 항목 | 무료 한도/일 | 실제 사용량 | 여유 |
|------|-------------|------------|------|
| 읽기 | 50,000회 | 약 2,880회 | 94% |
| 쓰기 | 20,000회 | 수십 회 | 99% |
| 동시 연결 | 100개 | 0개 (폴링) | 완벽 |

---

## 🗂 파일 구조

```
210-/
├── index.html          # 앱 전체 (React + Firebase 포함)
├── manifest.json       # PWA 설정
├── apple-touch-icon.png
├── icon-192.png
├── icon-512.png
└── LICENSE
```

---

## 🚀 로컬 실행

별도 빌드 없이 `index.html`을 서버로 실행하면 됩니다.

```bash
# Python
python -m http.server 8000

# Node.js (npx)
npx serve .
```

브라우저에서 `http://localhost:8000` 접속

---

## 📝 라이선스

MIT License © 2026 x77xdavid-prog