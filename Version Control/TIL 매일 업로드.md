# TIL 업로드 자동화
> 기록을 수치화 하고 싶다! 매일 코드를 커밋하지 않더라도 개발을 하고 있음을 기록하려고 TIL 노트를 매일 깃허브에 업로드 하려고 한다.

## Git Commit&Push 자동화 
### sh

```sh
Y=$(date +%Y)
M=$(date +%m)
D=$(date +%d)
Ymd=$Y-$M-$D
```
- 오늘 날짜를 YYYY-mm-dd 형식으로 저장한다.
```
cd ~/Library/Mobile\ Documents/iCloud\~md\~obsidian/Documents/HelloWorld/obsidian-git-sync 
```
- 내 경우에는 이 명령을 안하면 sh가 .git과 같은 위치에 있더라도 `fatal: not a git repository (or any of the parent directories): .git` 오류가 떴다.
- `\`로 폴더 이름에 있는 공백과 ~를 문자로 인식할 수 있게 했다. 안그럼 경로를 못찾는다.
```
eval `ssh-agent -s` && ssh-add ~/.ssh/id_rsa && ssh-add -l
```
- 리모트 연결하고 처음 한 번만 실행 해주면 된다. ssh-keygen과 등록은 이미 해둔 상태였다. RSA 인증키를 만들어 암호를 입력하지 않고 해당 컴퓨터에서 아이디/패스워드 없이 자유롭게 리모트 저장소에 접근이 가능하게 한다.
```
echo $(date '+%Y-%m-%d %H:%M:%S')
```
- 로그 파일을 읽을 때 편하도록 날짜와 실행 시간을 출력한다.
```
git status
git add .
git commit -m "TIL: ${Ymd} 🌱"
git push origin main
```
- 파일 상태를 확인하고 스테이지에 올리고 변경 사항을 커밋한 후 원격 저장소에 바로 푸시한다.
### crontab
```cron
crontab -e
```
- 크론탭의 내용을 생성 및 수정
```cron
# .---------------- 분 (0 - 59)
# |  .------------- 시간 (0 - 23)
# |  |  .---------- 일 (1 - 31)
# |  |  |  .------- 월 (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- 요일 (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |                  
# *  *  *  *  * user-name command to be executed
[48 23 * * * {sh 경로} ~/{실행할 파일 경로} >> ~/{로그 수집 파일 경로} 2>&1](<48 23 * * * /bin/sh ~/Library/Mobile\ Documents/iCloud\~md\~obsidian/Documents/HelloWorld/obsidian-git-sync/git-schedule-til.sh %3E> ~/Library/Mobile\ Documents/iCloud\~md\~obsidian/Documents/HelloWorld/obsidian-git-sync/til.sh.log 2>&1>)
```
- 매일 오후 11시 48분마다 위의 sh 파일이 실행되고 그 표준 로그를 수집한다.
- 꼭 한 줄에 적어야 한다.
###### 참고
- [크론탭을 이용한 Git PUSH 자동화](https://ssafy-story.tistory.com/37)
- [Unable to run git commands with crontab](https://stackoverflow.com/questions/55966634/unable-to-run-git-commands-with-crontab)
- [Mac OS 에서 crontab 사용 - 크롤러 크론탭으로 돌려보기](https://f-dever-error-log.tistory.com/29)
- [크론탭 스케줄링사용하여 github auto commit push 자동 잔디밭 만들기](https://shlee0882.tistory.com/270)
- [[MAC] Crontab(크론탭)으로 파이썬을 특정 주기 자동 실행시키기](https://23log.tistory.com/174)
- [맥북 작업 스케줄러 (feat. Crontab사용방법, Cron과 Crontab 차이, 파이썬 파일 자동실행)](https://june98.tistory.com/101)
- [Crontab 로그(log) 남기는 방법 (feat. 출력 리다이렉션, 2>&1의 의미)](https://june98.tistory.com/102)