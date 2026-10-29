# Suno iOS Design System — 디자인 가이드라인

Figma 파일: https://www.figma.com/design/L8QrbkFEPcfSiB8KLq5SBA/Untitled
기준 기기: iPhone 402 × 874 pt (Dynamic Island), 상태바 62, 하단 세이프 에어리어 34
기반 라이브러리: iOS and iPadOS 27 (상태바·홈 인디케이터·키보드), 자체 컴포넌트 (상단바·탭바·FAB·카드 등)
분석 원본: IMG 페이지의 Suno iOS(2026-10) 화면 131장 (노드 1:2 ~ 1:132), 이미지 소스는 Design Source 페이지
토큰 내보내기: `docs/suno-ios-tokens.json`

## 1. 분석 요약 (131장에서 읽어낸 것)

| 영역 | 관찰 결과 | 시스템 반영 |
|---|---|---|
| 배경 | 기본은 #101012 단색. 생성·재생·인증 화면은 상단 #1D1512 → 중간 #35231E → 하단 #120E0B의 따뜻한 세로 그라데이션 위에 미세 입자. 프로필은 주황 #DE6E48 커버가 42% 지점에서 투명으로. 스플래시·AI 결과는 핑크·오렌지·앰버 그라데이션 + 입자, Hooks 피드는 파랑 | `Background` 컴포넌트 5종(Type=Canvas/Warm/Cover/Brand/Cool), 스톱은 `gradient/*` 토큰, `Grain/*` 효과 |
| 상단 바 | 54pt. 루트 탭은 28pt Bold 큰 제목 + 우측 44 원형 유리 버튼(search·bell), 하위 화면은 뒤로가기 + 가운데 17pt Semibold 제목, 홈은 SUNO 워드마크, Create는 "Advanced ▾" 드롭다운 + "v6-mini ▾" 모델 필 | `Top Bar` (Type=Large Title/Centered/Logo/Dropdown), `Icon Button` Glass, `Model Pill` |
| 하단 탭바 | 402×83(49 + 홈 인디케이터 34). 아이콘 전용 5칸(home·search·create·library·아바타). 활성 탭은 52×40 곡선형 캡슐(#161618 + 12% 외곽선 + 상단 하이라이트), 비활성 아이콘은 회색 #6E6E73. Library 탭에 핑크 숫자 배지 | `Tab Bar` (Active 5 × Style Flat/Floating), `Tab Bar Item` (Type×State), `layout/tab-pill-*` |
| 플로팅 버튼 | Create 화면 하단에 56pt 캡슐(핑크→오렌지→앰버 그라데이션 + 입자 + 그림자 + 핑크 글로우) + 좌측 44 유리 휴지통. 생성 중에는 "Creating…" + 스피너. 프롬프트 전송은 32 핑크 원형 | `FAB Create` (State 4), `FAB Mini`, `Composite/FAB Create` 효과 |
| 미니 플레이어 | 탭바 위 64pt 유리 바, 썸네일 44 + 제목/아티스트 + Remix + Play/Pause, 하단 2px 진행 바 | `Mini Player` (State=Playing/Paused/Loading), `Progress Bar` Hairline |
| 버튼 | 크림 #F7F5F0 알약(1차) · 브랜드 그라데이션(Continue/Confirm) · 톤 #2C2C2E · 아웃라인 · 붉은 텍스트(파괴). Large 52, Medium 44, Small 32 | `Button` (Style 5 × Size 2 × State 3), `Button Small` (Style 6 × State 3) |
| 타이포 | SF Pro 4굵기, 제목형 질문·요금제 헤드라인만 세리프, 모델명·시간·버전은 고정폭 대문자 | 텍스트 스타일 29개 (Display 2 · SF Pro 24 · Mono 3) |
| 색 | 상태 강조는 핑크 #FF3E9A 한 색(토글·재생 중·배지·커서). 요금제 선택 테두리는 오렌지, Most Popular 초록, Best Value 노랑, 파괴 빨강 | Primitives 54 · Semantic 59 (Dark/Light) |
| 시트·메뉴 | 상단 반경 24 다크 시트 + 그래버 36×5, 318 폭 다이얼로그, 220 폭 컨텍스트 메뉴(마지막 행 빨강) | `Sheet / Bottom`, `Dialog / Alert`, `Menu / Context`, `Share Item` |

## 2. 토큰 구조

아홉 개의 변수 컬렉션으로 나눈다. 번호는 참조 순서다.

- **01 Primitives** (모드 1개, 64개): `neutral/0~1000`, `brand/*`(violet 포함), `warm/*`, `cover/*`, `cool/*`, `system/*`, `alpha/*`(white·black·magenta·warm·orange·amber·pink), `external/*`. 피커에서 숨김(scopes 없음). 직접 바인딩 금지.
- **02 Semantic Color** (Dark / Light, 71개): `bg/*`(단색 surface-1~3과 반투명 layer-1~3·brand-tint), `fg/*`, `icon/*`, `border/*`(layer 포함), `brand/*`, `control/*`, `gradient/*`(warm·cool·cover·brand-stroke-start/mid/end), `shadow/*`, `highlight/*`, `grain/*`, `glow/*`. 모든 값은 Primitives의 별칭이고 스코프가 역할별로 제한된다(bg → Fill, fg → Text, icon → Shape·Stroke, border → Stroke, shadow → Effect).
- **03 Spacing**: `space/0~64` (GAP). 4pt 배수, 2·6·10은 아이콘-텍스트 사이 미세 간격 전용.
- **04 Radius**: `radius/xs 4 · sm 8 · md 12 · lg 16 · xl 20 · 2xl 24 · 3xl 32 · full 999`.
- **05 Size**: `icon/*`, `control/*`, `avatar/*`, `thumb/*`, `layout/*`(screen 402×874, status-bar 62, top-bar 54, tab-bar 49, home-indicator 34, mini-player 64, margin 16, gutter 12, content-width 370, fab-height 56, cta-height 52, toggle 51×31, tab-pill 52×40, card-category 120×90), `stroke/*`.
- **06 Typography**: `font/family/*`, `font/weight/*`, `font/size/*`, `font/line-height/*`. 텍스트 스타일이 이 변수에 바인딩된다.
- **07 Motion**: `duration/instant 0.1 · fast 0.2 · base 0.3 · slow 0.45 · gradient-loop 6`, `delay/none 0 · short 0.05 · stagger 0.03`, `easing/standard · emphasized · spring`.
- **08 Opacity**: `opacity/disabled 40 · pressed 70 · inactive 60 · overlay 60 · glass 12 · full 100` (퍼센트).
- **09 Z-Index**: `z/background 0 · content 10 · sticky-header 20 · top-bar 30 · mini-player 40 · tab-bar 50 · fab 60 · banner 70 · overlay 80 · sheet 90 · menu 100 · dialog 110 · toast 120 · system 1000`. 피커에는 나타나지 않는 참고용 값.

코드 구문은 모든 변수에 붙어 있다. WEB `var(--suno-bg-canvas)`, iOS `SunoColor.bgCanvas` / `SunoSpacing.s16` / `SunoRadius.lg` / `SunoSize.iconMd`, Android `sunoBgCanvas`.

## 3. 네이밍과 계층

- 변수: 소문자, 슬래시 구분, 그룹 → 역할 → 변형. 예 `bg/surface-2`, `fg/on-brand`, `layout/tab-pill-height`.
- 텍스트 스타일: 역할/굵기. iOS Dynamic Type 역할명을 그대로 쓴다. `Title 1/Bold`, `Subheadline/Medium`, `Mono/Label`, `Display/Serif XL`.
- 효과 스타일: `Elevation/Level 1~3`, `Glow/Brand`, `Glow/Brand Inner`, `Glass/Regular·Strong`, `Grain/Brand Gradient·Background`, `Composite/FAB Create`.
- 컴포넌트: 카테고리 / 이름. `List Row / Song`, `Card / Pricing`, `Sheet / Bottom`, `Icon/Nav/home`. 변형 축은 Style · Size · State · Type 네 가지, 값은 대문자 시작. `Style=Brand, Size=Large, State=Pressed`.
- 프로퍼티: 텍스트는 내용명(Label · Title · Subtitle · Meta · Message), 불리언은 `Show ○○`, 인스턴스 스왑은 `Icon` / `Leading Icon`. 중첩 버튼·아바타·배지는 노출 인스턴스로 열어 두었다.
- 템플릿 레이어 이름은 개발 뷰 이름과 같다: `Background`, `Content (Scroll)`, `Top Bar`, `Mini Player`, `Tab Bar`, `FAB · Create`, `Status Bar`.
- 아이콘: `Icon/카테고리/이름` 24×24, 스트로크 1.75, 색은 `icon/primary` 바인딩. iOS 에셋 이름은 `icon_nav_home`.

## 4. 화면 레이어 구조

Templates 페이지의 8개 화면(Home · Library · Create · Now Playing · Profile · Settings · Auth · Phone Number)이 기준이다.

```
Screen (402×874)
├─ Background        Background 컴포넌트 인스턴스 (절대 위치, Stretch/Stretch)
├─ Content (Scroll)  세로 오토레이아웃, 패딩 16/16/16/24, y 116, 높이 = 874 − 116 − 83(− 64 미니플레이어)
├─ Top Bar (54)      y 62, 상단 Stretch. 스크롤 시 Glass/Regular 블러
├─ Mini Player (64)  y 727, 하단 Stretch (재생 중일 때만)
├─ Tab Bar (83)      y 791, 하단 Stretch. 루트 5탭에서만, 푸시·모달은 홈 인디케이터만
├─ FAB · Create (56) 미니플레이어/키보드 위 16, Fill 폭
└─ Status Bar (62)   iOS 27 라이브러리 인스턴스, 프레임의 라이브러리 Color Scheme 모드 = Dark
```

콘텐츠 폭 370 = 402 − 16×2. 2열 그리드 카드 폭 179(거터 12). 가로 스크롤 행은 좌측 16에서 시작해 우측에서 잘린다. 키보드가 올라오면 CTA는 키보드 위 16에 고정하고 탭바를 숨긴다(Phone Number 템플릿, 숫자 키패드 308).

## 5. 상태

- Button / Icon Button / Chip / FAB: Default · Pressed(opacity 70%, 스케일 0.98) · Disabled(opacity 40%, 그라데이션·그림자 제거, 텍스트 fg/disabled). FAB는 Loading(스피너 + "Creating…", 그라데이션 6s 루프) 추가.
- Tab Bar Item: Default(icon/inactive) · Active(곡선형 캡슐 bg/tab-active + border/default + 상단 하이라이트, icon/primary, 아바타는 Ring) · Pressed(bg/glass). 전환 0.2s spring.
- List Row / Song: Default · Playing(제목 fg/accent + 이퀄라이저) · Locked(자물쇠 + "V6 PREVIEW" + Unlock 버튼) · Loading(스켈레톤). Show Unread로 좌측 핑크 점.
- Search Field / Prompt Field: Default(플레이스홀더 fg/tertiary) · Focused(핑크 커서, 링 없음) · Filled(값 + 지우기, 전송 버튼 활성).
- Text Field(Default/Centered/Multiline): Default · Focused · Filled · Error(테두리·헬퍼·아이콘 fg/destructive, 글자 수 초과 시) · Disabled(40%). 플레이스홀더는 `Placeholder` 프로퍼티(Default·Disabled에만 바인딩), 입력값은 `Text` 프로퍼티(Focused·Filled·Error)로 나눠 변형마다 다른 글이 보인다. 같은 이유로 Progress Banner는 `Loading Message`·`Ready Message`, Empty State는 `Error Title`·`Error Description`을 따로 가진다. 카운터 "12/60"은 제한이 있을 때 항상 표시. Code Input: Default(첫 칸 커서) · Filled · Error(헬퍼 "That code didn’t work").
- Empty State(Default/With Action/Error): 검색 초기·결과 없음·네트워크 오류는 토스트가 아니라 콘텐츠 영역의 Empty State로. 로딩은 스켈레톤. Avatar·Thumbnail은 Type=Placeholder로 이미지 실패를 대체한다.
- Toggle On/Off(+Disabled), Segmented Control Left/Right(+SAVE 20% 배지), Underline Tabs Selected 1~3, Card / Pricing Selected(border/brand 2px), Card / Creator Follow/Following, Mini Player Playing/Paused/Loading, Toast Light/Dark, Progress Banner Loading/Ready.
- 햅틱: 탭 전환 .selection, 토글 .light, 생성 시작 .medium, 오류 .error.

## 6. 시그니처 컴포넌트 제작 기준

- **상단 바**: 402×54, 좌우 16, 버튼 44 원형(bg/glass-strong + border/default + Glass/Regular), 아이콘 20. Large Title은 Title 1/Bold 좌측 정렬 + 우측 버튼 2개, Centered는 Headline/Semibold 절대 중앙, Dropdown은 Title 3/Semibold + caret 14 중앙 + Model Pill 우측.
- **하단 탭바**: Flat은 bg/tab-bar + 상단 1px border/subtle, 탭 행 49 안에 52×40 캡슐 5개를 좌우 16 패딩으로 균등 배치, 아래 홈 인디케이터 34. Floating은 370×64 캡슐(bg/glass-strong + Glass/Regular + Level 2)을 홈 인디케이터 위에 띄운다. 활성 캡슐 반경 16(continuous).
- **FAB Create**: 행 370×56, 좌측 Trash(Glass Icon Button 44, Show Trash), 메인 캡슐 Fill. 캡슐 프레임은 채움이 없고 자식 `Gradient Surface`(absolute·stretch·잠금) 한 장이 채움 6겹과 그레인을 가진다(레시피는 7장). 프레임 효과 = Composite/FAB Create(Level 3 그림자 + 핑크 글로우). 레이블 Body/Semibold + 아이콘 20은 Surface의 형제라서 그레인이 묻지 않는다.
- **브랜드 아웃라인 캡슐**(Button Style=Brand Outline, Button Small Brand Outline, Model Pill): 채움 bg/brand-tint(오렌지 16%) + 1px 안쪽 테두리를 135° 그라데이션(gradient/brand-stroke-start → mid 55% → end) + Glow/Brand Inner(안쪽 16 blur 웜 글로우 + 상단 1px 림). Pressed 70%, Disabled는 채움 없이 border/subtle.
- **레이어 오버레이**(Card / Layer Group + List Row / Option): 웜·쿨 그라데이션 배경 위에서는 단색 surface 대신 반투명 층을 쌓는다. 카드 bg/layer-1(반경 lg, 패딩 12/16) → 행 bg/layer-2(높이 52, 반경 md) → 행 안의 컨트롤 bg/layer-3. 각 층은 border/layer 1px 안쪽 헤어라인. Create 화면의 Lyrics·Styles·More Options 카드가 이 규칙을 쓴다.
- **Background Warm**: bg/canvas 베이스 + 세로 그라데이션(gradient/warm-top 0 → warm-mid 0.45 → warm-bottom 1) + 중앙 라디얼 글로우(gradient/warm-glow → 0) + Grain/Background(모노톤 grain/surface 10%, size 1, density 45%). Cover는 gradient/cover 0 → 60% 0.22 → 투명 0.42. Brand는 대각 3색 + 블롭 2개 + Grain/Brand Gradient.
- **미니 플레이어**: 402×64, 패딩 12/8, bg/glass-strong + Glass/Regular + 상단 hairline, 썸네일 SM 44(길이 배지 숨김), 제목 Subheadline/Medium 1줄, 아티스트 Footnote, Ghost Icon Button 44 두 개, 하단 Progress Bar Hairline 2px.
- **시트**: 402 폭, 상단 반경 24, bg/surface-1 + Level 3, 그래버 36×5 bg/surface-3, Header 타입은 Headline 제목 + 우측 Glass 닫기 버튼, Content 슬롯(인스턴스에서 자유 배치).

## 7. 레이어 오버레이 · 그레인 · 브랜드 그라데이션 레시피

Layout & Effects 페이지의 "Surface Layers · Grain · Brand Gradient" 섹션과 같은 내용이다. 이 세 가지는 Suno 화면의 질감을 정하는 고정 레시피라서 값을 바꾸지 않고 토큰만 바꾼다.

**반투명 층(오버레이)**

| 층 | Dark | Light | 쓰는 곳 |
|---|---|---|---|
| bg/layer-1 | 흰색 4% | 검정 6% | 그라데이션 배경 위 카드 (반경 lg) |
| bg/layer-2 | 흰색 6% | 검정 8% | 카드 안의 행·그룹 (높이 52, 반경 md) |
| bg/layer-3 | 흰색 8% | 검정 12% | 행 안의 컨트롤(칩·미니 필) |
| bg/layer-pressed | 흰색 12% | 검정 12% | Layer 컨트롤의 눌림(한 단계 밝게) |
| border/layer | 흰색 6% | 검정 6% | 모든 층의 1px 안쪽 헤어라인 |

- 최대 3단까지만 쌓고, 같은 단을 같은 단 위에 올리지 않는다. 층에는 블러를 걸지 않는다(Glass/*는 스크롤 위를 떠다니는 바 전용).
- 평면 캔버스(Library·Settings·Home)의 카드는 그대로 단색 bg/surface-1~3을 쓴다. layer-*는 Warm·Cool·Cover 배경 위에서만 쓴다.
- 컴포넌트: `Card / Layer Group`(헤더 + Content 슬롯)과 `List Row / Option`(Control=Slider·Segmented). 템플릿 `Create · More Options`가 실제 쌓임 예시다.
- 컨트롤도 같은 규칙을 따른다. Icon Button·Chip·Button Small은 `Style=Layer`(bg/layer-3, Pressed는 투명도 대신 bg/layer-pressed로 한 단계 밝게, Disabled 40%), Segmented Control은 `Style=Layer`(bg/layer-2 컨테이너 + 선택 세그먼트 bg/layer-3), Card / Creator는 `Surface=Layer`(bg/layer-1 + border/layer). 평면 캔버스에서는 Filled·Tonal·Solid를 그대로 쓴다. FAB Create Disabled는 bg/layer-2 + border/layer + fg/tertiary로, 참고 화면처럼 읽히는 회색 라벨을 유지한다.
- 트랙과 눈금: control/track을 흰색 24%(라이트 검정 12%)로 바꿔 웜 배경·카드 위에서도 같은 밝기 차를 낸다. 시트 그래버는 control/grabber(흰색 24%).

**브랜드 그라데이션 면(Gradient Surface)**

| 순서 | 채움 | 값 |
|---|---|---|
| 1 | 단색 | brand/orange |
| 2 | 선형 0° | brand/pink 0% → brand/orange 32% → brand/amber 85~100% |
| 3 | 라디얼 | brand/amber 85%, 중심 (62%, 5%), 반경 42%×150% |
| 4 | 라디얼 | brand/pink 90%, 중심 (4%, 75%), 반경 16%×110% |
| 5 | 라디얼 | brand/magenta 85%, 중심 (94%, 100%), 반경 16%×100% |
| 6 | 라디얼 스크림 | 검정 22%, 중앙, 반경 71%×21% (레이블 대비용) |
| 효과 | Grain/Brand Gradient | 듀오톤 노이즈: grain/brand-light(앰버) 30% + grain/brand-dark(딥 핑크) 26%, size 1.1, density 50% |

- 구조: 버튼 프레임은 채움이 없고, 자식 `Gradient Surface`(absolute, stretch, 잠금)가 위 7가지를 가진다. 라벨·아이콘은 Surface의 형제라서 그레인이 글자에 묻지 않는다. Pressed는 프레임 opacity 70%, Disabled는 bg/control 단색(그림자·그레인 없음).
- 적용: Button Style=Brand(Large·Medium), FAB Create Main, Background Type=Brand.

**브랜드 아웃라인 캡슐**(Unlock for full access, v6-mini 모델 필)

- 채움 bg/brand-tint(Dark 오렌지 16% / Light 살구 10%) + 1px 안쪽 테두리 135° 그라데이션(gradient/brand-stroke-start 오렌지 → mid 55% 핑크 → end 바이올렛) + Glow/Brand Inner(glow/brand-inner 안쪽 16 blur + 흰색 10% 상단 1px 림). Pressed 70%, Disabled는 채움 없이 border/subtle.

**그레인 규칙**

- Grain/Background: 모노톤 grain/surface(흰색 10%), size 1, density 45%. Warm·Cool 전체 배경에만.
- Grain/Brand Gradient: 위 듀오톤. 브랜드 그라데이션 면에만.
- 금지: 텍스트·아이콘·사진 위, layer-* 카드 면. 노이즈는 스크롤·애니메이션에 따라 흔들리지 않게 고정한다.

## 8. iOS 구현 메모

- 색: Asset Catalog에 Semantic 이름 그대로 Dark/Light appearance 두 값. `enum SunoColor { static let bgCanvas = Color("bg/canvas") }`. Primitives는 private.
- 폰트: `Font.suno.body` 같은 역할 extension으로 감싼다. SF Pro는 `.system(size:weight:)`, 세리프는 `Font.custom("InstrumentSerif-Regular")`, 모노는 `"JetBrainsMono-Regular"`. Dynamic Type은 `.relativeTo(.body)`.
- 레이아웃: 패딩·간격은 `SunoSpacing`, 반경은 `RoundedRectangle(cornerRadius:, style: .continuous)`, 구조 상수(`layout/*`)는 `SunoSize`에서 읽고 safeAreaInset 계산에 쓴다.
- 탭바: 시스템 탭바를 숨기고 커스텀 뷰를 `.safeAreaInset(edge: .bottom)`에 둔다. 활성 캡슐은 `matchedGeometryEffect(id: "tab")`, Floating은 iOS 26+ `.glassEffect(.regular, in: Capsule())`.
- 효과: 그림자는 `.shadow(color:radius:y:)` 두 겹(FAB), 유리는 `.ultraThinMaterial` 또는 `.glassEffect`. 그레인은 1× 노이즈 타일(64×64 PNG, `resizable(resizingMode: .tile)`)을 그라데이션 뷰에만 `.overlay`로 얹는다. 브랜드 면은 앰버 타일 30% + 딥 핑크 타일 26% 두 장(듀오톤), 배경은 흰색 타일 10% 한 장. 라벨은 그 위의 별도 뷰. 반투명 층은 `.background(SunoColor.bgLayer1)`처럼 알파 색을 그대로 겹친다(블러 없음). 브랜드 테두리는 `Capsule().strokeBorder(LinearGradient(...), lineWidth: 1)` + `.shadow` 대신 안쪽 글로우는 `Capsule().fill(RadialGradient)` 오버레이로 근사.
- 모션: `Animation.timingCurve(0.4, 0, 0.2, 1, duration: 0.3)` 기본, 탭 캡슐·FAB 복귀는 `.spring(response: 0.35, dampingFraction: 0.7)`. Reduce Motion에서는 그라데이션 루프와 스프링을 끈다.
- 아이콘: SVG 템플릿 에셋 24pt, `renderingMode(.template)` + `icon/*` 색.

## 9. 접근성 · 반응형 · 예외 처리 요약

- 대비(Dark): fg/primary 18.9:1, fg/secondary 6.8:1, fg/tertiary 5.2:1(#85858A), fg/accent 5.8:1. icon/inactive(#6E6E73)는 3.7:1로 아이콘에만 쓴다. Light: fg/secondary 6.1:1, fg/tertiary 4.7:1.
- 브랜드 그라데이션 위 흰 레이블은 레이블 뒤 검정 24% 라디얼 스크림을 포함한 상태에서 핑크·오렌지 구간 5.4:1. 앰버 구간에는 레이블을 두지 않는다(그라데이션 방향 좌→우 고정).
- 터치 44, 32 요소는 히트 영역 확장. VoiceOver 순서: 상태바 → 상단바(좌 → 제목 → 우) → 콘텐츠(위→아래) → 미니플레이어 → 탭바(좌→우). 아이콘 전용 버튼은 accessibilityLabel 필수, 재생 중·잠금은 색 + 아이콘으로 이중 표시.
- Dynamic Type: 본문·리스트·시트는 AX5까지 Hug로 자라고, Top Bar 제목·버튼·칩은 xxLarge~xxxLarge에서 제한, 탭바·아이콘 버튼·FAB는 고정. Reduce Motion에서 그라데이션 루프·스프링을 끈다.
- 반응형: 가로 여백 16 고정, 콘텐츠 폭 = 화면 폭 − 32(375→343, 393→361, 402→370, 430→398). 열 수 고정, 카드 폭만 변함. 컴포넌트별 Fixed/Hug/Fill·Min/Max·넘침 규칙은 Layout & Effects 페이지 Resizing 표 기준.
- Light 모드: Library·Create·Settings 템플릿을 Light로 전환해 검증했다(Templates 페이지 하단). 글로우 색은 gradient/warm-glow 토큰으로 분리했고, 그 외 토큰은 그대로 동작한다. 제품은 Dark만 출시한다.

## 10. Do / Don't

- Do: 화면당 브랜드 그라데이션 버튼 하나. 상태 강조는 핑크 한 색. 유리 면은 뒤에 콘텐츠가 있을 때만. 리스트 제목 1줄 말줄임. 모든 색·간격·반경은 토큰 바인딩. 터치 44 유지.
- Don't: Primitives 직접 바인딩. 두 번째 강조색 추가. 탭바에 라벨 텍스트·탭 수 변경. Disabled에 그림자·글로우. 컴포넌트 detach. hex·px 하드코딩.

## 11. 파일 페이지 구성

| 페이지 | 내용 |
|---|---|
| 📘 Cover | 제목, 버전, 페이지 색인, 템플릿 미리보기 3장 |
| 📖 Guidelines | 이 문서의 피그마 버전(원칙·구조·토큰·레이아웃·컴포넌트·상태·모션·접근성·핸드오프·변경 관리) |
| 🎨 Foundations · Color | Primitives 스와치 64, Semantic 표 71(Dark/Light 스와치, 용도, hex, iOS 구문; grain·glow 그룹 포함), 배경 5종, 사용 규칙 |
| 🔤 Foundations · Typography | 서체 3종, 타입 스케일 29 표본과 iOS 매핑, 규칙 |
| 📐 Foundations · Layout & Effects | 402×874 해부도, Spacing·Radius·Size 표, 효과 10종 데모, Surface Layers·Grain·Brand Gradient 레시피(층 데모·채움 7겹 표·그레인 표본), Motion(delay 포함)·Opacity, iPhone 폭별 규칙, 컴포넌트별 리사이징 표, Z-Index, Dynamic Type 표 |
| ✦ Icons | 24px 아이콘 83종 (Nav 5 · Media 16 · Action 38 · Object 20 · Social 4) |
| 🧩 Navigation | Icon Button(30, Layer 포함) · FAB Mini(3) · FAB Create(4) · Model Pill · Top Bar(4) · Tab Bar Item(6) · Tab Bar(10) · Mini Player(3) · Toast(2) · Progress Banner(2) · Background(5) |
| 🧩 Buttons & Controls | Avatar(24, Photo/Placeholder) · Badge(3) · Thumbnail(8, Image/Placeholder) · Toggle(4) · Chip(15, Layer 포함) · Button(36, Brand Outline 포함) · Button Small(21, Layer 포함) · Segmented Control(4, Solid/Layer) · Underline Tabs(5) · Search Field(3) · Prompt Field(3) · Text Field(15) · Code Input(3) · Step Slider(3) · Progress Bar(2) · Page Control(5) |
| 🧩 Cards · Lists · Overlays | List Row / Song(4) · Setting(12) · Menu(3) · Option(2) · Card / Layer Group · Section Header(2) · Comment Row · Card / Category(2) · Song Poster(2) · Pricing(2) · Creator(4, Surface Solid/Layer) · Promo Banner · Sheet / Bottom(2) · Dialog / Alert · Menu / Context(3) · Share Item(5) · Empty State(3) · Tooltip(2) |
| 📱 Templates | Home · Library · Create · Now Playing · Profile · Settings · Auth · Onboarding · Phone Number · Create · More Options(레이어 오버레이 예시) + Light mode check 3장(Library·Create·Settings 검증용) |
| — Reference — | IMG(원본 131장) · Design Source(iOS 27 예시, 이미지 소스) |

## 12. 알려진 한계

- 세리프는 Figma 환경에 New York이 없어 Instrument Serif로 대체했다. iOS에서는 New York(`.serif` design) 또는 Instrument Serif 번들 중 하나로 통일해야 한다.
- 템플릿의 사진은 Design Source 페이지의 예시 이미지(cosmos·여행·공연)다. 실제 앨범 아트로 교체해야 한다.
- 브랜드 그라데이션의 유기적인 블롭 형태는 라디얼 그라데이션 3개 + 스크림 + 듀오톤 노이즈로 근사했다(7장). 실제 앱의 애니메이션 셰이더(6s 루프)는 모션 토큰으로만 정의했다. Figma의 노이즈 효과는 1× 내보내기에서 입자가 뭉개질 수 있으니 2× 이상으로 검토한다.
- iOS 27 라이브러리는 상태바·홈 인디케이터·키보드만 쓴다. 탭바와 상단바는 Suno의 캡슐 활성 표시와 유리 버튼이 시스템과 달라 자체 컴포넌트로 만들었다.
- Light 모드 토큰은 정의만 되어 있고 템플릿은 모두 Dark다.

## 13. 변경 관리

- 버전: Cover의 버전 문자열, 이 문서, `suno-ios-tokens.json`의 `$meta.version`을 함께 올린다. 토큰 이름 변경은 코드 구문도 같이 바꾼다.
- 폐기: 토큰·컴포넌트를 없앨 때는 이름 앞에 `_deprecated/`를 붙이고 description에 대체 항목과 제거 예정 버전을 적는다. 한 마이너 버전 동안 유지한 뒤 제거한다. 변형 축을 줄일 때는 인스턴스를 먼저 새 변형으로 옮긴다.
- 추가: 새 색은 Primitives에 원시값, Semantic에 역할 이름으로. 변형 축은 Style·Size·State·Type만 쓰고 30 조합을 넘으면 컴포넌트를 나눈다. 아이콘은 `Icon/카테고리/이름` 24×24.

### 변경 이력

- 1.2.1 (2026-10-29): 웜 배경 위에 단색으로 남아 있던 컨트롤을 반투명 Layer 스타일로 통일. Icon Button·Chip·Button Small `Style=Layer`, Segmented Control `Style` 축(Solid/Layer), Card / Creator `Surface` 축(Solid/Layer), FAB Create Disabled 반투명화. 토큰 bg/layer-pressed·control/grabber 추가, control/track을 흰색 24%/검정 12%로 재정의. Create·Create · More Options·Light Check · Create·Now Playing·Phone Number 템플릿의 인스턴스 전환.
- 1.2.0 (2026-10-29): 변형별 기본값 분리(Text Field Placeholder/Text, Progress Banner Loading/Ready Message, Empty State Error Title/Description + x-circle, Share Item 라벨 고정, Segmented Control 배지 SAVE 20%). 반투명 레이어 토큰 bg/layer-1·2·3 · border/layer · bg/brand-tint, 브랜드 테두리 그라데이션 gradient/brand-stroke-start·mid·end, grain/* · glow/* 토큰, 프리미티브 brand/violet · alpha 6종. Grain/Brand Gradient 듀오톤 재조정, Grain/Background 재조정, Glow/Brand Inner 신설. 브랜드 버튼·FAB Create 채움·그레인을 자식 Gradient Surface로 분리(채움 6겹, 라벨에 그레인 없음). Button Style=Brand Outline 6종, Button Small Brand Outline·Model Pill 그라데이션 테두리 + 웜 틴트 + 안쪽 글로우. List Row / Option · Card / Layer Group, 템플릿 Create · More Options, Create 카드 bg/layer-1 전환. 문서: Layout & Effects 레시피 섹션, Color 토큰 표·Cover 갱신, tokens.json recipes.
- 1.1.0 (2026-10-29): 대비 보정(neutral/350·450 추가, fg/tertiary·fg/secondary·icon/inactive 재정의), Brand 버튼·FAB 레이블 스크림, gradient/warm-glow, delay 토큰, 09 Z-Index, Text Field·Code Input·Page Control·Empty State·Tooltip 추가, Avatar/Thumbnail Placeholder 타입, 버튼·칩 Min/Max와 1줄 말줄임, 리사이징·Dynamic Type·iPhone 폭별 규칙 표, Light 모드 검증.
- 1.0.0 (2026-10-29): 최초 구축.
