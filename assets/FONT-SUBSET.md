# BlackHanSans-subset.woff2 재현 방법

1. 원본 TTF 다운로드 (OFL, Google Fonts 저장소):
   `curl -L -o BlackHanSans-Regular.ttf https://raw.githubusercontent.com/google/fonts/main/ofl/blackhansans/BlackHanSans-Regular.ttf`
2. 서브셋 대상 코드포인트: **KS X 1001 한글 2,350자**(EUC-KR 0xB0A1–0xC8FE 디코딩) + **printable ASCII 0x20–0x7E** + 라틴 구두점·화살표(`· × — … → ← ↓` 등). 이 목록을 `U+XXXX` 콤마 구분으로 `unicodes.txt`에 저장.
3. `pip install fonttools brotli` 후 서브셋:
   `python3 -m fontTools.subset BlackHanSans-Regular.ttf --unicodes-file=unicodes.txt --output-file=BlackHanSans-subset.woff2 --flavor=woff2 --layout-features='' --no-hinting --desubroutinize --drop-tables+=DSIG`
4. 결과: 글리프 2,453개 / cmap 2,452개 / **78.4 KB**. 요청한 2,484자 중 32자(`→ ← ↓ · — … ° ± ※` 등)는 **원본 Black Han Sans에 애초에 없어** 제외됨 — 해당 문자는 `--font-mono` / `--font-body` 폴백으로 렌더된다.
