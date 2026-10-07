# PlanCraft

한국어 · [English](README.md)

가입도 설치도 필요 없는, 브라우저에서 바로 쓰는 **2D → 3D 인테리어 도면 도구**입니다.
도면을 그리고, 가구를 놓고, 벽을 허물고, 치수를 재다가 — 3D로 전환해 걸어 다닐 수 있습니다.

- **계정 없음, 설치 없음.** 모든 처리가 브라우저 안에서 일어나며 작업 내용은 `localStorage`에 저장됩니다.
- **한국어 / English / 일본어 UI.** 첫 방문 시 브라우저 언어를 따르고, 헤더에서 언제든 바꿀 수 있습니다 (선택은 기억됩니다).
- **MIT 라이선스.** [wy51ai/floorplan-3d](https://github.com/wy51ai/floorplan-3d)를 리브랜딩·확장한 포크입니다.

## 프로젝트 구조

```
plancraft/
├─ index.html          # 랜딩 페이지 (ko / en / ja)
├─ app/
│  └─ index.html       # 도면 툴 본체 (단일 파일, 빌드 없음)
├─ assets/
│  ├─ i18n-app.js      # 앱이 사용하는 한/일 사전
│  ├─ favicon.svg
│  └─ og.png
├─ LICENSE             # MIT (원저작자 표기 유지)
└─ README.md
```

## 기능

**2D 도면**

- 원도면을 1:60 / 1:100 축척으로 표시, 단위 mm
- 침실·거실·주방·욕실·가전·서재 카테고리의 가구 60여 종을 라이브러리에서 드래그
- 이동 / 회전(Shift 자유 각도) / 크기 조절, 벽면 자동 스냅
- 벽에 스냅되는 측정 도구, `Shift`로 수평·수직 고정
- 비내력벽 철거 — 내력벽은 별도 표시되어 보호됩니다
- 레이어 온오프: 치수, 방 이름, 가구, 격자, 내력벽

**3D 장면**

- 어버드뷰 / 아이소 / 탑 뷰, 방 목록을 클릭하면 해당 방으로 이동
- 1인칭 산책 모드: 데스크톱은 `WASD` + 마우스, 터치 기기는 가상 조이스틱, 문을 탭해 열고 닫기
- 전체 높이 / 절단 벽, 일광 슬라이더, 야경 조명
- 실제 질감을 살린 가구 모델
- 3D에서도 가구를 선택·드래그, 2D와 실시간 동기화

**도면과 견적**

- 방별 면적과 전용 사용면적 자동 집계
- 방별 바닥재(원목 마루, 타일, 대리석, 테라조, 카펫)를 5% 손실률까지 포함해 견적
- 실행 취소 / 다시 실행, 브라우저 자동 저장
- PNG 이미지 내보내기, 도면 JSON 내보내기 / 가져오기

## 시작하기

```bash
git clone https://github.com/<you>/plancraft.git
cd plancraft
python3 -m http.server 8000
```

그런 다음 아래 주소로 열면 됩니다.

- `http://localhost:8000/` — 랜딩 페이지
- `http://localhost:8000/app/` — 도면 툴

> 도면 툴은 ES 모듈과 import map을 사용하므로 반드시 HTTP로 서빙해야 합니다. 파일 시스템에서 `app/index.html`을 바로 열면 동작하지 않습니다.
> Three.js는 jsDelivr CDN에서 불러오므로 3D 장면을 처음 열 때 인터넷 연결이 필요합니다.

## 단축키

| 키 | 동작 |
| --- | --- |
| `T` | 2D / 3D 전환 |
| `V` / `M` / `X` | 선택 / 측정 / 벽 철거 |
| `R` / `Shift+R` | 선택 항목 90° 시계 / 반시계 회전 |
| `Delete` / `Backspace` | 선택 항목 삭제 |
| `Ctrl/⌘ + D` | 선택 항목 복사 |
| `Ctrl/⌘ + Z`, `Ctrl/⌘ + Shift + Z` | 실행 취소, 다시 실행 |
| `F` | 화면에 맞춤 |
| `+` / `-` | 확대 / 축소 |
| `[` / `]` | 가구 라이브러리 / 속성 패널 표시·숨기기 |
| `Shift + F` | 전체화면 |
| `Esc` | 현재 작업 취소 |
| 산책 모드: `WASD` / 방향키, `Shift`, `E` | 이동, 빠르게 걷기, 문 열기 |

## 도면 바꾸기

모든 도면 데이터는 `app/index.html`에 있습니다.

- `ROOMS` — 방 폴리곤, 이름, 기본 바닥재
- `WALLS` / `WINS` — 벽체와 창洞
- `MATS` — 바닥재 종류와 단가
- `LIB` — 가구 라이브러리 (유형, 이름, 기본 크기, 색상)
- `buildFurniture()` — 가구 종류별 3D 모델

이 데이터만 바꾸면 본인 도면으로 교체할 수 있습니다.

### 현지화

- `app/index.html`은 원본 문자열을 **내부 키**로 사용합니다. `tr(zh, en)`이 `assets/i18n-app.js`에서 조회하고, 번역이 없으면 영문으로 폴백합니다.
- 정적 마크업은 `data-en` / `data-en-title` 속성으로 번역됩니다.
- 내장된 방·가구 이름은 `nm()`을 거치며 사전 → `NAMES_EN` → 원문 순으로 조회됩니다.
- 랜딩 페이지는 `index.html` 안에 별도 사전을 갖고 있으며 `navigator.language`로 언어를 감지합니다.

## 배포

정적 사이트이므로 어떤 정적 호스팅에서도 동작합니다. Vercel에서는 저장소를 임포트하고 프레임워크 프리셋을 **Other**로 둔 뒤 배포하면 됩니다. 빌드 커맨드도 출력 디렉터리도 필요 없습니다.

## 크레딧

- [wy51ai/floorplan-3d](https://github.com/wy51ai/floorplan-3d)를 기반으로 함 — MIT 라이선스
- [Three.js](https://threejs.org/) r160 (OrbitControls, PointerLockControls, RoundedBoxGeometry, RoomEnvironment, CSS2DRenderer)
- 폰트: Bricolage Grotesque, IBM Plex Mono, Noto Sans JP, Pretendard

## 라이선스

[MIT](LICENSE)
