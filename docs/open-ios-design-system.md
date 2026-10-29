# Open iOS Design System — 디자인 가이드라인 (v1.3.1)

Figma 파일: https://www.figma.com/design/Uh7ZcxfLnMibPHshG1bUur/Untitled
기준 기기: iPhone 402 × 874 pt (Dynamic Island), 상태바 62, 하단 세이프 에어리어 34
기반 라이브러리: iOS and iPadOS 27 (상태바), 자체 컴포넌트 (탭바·버튼·카드 등)
분석 원본: Open 페이지의 참고 화면 211장 (노드 1:2 ~ 1:212)

## 1. 분석 요약 (211장에서 읽어낸 것)

| 영역 | 관찰 결과 | 시스템 반영 |
|---|---|---|
| 배경 | 순흑 캔버스 위에 따뜻한 앰버 라디얼 그라데이션. 상단 우측이 가장 밝고 52% 지점부터 검정으로 사라짐. 플레이어는 올리브, 즐겨찾기는 파랑, 프로모는 빨강, 준비 화면은 라임 | `Aura Background` 컴포넌트 5종(Tone=Warm/Olive/Blue/Red/Lime), `Blur/Hero` 효과 |
| 상단 탭 | 두 종류. ① Today의 요일 스트립 S M T W T F S (10pt 자간 넓게, 활성 밑줄 14×2) + 종 아이콘 ② Discover/Community의 큰 텍스트 탭 (Title/3, 활성 밑줄 1pt, 비활성 회색) | `Week Strip Header`, `Day Tab`, `Segment Tab`, `Segment Header` |
| 하단 탭바 | 402×96. 상단이 볼록한 곡선(중앙 0, 양끝 11pt), 반투명 검정 + 블러, 위쪽 그림자. 아이콘 5개(globe, search, Open 마크, pin, 아바타). 활성 탭만 모노 대문자 라벨, 비활성은 60% 투명도 | `Tab Bar` (Active 5변형), `Tab Item` (Type×State), 토큰 `tab-bar/curve-rise` |
| 플로팅 버튼 | ① 56pt 흰 링 + 재생 글리프 + 흰 글로우 ② 60pt 크림 원 + 화살표 (온보딩 다음) ③ 60pt 아웃라인 원 (공유) | `FAB` (Type=Primary/Outline/Play/Pause × State), `Glow/Play Ring`, `Shadow/FAB` |
| 주 버튼 | 56pt 크림 필 알약, 다크 텍스트, 화살표 →. 보조는 1px 아웃라인, 파괴는 빨강, 비활성은 회색 | `Button` (Style 5 × State 4) |
| 타이포 | SF Pro Regular 단일 굵기, 대문자 히어로 44pt, 스탯 56pt Light, 모노 대문자 라벨(DM Mono) | 텍스트 스타일 24개 |
| 색 | 크림 #F0ECE8 단일 액센트, 라임 #C3D44B 성공/활성, 카테고리 색 BREATHE 파랑·MEDITATE 세이지·MOVE 앰버, 태그 BALANCE/ENERGY/REST | Semantic 토큰 70개 (Dark/Light) |
| 시트 | 다크 시트(Technique/Sound)와 종이색 라이트 시트(Credit Store/Cart/Filter), 상단 반경 24, 그래버 36×4 | `Bottom Sheet` (Theme=Dark/Light), Semantic Light 모드는 시트에만 적용 |

## 2. 토큰 구조

세 개의 변수 컬렉션으로 나눈다.

