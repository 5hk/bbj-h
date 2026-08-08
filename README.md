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
| `type2.html` | 안 2 — **흑백 편집형 + 영상 히어로**. 워드마크가 영상 경계를 가로지름, 텍스트 애니메이션 (참조: Peter Lindbergh) |

두 안 모두 정보구조는 동일하다 (00 Opening · 01 Now · 02 Roster · 03 Catalog · 04 Stage · 05 Press · 06 Studio · 07 Inquiry).
시각 방향만 다르므로 나란히 비교할 수 있다.

### type2 조절 포인트

```css
--stage-h:  74vh;   /* 히어로 영상 높이 */
--straddle: 50%;    /* 워드마크가 영상 쪽으로 걸치는 비율. 50% = 영상/지면 반반 */
```

영상이 준비되면 `<video>` 의 `src` 를 채우고 `hidden` 을 지운 뒤, 옆의 `.ph` 플레이스홀더를 삭제한다.
`muted` `loop` `playsinline` 은 모바일 자동재생에 필수라 지우면 안 된다.

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
