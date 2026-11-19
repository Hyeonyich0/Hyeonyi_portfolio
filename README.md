# 조현이 포트폴리오 사이트

서버 설정이 필요 없는 정적 사이트예요. `index.html`과 `assets/` 폴더, 파비콘 파일만 있으면 돼요.

## 폴더 구성

```
index.html            페이지 전체 (디자인, 동작, 프로젝트 설정)
favicon.ico 외        파비콘 (로고 PNG로 만들었어요)
assets/mark.png       표지 로고 (색은 코드에서 바뀌어요)
assets/img/           프로젝트 이미지
assets/video/         영상을 넣는 폴더 (지금은 비어 있어요)
```

이미지 파일 번호는 상세 화면의 갤러리 순서와 같아요. 예: `kitsch-1.jpg`는 KITSCH의 첫 번째 이미지예요.

## 내 컴퓨터에서 미리보기

폴더 안에서 터미널을 열고 실행해요.

```
python3 -m http.server 8000
```

브라우저에서 `http://localhost:8000`을 열면 돼요. `index.html`을 더블클릭해도 대부분 열리지만, 영상은 위 방법으로 확인하는 게 안전해요.

## 배포 (셋 중 하나)

**Netlify (가장 쉬움)**: app.netlify.com에서 "Add new site → Deploy manually"를 고르고 이 폴더를 통째로 끌어다 놓으면 끝이에요.

**Vercel**: 폴더를 GitHub 저장소로 올린 뒤 Vercel에서 그 저장소를 Import해요. 설정은 기본값 그대로 두면 돼요.

**GitHub Pages**: 저장소에 올린 뒤 Settings → Pages에서 브랜치를 선택해요. `.nojekyll` 파일이 이미 들어 있어요.

내 도메인을 쓰려면 각 서비스의 Domains 메뉴에서 도메인을 추가하고, 안내해 주는 DNS 값을 도메인 업체에 입력하면 돼요.

주소의 `#works`, `#about`, `#work-kitsch` 같은 부분은 화면 전환용이라서 서버 설정 없이 그대로 동작해요.

## 영상 넣기

`index.html`에서 `PROJECT CONFIG` 주석을 찾으세요. 프로젝트마다 두 가지를 쓸 수 있어요.

1. **갤러리에 영상 추가**: `imgs` 목록에 아래처럼 넣어요. `poster`는 영상 시작 전에 보일 이미지 키예요.

   ```
   imgs:["q2","q3","q4",{video:"assets/video/kitsch.mp4",poster:"q4"}]
   ```

   영상 썸네일에는 ▶ 표시가 붙고, 큰 화면 모서리의 ⤢ 버튼을 누르면 전체 화면 뷰어에서 재생돼요.

2. **카드 미리보기**: Works 화면에서 카드에 마우스를 올리면 소리 없는 짧은 영상이 반복 재생돼요.

   ```
   preview:"assets/video/kitsch-preview.mp4"
   ```

두 기능은 따로 써도 되고 함께 써도 돼요. 영상 파일은 `assets/video/`에 넣으면 돼요.

### YouTube / Google Drive 링크로 넣기

지금 KITSCH와 워크쏠에는 링크 영상이 첫 번째 항목으로 들어 있어요. 내 도메인에서는 페이지 안에서 바로 재생돼요.

```
{link:"https://youtu.be/영상ID", embed:"https://www.youtube.com/embed/영상ID", poster:"t_kitsch"}
```

- YouTube: `embed`는 `https://www.youtube.com/embed/영상ID` 형식이에요. 영상ID는 링크의 `youtu.be/` 뒤 글자예요.
- Google Drive: `embed`는 `https://drive.google.com/file/d/파일ID/preview` 형식이에요. 파일의 공유 설정이 "링크가 있는 모든 사용자"여야 다른 사람도 볼 수 있어요. 올린 직후에는 Drive가 영상을 처리하는 동안 재생이 안 될 수 있어요.
- Figma 프로토타입: `embed`는 `https://www.figma.com/embed?embed_host=share&url=` 뒤에 프로토타입 링크를 URL 인코딩해서 붙인 값이에요(Moudi에 들어 있어요). 파일의 공유 설정이 "링크가 있는 모든 사용자 보기"여야 해요. 버튼 문구는 `label:"▶ 프로토타입 열기"`처럼 바꿀 수 있어요.
- `embed`를 빼면 썸네일과 "새 창에서 영상 보기" 버튼만 보이고, 누르면 `link`가 새 창으로 열려요.

### 영상 줄이기 (ffmpeg)

본 영상 (1080p, 웹 재생용):

```
ffmpeg -i 원본.mov -vf scale=-2:1080 -c:v libx264 -crf 23 -preset slow -movflags +faststart -c:a aac -b:a 128k kitsch.mp4
```

카드 미리보기 (8초, 소리 없음, 540p):

```
ffmpeg -i 원본.mov -t 8 -an -vf scale=-2:540 -c:v libx264 -crf 28 -preset slow -movflags +faststart kitsch-preview.mp4
```

`-movflags +faststart`가 있어야 파일을 다 받기 전에 재생이 시작돼요.

### 용량 참고

- 배포 서비스마다 파일 크기, 전체 용량, 월 데이터 전송량 제한이 달라요. 무료 플랜 기준은 바뀔 수 있으니 올리기 전에 각 서비스 안내를 확인하세요. GitHub 저장소는 파일 하나가 100MB를 넘으면 올릴 수 없어요.
- 몇 분짜리 풀 영상이 여러 개라면 YouTube나 Vimeo에 올리고 링크만 연결하는 편이 안정적이에요.

## 파비콘

로고를 핑크로 칠한 투명 배경 PNG와 ICO예요. 라이트 모드와 다크 모드 탭 모두에서 보여요. `apple-touch-icon.png`는 아이폰 홈 화면용이라 흰 배경이에요. 색이나 여백을 바꾸고 싶으면 알려주세요.

## 폰트

Bricolage Grotesque는 Google Fonts에서 불러와요. 인터넷이 없는 환경에서는 시스템 폰트로 대체되어 표시돼요.
