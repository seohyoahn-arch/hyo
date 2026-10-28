# Open iOS Design System — 디자인 가이드라인

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

## 10. 알려진 한계

- Design Source 페이지가 비어 있어 타일·카드·배너 이미지는 참고 화면에서 잘라낸 임시 이미지다. 최종 에셋으로 교체해야 한다.
- 상태바만 iOS 27 라이브러리를 쓴다. Open의 탭바는 곡선과 활성 라벨 방식이 시스템과 달라 자체 컴포넌트로 만들었다.
- 키보드·액션시트·알럿은 iOS 27 라이브러리의 `Keyboard`, `Action Sheet`, `Alert (Delete)`를 그대로 쓰면 된다. 템플릿에는 넣지 않았다.