- **Primitives** (모드 1개): 원색 팔레트 `cream/*`, `gray/*`, `amber/*`, `lime/*`, `blue/*`, `sage/*`, `navy/*`, `red/*`, `ios/*`, 간격 `space/2~64`, 반경 `radius/xs~full`, 투명도 `opacity/*`. 피커에서 숨김. 직접 바인딩 금지.
- **Semantic** (Dark / Light): 역할 기반. `color/bg/*`, `color/text/*`, `color/icon/*`, `color/stroke/*`, `color/divider`, `color/accent/*`, `color/status/*`, `color/category/*`, `color/tag/*`, `color/button/<style>/<part>`, `color/chip/*`, `color/control/*`, `color/progress/*`, `color/glow/*`, `color/aura/*`, `color/ios/*`. 모든 값은 Primitives의 별칭이다. 스코프는 역할별로 제한한다.
- **Layout** (iPhone 402): `screen/*`, `safe/*`, `nav/height`, `tab-bar/height 96`, `tab-bar/curve-rise 11`, `margin/screen 16`, `gutter/grid 8`, `gutter/card 12`, `section/gap 32`, `control/height-sm|md|lg 32/44/56`, `fab/size 60`, `fab/play-size 56`, `avatar/* 32~96`, `tile/radius 12`, `card/radius 16`, `sheet/radius 24`.

코드 구문은 모든 변수에 붙어 있다. WEB `var(--color-text-primary)`, iOS `Color.textPrimary` / `Token.marginScreen`, Android `color_text_primary`.

## 3. 네이밍과 계층

- 변수: 소문자, 슬래시 구분, 역할 → 부위 → 상태. 예 `color/button/primary/bg-disabled`.
- 텍스트 스타일: 가족/역할. `Display/Hero`, `Title/1~3`, `Body/Large`, `Label/Section`, `Mono/Tab`. 대문자는 스타일의 textCase로 처리하므로 레이어 텍스트는 문장형으로 둔다.
- 효과 스타일: `Glow/*`, `Shadow/*`, `Blur/*`.
- 컴포넌트: 명사, 가족은 슬래시. `List Row / Class`, `Card / Price`, `Shape / Seed of Life`, `Icon/heart`. 변형 축과 값은 대문자 시작. `Style=Primary, State=Disabled`.
- 프로퍼티: 텍스트는 Label / Title / Meta / Value, 불리언은 Show X, 인스턴스 스왑은 Icon / Trailing Icon / Category / Tag. 이름이 같으면 스왑해도 오버라이드가 유지된다.
- 컴포넌트 내부 레이어는 소문자 서술형: `leading`, `title`, `trailing`, `texts`, `thumbnail`, `scrim`, `glow`, `home-indicator`. 개발자가 찾는 뷰 이름이다.

## 4. 화면 레이어 구조

`Screen Template`(Foundations · Layout & Effects 페이지)에서 시작한다.

```
Screen (402×874, bg canvas)
├─ background        Aura Background 또는 사진 + Blur/Hero (절대 위치, 늘리기)
├─ content (scroll)  세로 오토레이아웃, 패딩 0/16/96/16, 섹션 간격 32
├─ nav-bar (44)      선택. Leading × Trailing 변형
├─ status-bar (62)   iOS 27 라이브러리 인스턴스, 라이브러리 Colors 모드 = Dark
└─ tab-bar (96)      곡선형. 루트 탭 5개에서만. 푸시 화면은 탭바를 빼고 홈 인디케이터만 둔다
```

content 안의 모든 섹션은 Section Header(선택) + 본문이다. 그리드는 8pt 간격의 행, 가로 캐러셀은 clip 해제로 세 번째 카드가 살짝 보이게 한다. 플로팅 액션(FAB, 주 버튼)은 content가 아니라 screen 프레임에 두고 세이프 에어리어에서 16pt 띄운다.

## 5. 상태

- Button: Default / Pressed(85%) / Disabled(회색 필·회색 라벨, 아웃라인은 35%) / Loading(••• 로 라벨 교체)
- Tab Item: Active = 100% 아이콘 + 모노 라벨, Inactive = 60% 아이콘, 라벨 없음. Today는 채움/아웃라인 마크 전환
- Filter Chip: Default(gray 800) / Selected(크림, 다크 텍스트). Category Chip: Active = 흰 텍스트 + 라디얼 글로우, Inactive = tertiary
- Text Field: Empty / Focused(진한 밑줄 + 파란 캐럿) / Filled / Valid(라임 체크) / Error(빨간 밑줄·헬퍼) / Disabled
- Day Tab: Default / Active(밑줄) / Done(라임 체크 원). Segment Tab: Active / Inactive
- 행: Selection Row Default / Selected(체크), List Row / Class Default / Favorited / Completed
- Toggle Off/On, Checkbox Unchecked/Checked(× 글리프), Score Circle Default/Selected, Card / Price Selected/Default, Avatar Placeholder/Photo

