# TIL
: Today I Learned
>옵시디언(마크다운 에디터)을 이용해 작성하고 있습니다.
### 📍 규칙
1. 매일 컴퓨터를 열고 오늘의 TIL을 템플릿으로 생성한다.
2. TIL 항목마다 해당되는 것들을 시간날 때 바로 정리한다.
	- 글이 너무 길어지면 새로운 노트를 만들어 템플릿 형식에 맞추어 작성한다.
	- 새 노트는 TIL에 [[]]로 연결한다.
3. 매일 11시 48분에 change가 있는 파일들을  github action으로 github에 업데이트 한다.
### ⛓ Github 자동 업로드 방법 (m1, macOS Ventura, zsh)
#### sh

```sh
Y=$(date +%Y)
M=$(date +%m)
D=$(date +%d)

Ymd=$Y-$M-$D

cd ~/{깃 폴더의 절대 경로}
git add .
git commit -m "TIL: ${Ymd} 🌱"
git push origin main
```
#### crontab 

```cron
crontab -e # 크론탭 설정

48 23 * * * {sh 경로} ~/{실행할 파일 경로} >> ~/{로그 수집 파일 경로} 2>&1
```

