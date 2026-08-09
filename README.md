# bbj-h — PLAN-G 레이아웃 프로토타입

홈페이지 개편을 위한 **레이아웃 실험 저장소**. 운영 사이트가 아니다.

- 운영: [5hk/bbj](https://github.com/5hk/bbj) → https://plan-g.io
- 문서·플랜: [5hk/bbj-docs](https://github.com/5hk/bbj-docs)

## 규칙

- ⛔ **`CNAME` 파일을 두지 않는다.** 이 저장소가 plan-g.io 도메인을 가로채면 운영 사이트가 죽는다.
- 모든 프로토타입 HTML에 `<meta name="robots" content="noindex, nofollow">` 를 넣는다.
- 레이아웃 안(案)은 `typeN.html` 로 하나씩 추가한다. 각 파일은 자체 완결(인라인 CSS/JS)이라 서로 영향을 주지 않는다.

## 파일

| 파일 | 내용 |
|---|---|
| `type1.html` | 안 1 — **브루탈리스트 컬러**. 채도 높은 컬러 블록, 초대형 컨덴스드 대문자 (참조: OFEN) |
| `index.html` | 안 2 — **흑백 편집형 + 영상 히어로**. 워드마크가 영상 경계를 가로지름, 텍스트 애니메이션 (참조: Peter Lindbergh) |

`index.html` 이 현재 진행 중인 안이다. `type1.html` 은 초기 비교용으로 남겨둔 것으로,
아래 정보구조 변경은 반영돼 있지 않다.

현재 정보구조 (index.html):
**00 Opening · 01 Now · 02 Artists · 03 Releases · 04 Notice · 05 Press · 06 Contact**

- Roster → **Artists**, Catalog → **Releases**, Stage → **Notice**, Inquiry → **Contact** 로 개명
- **Studio 삭제** — 서비스 라인을 채울 실제 내용이 없었다. 빈 섹션은 신뢰를 깎는다
- Notice 는 공연 소식을 한 건당 한 줄로, 3건씩 페이징

### index.html 조절 포인트

```css
--stage-h:  74vh;   /* 히어로 영상 높이 */
--straddle: 50%;    /* 워드마크가 영상 쪽으로 걸치는 비율. 50% = 영상/지면 반반 */
```

### 히어로 영상 준비

`media/` 는 기본적으로 .gitignore 대상이고, 배포에 필요한 것만 예외로 커밋한다
(`hero.mp4` · `hero-poster.jpg` · `flowerburn.mp4` · `flowerburn-poster.jpg`).

원본에서 만드는 절차 (ffmpeg 필요 — `brew install ffmpeg`):

```bash
ffmpeg -ss 8 -t 12 -i 원본.MP4 -an -c:v libx264 -crf 30 -preset slow -pix_fmt yuv420p -movflags +faststart media/hero.mp4
```

```bash
ffmpeg -ss 1 -i media/hero.mp4 -frames:v 1 -q:v 3 media/hero-poster.jpg
```

- `-ss 8 -t 12` — 8초 지점부터 12초. 가장 좋은 구간으로 바꿔서 쓴다
- `-an` — **오디오 제거.** 히어로는 항상 muted라 소리는 절대 안 들리는데 용량만 차지한다.
  실제로 오디오가 섞여 들어간 적이 있고, 빼면서 화질을 건드리지 않고 용량이 줄었다
- `-crf 30` — 어두운 조명 클립에서는 25와 육안 차이가 없고 용량은 절반이다
- `-movflags +faststart` — 메타데이터를 앞으로 보내 다운로드 도중 재생 시작
- `muted` `loop` `playsinline` 은 모바일 자동재생 조건이라 지우면 안 된다

**목표치**: 8~15초 · **2MB 이하**. 히어로가 무거우면 첫인상이 검은 화면이 된다.

⚠ `index.html` 은 `src` 대신 `data-src` 로 영상을 들고 있다가 첫 화면 자원이
다 내려온 뒤에 붙인다. 영상과 이미지가 대역폭을 다투면 둘 다 늦어지기 때문이다.
영상을 교체할 때 `src=` 로 되돌리지 말 것.

### 이미지 규칙

- **표시 폭 × 2(레티나)** 를 넘는 파일을 두지 않는다. 넘으면 느린 회선에서 그대로 손해다
- `loading="lazy" decoding="async"` 를 붙이고, `width`/`height` 로 자리를 미리 잡는다
- 사진은 컨테이너의 reveal 이 아니라 **자기 자신의 `load`** 에 맞춰 나타난다
  (그렇지 않으면 느린 회선에서 빈 상자가 먼저 떠올라 깜빡이는 것처럼 보인다)
- 쓰지 않게 된 이미지는 남기지 않는다 — 페이지는 요청하지 않아도 배포에는 실린다

## 배포 (GitHub Pages)

**https://5hk.github.io/bbj-h/**

`main` 에 push하면 GitHub Actions(`.github/workflows/pages.yml`)가 저장소를 그대로 Pages에 올린다.
빌드 과정은 없다. 배포 상태는 저장소의 **Actions** 탭에서 확인한다.

```bash
git push        # 이게 곧 배포
```

- 저장소 **Settings → Pages → Source** 는 `GitHub Actions` 로 설정돼 있어야 한다
- ⛔ **`CNAME` 파일을 만들지 않는다.** 만들면 plan-g.io 를 이 프로토타입이 가로채 운영 사이트가 죽는다
- 모든 안(案)에 `noindex` 가 걸려 있어 검색에는 잡히지 않는다

### 히어로 영상이 배포본에서 안 보인다면

`media/` 는 기본적으로 gitignore 대상이라, **영상이 저장소에 없으면 배포된 페이지에는 플레이스홀더만 보인다.**
아래 두 파일만 예외로 커밋하도록 설정돼 있으니, **압축한 뒤** 커밋한다.

```bash
git add -f media/hero.mp4 media/hero-poster.jpg
```

⚠️ 압축 전 원본(수 MB~수십 MB)을 그대로 커밋하지 않는다. git 저장소는 한 번 커밋한 큰 파일을
되돌리기 어렵다. 4MB 이하로 줄인 뒤 올린다.

## 로컬 미리보기

```bash
node server.mjs
```

http://localhost:4321/type1.html

## 플레이스홀더 사용법

이미지 영역은 전부 라벨이 붙은 프레임이다.

```html
<div class="ph" data-label="프로필 3:4" style="--ar:3/4"></div>
```

실제 에셋이 나오면 해당 프레임만 `<img>` 로 교체하면 되고, 주변 레이아웃은 손대지 않는다.
어두운 배경 위에서는 `ph--dark`, 부모 높이를 꽉 채우려면 `ph--fill` 을 함께 쓴다.

## 색상 교체

브랜드 컬러가 확정되면 `:root` 의 액센트 4개만 바꾸면 전체가 따라온다.

```css
--accent:  #ff4a17;   /* 주 액센트 */
--accent2: #c6f24e;   /* 보조 */
--accent3: #ffc22b;   /* 3차 */
--deep:    #17402c;   /* 딥톤 */
```