## 6. 시그니처 컴포넌트 제작 기준

- **곡선 탭바**: 402×96 프레임에 경로 `M0 11 Q201 −11 402 11 L402 96 L0 96 Z`. 채움 `color/bg/tab-bar` 94% + `Blur/Tab Bar` + `Shadow/Tab Bar`. 콘텐츠는 아래로 스크롤되므로 96pt 하단 인셋. 아이콘 중심 y=34, 라벨 y=56(Mono/Tab), 홈 인디케이터 134×5 y=79.
- **Play FAB**: 56pt, 2pt 흰 링, 재생 글리프 +1pt 오프셋, `Glow/Play Ring`(흰 그림자 14/32 블러 두 겹). 히어로 텍스트 블록 우측에 세로 중앙.
- **Primary FAB**: 60pt 크림 원 + 24pt 화살표, `Shadow/FAB`. 우하단 16pt 인셋, 키보드와 함께 올라감.
- **Aura Background**: 라디얼 베이스(mid → deep → black) + 블러 핫스팟(우상단) + 중간 글로우 + 52%부터 하단 페이드. Tone별 용도: Warm(Today), Olive(플레이어), Blue(즐겨찾기), Red(프로모), Lime(준비).
- **Category Label**: 10pt 대문자 + 카테고리 색 1pt 밑줄. BREATHE 파랑, MEDITATE 세이지, MOVE 앰버, SOUND 크림.
- **Bottom Sheet**: 상단 반경 24, 그래버 36×4, Title/3, Content 슬롯, Save 알약, 하단 패딩 34. Light 테마는 시트 프레임에만 Semantic 모드를 바꾼다.

## 7. iOS 구현 메모

- 폰트: SF Pro는 Typography 페이지의 매핑대로 `UIFont.preferredFont(forTextStyle:)`, DM Mono는 번들. 자간은 kern 속성, 대문자는 렌더 시 `.uppercased()`.
- 색: Semantic 컬렉션에서 `Color` 확장을 생성(Dark/Light → dynamic provider). Primitives는 private static.
- 레이아웃: 패딩·간격은 Layout 토큰에서 읽는다. 콘텐츠 열 370 = 화면 너비 − 2 × marginScreen. 상태바·홈 영역은 safeAreaInsets, 탭바 높이 96은 고정.
- 탭바: `UITabBar.isTranslucent = false` 후 커스텀 배경 뷰(CAShapeLayer 마스크 + `.systemUltraThinMaterialDark` + 검정 94% 오버레이). SwiftUI는 시스템 바를 숨기고 ZStack에 Path를 그린다.
- 효과: 글로우는 shadowRadius 14·32 흰색 70%/35% 두 겹. 오라는 `CAGradientLayer(type: .radial)` + 가우시안 블러, SwiftUI는 `RadialGradient` + `.blur`.
- 이미지: 리스트 썸네일 80 정사각(반경 0), 카드 반경 4, 타일 반경 12. 그라데이션 위 사진에는 0% → 45~75% 검정 스크림.

## 8. Do / Don't

- Do: 화면당 주 액션 하나만 크림 알약으로. 텍스트는 Regular만. 대문자는 Label/Mono 스타일로만. 섹션 헤더 위에 헤어라인. 모든 색은 Semantic 토큰에 바인딩.
- Don't: 두 번째 액센트 색 추가. Bold/Heavy 굵기. 푸시·모달 화면에 탭바. Today 오라 위에 회색 카드. hex·px 하드코딩.

## 9. 파일 페이지 구성

