# nemotron-30b-a3b-atlas-nisten — 원작 사이트 그대로(픽셀 동일) + 오프라인화

## 무엇인가
nisten 의 **LLMViz-Nemotron-3.5-Lightning-30B-A3B** 를 그대로 가져온 사본이다.
시각·동작 코드(3D 씬, 교육 문서 섹션 18개, 컨트롤, 단축키, 모바일 대응)는 **한 줄도 바꾸지 않았고**,
CDN 에 있던 라이브러리 2개만 파일 안으로 인라인해 **인터넷 없이도(GFW 포함) 동일하게 보이도록** 했다.

- 원본: https://github.com/nisten/LLMViz-Nemotron-3.5-Lightning-30B-A3B
- 라이브: https://nisten.github.io/LLMViz-Nemotron-3.5-Lightning-30B-A3B/
- 원작 라이선스: **MIT** — Copyright (c) 2026 netsin (저장소 `LICENSE` 원문 동봉)
  ※ 원작 README 에는 "Apache 2.0" 이라 적혀 있으나 실제 `LICENSE` 파일은 MIT 다 (README 표기가 잘못됨)

## 원본과의 차이 — 정확히 4곳 (diff 검증: 변경 구간 4개 / 시각 코드 0줄)
| # | 위치 | 변경 | 이유 |
|---|---|---|---|
| 1 | `<head>` | 출처·라이선스 주석 8줄 추가 | MIT 고지 유지 |
| 2 | 15행 | `cdnjs` three.js r128 `<script src>` → **파일 내 인라인** | CDN 차단/오프라인 대응 |
| 3 | 17행 | `unpkg` htm/preact ES import → `window.htmPreact` 구조분해 | 모듈 스크립트는 그대로 유지(엄격모드·스코프 보존) |
| 4 | `</body>` 앞 | 비시각 검증 훅 `window.__NM` 16줄 | 화면엔 무관, 실측값 교차검증용 |

인라인한 라이브러리: `three.js r128` (MIT, Copyright 2010-2021 Three.js Authors), `htm 3.1.1 + preact` (MIT).

## 검증 (오프라인 브라우저 실측)
- 콘솔 오류 0 · `#root` 렌더 · 캔버스 838×633 · 범례 8종 · 버튼 9개(정밀도 토글 포함)
- 정밀도 토글: **BF16 65.8 GB ↔ NVFP4 21.5 GB**
- 원작 계산값 ↔ 내가 HF safetensors 헤더에서 실측한 값 교차검증:
  | | 원작(계산) | 실측(헤더) | 일치 |
  |---|---|---|---|
  | BF16 | 65.799 GB | **65.827 GB** | 100.0 % |
  | NVFP4 | 21.515 GB | **21.560 GB** | 99.8 % |
  (콘솔에서 `__NM.check()` 로 확인 가능)

## 여는 법
`index.html` 을 브라우저에서 열면 끝. 707 KB 단일 파일, 서버·CDN·빌드 불필요.

## 같은 모델의 다른 버전
`../nemotron-30b-a3b-atlas/` 는 같은 모델을 **내가 실측 데이터로 새로 만든** 버전이다
(4탭 분해 · 128 전문가 뱅크 · BF16/NVFP4 모드). 이 폴더는 **원작 룩 그대로**가 목적이라 서로 별개다.
