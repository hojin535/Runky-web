# 🏃‍♂️ Runky : 크루 시스템으로 동기부여를 강화한 실시간 그룹 러닝 서비스

> **"혼자가 아닌 함께 달리는 즐거움, 실시간 데이터 동기화로 꾸준한 러닝 경험을 설계합니다."**

**팀 프로젝트 (Frontend 2, Backend 2, Designer 2)**  

---

## 역할 (Frontend Developer)
- React Native 앱의 **WebView 기반 웹 프론트엔드 UI 개발**
  - 온보딩, 메인, 러닝, 대시보드 화면 구현

- WebView 기반 러닝 UI 전반 설계 및 구현
  - Google Maps Polyline 기반 러닝 경로 시각화
  - 거리·시간·페이스 통계 대시보드 구현

- 백엔드와 실시간 러닝 데이터 포맷·전송 주기 협의
- 디자이너와 인터랙션·애니메이션 단위 협업
  - 보상 UI 및 캘린더 드래그 인터랙션 구현

---

## 기술 스택

- Next.js (App Router)
- TypeScript
- TanStack Query
- Jotai
- Tailwind CSS
- Recharts
- Google Maps API

---

## 주요 기능

- GPS 경로 시각화: Google Maps Polyline을 활용한 실시간 러닝 경로 렌더링

- 대시보드: 거리·시간·페이스 등 주요 지표의 시각화 및 스와이프 인터랙션

- 인터랙티브 UI: 가챠(보상) 애니메이션 및 캘린더 드래그 인터랙션

- WebView 최적화: 모바일 앱 환경에 대응하는 반응형 UI 및 브릿지 통신 구조 구축

---

## 주요 화면

### 러닝 경로 시각화 화면
> 단일 러닝 세션 기준 GPS 경로를 지도 위에 시각화한 화면
<table width="100%">
  <tr>
    <td align="center" width="50%">
      <b>러닝 경로 시각화</b><br/>
      <sub>단일 GPS 경로 시각화</sub>
      <br />
      <img src="./docs/images/러닝%20지도화면.png" width="280" />
    </td>
    <td align="center" width="50%">
      <b>러닝 통계 대시보드</b><br/>
      <sub>거리, 시간, 페이스 정보</sub>
      <br />
      <img src="./docs/images/러닝%20결과.png" width="280" />
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <b>러닝 기록 캘린더</b><br/>
      <sub>월간/주간 기록 조회</sub>
      <br />
      <img src="./docs/images/캘린더.png" width="280" />
    </td>
    <td align="center" width="50%">
      <b>가챠 보상 UI</b><br/>
      <sub>러닝 완료 보상 획득</sub>
      <br />
      <img src="./docs/images/뽑기.png" width="280" />
    </td>
  </tr>
</table>

---


## 기술적 도전사항 & 해결책

### 1. WebView 환경 차트 렌더링 이슈 대응
- WebView 환경에서 ApexCharts 일부 스타일 미적용 이슈 발생
- 렌더링 안정성이 높은 Recharts로 전환하여 크로스 브라우징 문제 해결
- 상세 분석: https://velog.io/@hojin535/react-chart-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC

### 2. TanStack Query 중심 데이터 관리 구조 설계
- 서버 상태 일원화 및 캐싱 전략(staleTime, gcTime) 적용
- API 호출 횟수 **7회 → 2회 (약 71.4% 감소)**
- API 응답 시간 **156ms → 114ms (약 27% 단축)**
- 구조 설계 상세: https://velog.io/@hojin535/TanStack-Query%EB%A1%9C-API-%ED%98%B8%EC%B6%9C-71-%EA%B0%9C%EC%84%A0%ED%95%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0

### 3. 앱-웹 간 통신 최적화

**문제**: React Native 앱과 WebView 간 원활한 데이터 통신 필요



**해결 방안**:

- `postMessageToApp` 유틸리티 함수를 통한 양방향 통신 구조 확립

- Module 기반 메시지 타입 정의로 타입 안정성 확보

- localStorage를 활용한 앱-웹 간 상태 공유 (닉네임, 클로버 개수 등)



```typescript

// utils/apis/postMessageToApp.ts

const postMessageToApp = (module: MODULE, data: string) => {

window.ReactNativeWebView?.postMessage(

JSON.stringify({ module, data })

);

};

```

**성과**:
- 네비게이션 및 데이터 동기화 안정성 확보