| 페이지 | 내용 |
|---|---|
| 📘 Cover | 제목, 버전, 페이지 색인, 수치, 화면 몽타주 |
| 🧭 Guidelines | 이 문서의 피그마 버전 |
| 🎨 Foundations · Color | Primitives 스와치, Semantic 표(Dark/Light 스와치, 별칭, 코드 구문) |
| 🔤 Foundations · Typography | 24개 텍스트 스타일 표본, iOS 매핑, 규칙 |
| 📐 Foundations · Layout & Effects | 402×874 해부도, Layout 토큰 표, 간격·반경 스케일, 탭바 곡선 스펙, 효과 11종 데모, Screen Template |
| 🖼 Assets · Icons | 24px 라인 아이콘 53종, Logo/Wordmark, Logo/Mark, Avatar/Placeholder |
| 🧩 Components · Navigation | Tab Bar, Tab Item, Nav Bar, Day Tab, Week Strip Header, Segment Tab, Segment Header, Category Chip, Category Row |
| 🧩 Components · Buttons | Button, Button Small, FAB, Icon Button, Text Link |
| 🧩 Components · Inputs & Controls | Text Field, Search Bar, Filter Chip, Tag, Category Label, Toggle, Checkbox, Score Circle, Selection Row, Setting Card, Stepper, Progress Bar, Page Dots |
| 🧩 Components · Content | Section Header, Avatar, List Row (Class/Feed/Settings/Link/Schedule), Stat, Stat Row, Aura Background, Hero / Today, Tile / Collection, Card (Class/Program/Featured/Price/Credit), Banner, Bottom Sheet, Dialog, Toast, Shape ×3, Chart / Life Score Radar |
| 📱 Templates · Screens | Today, Discover, Community, Studio, Profile, Paywall, Settings, Breathwork + Technique 시트, Life Score, Onboarding |

## 11. v1.1 보강 (검토 체크리스트 대응)

### 접근성
- `color/text/tertiary` Dark 값을 gray/500(2.8:1)에서 새 Primitive gray/450 #767676(4.5:1)으로 올렸다. 13pt 이상에서만 사용하고, 10~12pt 라벨에는 보조 단서(숫자, 밑줄)가 있을 때만 쓴다.
- `color/text/disabled`는 gray/500, Light 모드 `text/secondary`는 gray/600, 비활성 버튼 라벨은 gray/300으로 조정했다(4.6:1).
- 포커스 토큰 `color/focus/ring`(2pt, 오프셋 2pt), `focus/ring-width`, `touch/min-target 44`를 추가했다.
- ♿ Foundations · Accessibility 페이지: 실측 대비표, 컴포넌트별 터치 영역, Dynamic Type 확대 한도, 포커스 순서, VoiceOver 라벨·트레이트, 모션 감소·고대비 대응.

### 상태 확장
- Button: Focus 추가(5스타일 × 5상태). FAB: Pressed·Focus 추가. Icon Button: State 축 신설(Default/Pressed/Focus/Disabled). Text Link: Pressed.
- Filter Chip: Pressed·Focus·Disabled. Selection Row: Pressed·Focus. Toggle: Disabled 축. Checkbox: Error·Disabled. Score Circle: Focus. Search Bar: Focused.
- 공통 규칙: Pressed = `color/state/pressed-overlay` 12% 오버레이(+FAB/버튼 scale 0.96), Focus = 흰 2pt 링, Disabled = 35% 투명도.
- 예외 상태 컴포넌트: `State / Message`(Empty·Error·Offline·NoResults), `Skeleton`(List Row·Tile·Card·Text Block), `Toast`(Success·Error·Info).

### 모션
- Motion 컬렉션 13개: duration instant 100 / fast 200 / base 300 / slow 500 / breath-in 4000 / breath-hold 2000 / glow-pulse 2400 / toast 3000 ms, easing standard·decelerate·accelerate·breath, spring/sheet.
- 🎞 Foundations · Motion 페이지: 패턴별 진입·종료·모션 감소 대체표(탭 전환, 푸시, 모달, 시트, 다이얼로그, 토스트, 카테고리 칩 글로우, 히어로, 플레이 링, 하트, 호흡 도형, 스켈레톤, 눌림).

