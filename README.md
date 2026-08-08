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
| `type1.html` | 안 1 — 브루탈리스트 편집형. 정보구조를 새로 설계 (Now/Roster/Catalog/Stage/Press/Studio/Inquiry) |

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