### 반응형
- Layout 컬렉션 모드 iPhone 375 / 402 / 430. 새 토큰 `content/width` 343/370/398, `grid/col-2` 167.5/181/195, `grid/col-3`, `card/scroll-width` 150/160/170, `tab-bar/curve-control-x` 187.5/201/215(곡선 높이 11 유지).
- Layout & Effects 페이지에 고정/가변 요소 표(수평·수직 사이징, 내용이 늘 때 동작)와 조합·중첩 규칙을 추가했다. 가로 모드·iPad는 범위 밖.

### 거버넌스·운영 규칙 (Guidelines §10–13)
- 내비게이션·레이어 z-order: background → content → nav → status → tab bar → floating → toast → sheet → dialog → 시스템 알럿. 뒤로(←)와 닫기(×) 구분, 오버레이는 한 단계만.
- 폼·검증: blur 시 형식 검사, 제출 시 필수값 검사, 전화번호 포맷, 비밀번호 6자, 동의 체크박스 Error, 오류 문구 규칙, 복구 규칙.
- 데이터·콘텐츠: 빈/로딩/실패 처리, 글자 수·줄 수 한도, 숫자·날짜 표기, 이미지 실패 대비, 현지화 30% 여유.
- 버전 체계(MAJOR/MINOR/PATCH), 변경 이력, 폐기 절차(`⛔ Deprecated/` 접두어, 1 MINOR 유지), 예외 승인, 기여 절차.

## 12. v1.2 정밀도 보정 (2× 원본 캡처 실측)

원본 캡처(882px, 2.19배) 두 장을 픽셀 단위로 재어 스케일과 선 굵기를 맞췄다.

| 항목 | 실측 | 이전 값 | 변경 값 |
|---|---|---|---|
| 히어로 제목 ENGAGE | 대문자 높이 25.5pt, 행 간격 37pt | Display/Hero 44/46 | 36/38, 자간 −0.7 |
| 온보딩 제목·이름 | 대문자 높이 16.4pt, 행 간격 24.7 | Title/2 22/28 | 23/26, 자간 −0.3 |
| 히어로 보조 문구(Daily Meditation) | 대문자 10pt, 폭 148pt | Body 15 | Body/Small 14 |
| 카테고리 칩(Meditate) | 대문자 8~9pt | Body/Large 17 | Body/Small 14 |
| 요일 글자 S M T W | 대문자 6pt(≈9pt) | Label/Day 10 Medium | 10 Regular(접근성 하한), 자간 +1.4, 밑줄 11×1 |
| 탭바 라벨 TODAY | 대문자 5.5pt(≈8pt) | Mono/Tab 10 Medium | 10 Regular(접근성 하한), 자간 +1.2 |
| 네비 오른쪽 텍스트 Later → | 대문자 10.5pt | Footnote 13 | Body 15 |
| 라인 아이콘 선 | 2~3px ≈ 1pt | 1.25~1.5pt | 1pt (`icon/stroke`), 둥근 끝 |
| 종 아이콘 | 18×18pt 아치 | 14×12 | 18pt 글리프로 재작성 |
| 플레이 링 | 지름 58, 선 1.5pt | 56 / 2pt | 56 / 1.5pt (`stroke/ring`) |
| 탭바 곡선 | 가장자리 상승 9.6pt | 11 | 10 (`tab-bar/curve-rise`) |
| 홈 인디케이터 | 143×5 | 134 | 140 (`tab-bar/home-indicator-width`) |
| 사진 업로드 원 | 지름 180pt, 1pt 플러스 20pt | 없음 | `Avatar / Upload` 컴포넌트, Onboarding · Photo 템플릿 |

## 13. v1.3 화면별 대조 보정

템플릿 10장을 원본과 같은 배율(2×)로 나란히 놓고 비교해 고쳤다.

| 화면 | 원본에서 확인한 것 | 반영 |
|---|---|---|
| 공통 | 콘텐츠 좌우 여백 11~12pt, 네비 아이콘 여백 20pt, 온보딩 여백 20pt, FAB 우측 10·하단 14 | `margin/screen` 12, `nav/padding-x` 20, `margin/onboarding` 20, `fab/inset-x` 10, `fab/inset-bottom` 14 |
| Today | 요일 글자 간격 24.5pt, 밑줄 11×1 6pt 아래, 히어로 블록이 칩 위 40pt, 칩은 내용 폭 + 간격 42, 플레이 링 58 | 주간 스트립·카테고리 칩·히어로 간격 재조정 |
| Community | 챌린지 아트 139, 참여자 아바타 18pt 겹침(−5), "6 / 31" 26pt, 진행바 2pt, 피드 아바타 40 | Stat-sm 26, Progress Challenge 2pt, Feed 아바타 40, 아바타 스택 |
| Studio | 네비 우측 필터 + 아바타 40, 요일 열 간격 54(좌우 26 패딩), 사진 61×61 r4, 행 피치 94 | Schedule row·일정 스트립 수정, 네비 아바타 추가 |
| Profile | 이름 Title/2, 라벨이 숫자 위에 오는 통계, 퀵카드 152×60 r14, 섹션 헤더 모노, 배너 238 r24, CTA 38 | Stat Size=Profile, Section Header Style=Mono, Banner 재조정 |
| Paywall | 워드마크 35pt 넓은 자간, 카드 패딩 22/30·Body/Large, 셰브론 아이콘, 모노 섹션 헤더, 프로모 라벨 Body/Large 회색, 법적 문구 tertiary, CTA 60pt·하단 32 | 전부 반영 |
| Settings | 제목 위 48 여백, 행 82pt Title/3, 첫 행 위 구분선, 링크 Body 46 피치, 링크 위 92 | List Row / Settings·Link 수정 |
| Breathwork | 세그먼트 쌍이 네비 행 안(중앙), 씨앗 도형 212·선 1.5, 이름 Title/3 + 태그 + 설명 + 좌우 셰브론, 카드 74pt, 플레이 링 82·선 1 | 템플릿 재조립, Setting Card 수정, `Breathwork · Setup` 추가 |
| Technique 시트 | 검정 배경, 평평한 상단, 그래버 없음, 제목 아래 구분선, 행 62pt Title/3, 태그 21pt 9pt 글자 | Bottom Sheet Dark·Selection Row·Tag 수정 |
| Life Score | 네비 제목 Title/3, 부제 Footnote, 레이더 332(점선 링·축 라벨·중앙 글로우), 날짜는 밑줄 탭, SUMMARY 행 Body/Large, 본문 Body/Small 흰색 | Radar 컴포넌트 보강, 템플릿 수정 |
| 온보딩 | 제목 Title/2 24, 입력값 Title/3 20, 밑줄 회색, 헬퍼 없음, FAB 60 우10·하14 | Text Field 오버라이드, FAB 위치 |

발견해서 고친 버그: 섹션 헤더 Mono 변형의 Title 프로퍼티가 중복 생성돼 인스턴스 제목이 바뀌지 않던 문제, 시트 안 태그 인스턴스에 남아 있던 잘못된 채움 오버라이드, 리사이즈한 아바타·FAB가 둥근 사각형이 되던 문제(반경을 `radius/full`에 바인딩).

## 14. v1.3.1 최소 글자 크기 10pt 적용

원본 실측은 요일 9pt·탭 라벨 8pt·태그 9pt였지만, 접근성 페이지의 최소 10pt 규칙을 우선해 `Label/Day`, `Mono/Tab`, `Label/Tag`와 레이더 축 라벨을 모두 10pt로 올렸다. 시스템 안에 10pt 미만 스타일은 없다. 원본과의 차이는 1~2pt이며 자간을 유지해 인상은 같다.

## 10. 알려진 한계

- Design Source 페이지가 비어 있어 타일·카드·배너 이미지는 참고 화면에서 잘라낸 임시 이미지다. 최종 에셋으로 교체해야 한다.
- 상태바만 iOS 27 라이브러리를 쓴다. Open의 탭바는 곡선과 활성 라벨 방식이 시스템과 달라 자체 컴포넌트로 만들었다.
- 키보드·액션시트·알럿은 iOS 27 라이브러리의 `Keyboard`, `Action Sheet`, `Alert (Delete)`를 그대로 쓰면 된다. 템플릿에는 넣지 않았다.
