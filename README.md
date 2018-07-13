# develop branch
>클라우드캐시 개발용 브랜치입니다. 모든 작업은 이 브랜치에 병합합니다.  
>master 브랜치는 서비스의 각 배포 단계에만 fast-forward로 최신화하고 버전명을 tag로 붙여 관리합니다.

## 1. 코드리뷰 workflow

1. CloudCash 저장소(원본저장소) 생성

2. Develop 브랜치 생성 (깃헙에서)

3. Develop 브랜치를 각자의 저장소로 Fork (깃헙에서)

4. 각자의 원격저장소(개인 github저장소)에서 각자의 local저장소로 develop 브랜치만 Clone
   - `$git clone -b develop - -single-branch branchURL` (반드시 본인 원격저장소URL ! )

5. 모든 브랜치 확인
   - `$git branch -a`

6. (최초 1회만)CloudCash 저장소의 최신화된 develop브랜치와 local의 develop브랜치를 동기화해주기 위해 원격저장소 설정
   - `$git remote add CCproject(별명) CloudCash저장소URL` -> 본인저장소URL로 하지 말것.
   - `$git remote -v` 로 원격저장소 확인

7. Local에서 develop 브랜치는 master브랜치라 생각하고 작업용 브랜치 생성
   - `$git checkout -b ryanwork1`

------------------------------------------------------------
8. 작업
------------------------------------------------------------

9. 변경된 내용 확인 `$git status`

10. 변경된 내용 add, commit, push
   - `$git add .`
   - `$git commit -m '커밋내용'`
   - `$git push origin ryanwork1`

11. CloudCash 저장소로 Pull Request (깃헙에서)

12. 관리자는 코드 리뷰 후 develop 브랜치로 Merge (깃헙에서)

------------------------------------------------------------
13. — 다른 팀원이 작업하여 원본저장소에 커밋 추가했을시 — 
------------------------------------------------------------

14. 설정된 원본저장소(CloudCash저장소)의 develop 브랜치로 부터 local의 develop브랜치 동기화. (우선 HEAD가 develop으로 가도록하고!)
    - `$git checkout develop`
    - `$git fetch CCproject(별명) develop`
    - `$git branch -a` 로 브랜치 확인
    - `$git merge CCproject/develop`
    - 또는 merge 대신 `$git rebase CCproject/develop`
    - 또는 Pull로 한번에 동기화 `$git pull CCproject develop`

15. Merge 된 이후에는 ryanwork1 브랜치 삭제
    - `$git branch -d ryanwork1`

16. 이후 작업
    - CloudCash저장소(원본저장소)에서 local저장소로 Pull (동기화)
    - 작업용 브랜치를 만드는 7번부터 반복한다.
  
    
      
        
          
          
## 2. 기타 참고사항 (수정중)

- `$git log —branches —graph —decorate —oneline` 명령으로 gui비슷하게 볼 수 있다.

- `$git config --global core.pager cat` 명령으로 log와 diff를 cat으로 볼 수 있다.

- 이전 버전으로 되돌아가는 방법 reset과 revert 차이는 여기서 확인
   - https://www.popit.kr/%EA%B0%9C%EB%B0%9C%EB%B0%94%EB%B3%B4%EB%93%A4-git-back-to-the-future/

- Git stash 란?
- 다른 브랜치로 checkout을 해야하는데 아직 현재 브랜치에서 작업이 끝나지 않은 경우에 커밋을 하기 애매하다. 이런 경우 Stash를 이용하면 작업중이던 파일을 임시로 저장해두고 현재 브랜치의 상태를 마지막 커밋의 상태로 초기화 할 수 있다. 그 후에 다른 브랜치로 이동하고 작업을 끝낸 후에 작업중이던 브랜치로 복귀한 후에 이전에 작업하던 내용을 복원할 수 있다. 즉, stash를 사용하여 작업중인 파일을 숨겨둘 수 있다.
   - `$git stash`
   - `$git stash list`
   - `$git stash apply` — 최근의 stash 내용으로 다시 복원
   - `$git stash drop` — 최근 stash 삭제
   - `$git stash pop` — stash 복원 및 삭제

- 모든 commit에서 특정 파일 제거하기
   - `$git filter-branch - -tree-filter 'rm -f passwords.txt' HEAD`

- 이미 공개저장소에 push 한 커밋을 rebase하면 절대 안된다.
   - Push 하기 전에 정리하기 위해 rebase를 하는 것은 괜찮다.
   - 절대 공개하지 않고 혼자 rebase하는 경우도 괜찮다.
   - 하지만 이미 공개된 커밋을 rebase하면 망한다.






## 3. git 고급 기능 실습  
>https://learngitbranching.js.org/

### 원격 저장소 추적하기  
- `$git checkout -b foo origin/master` - 로컬브랜치 foo를 생성하고 원격브랜치 origin/master를 추적하게 설정  
- `$git branch -u origin/master foo` - foo 브랜치가 origin/master를 추적하도록 설정  
- `$git branch -u origin/master` - foo가 현재 작업중인 브랜치라면 생략가능  

### git push의 인자들
- `$git push <remote> <place>` - place 인자는 git에게 '어디서부터 오는 커밋인지', '어디로 가는 커밋인지', 두 저장소 간 동기화 작업을 할 장소지정. <remote>와 <place>를 모두 지정했기 때문에 git은 현재 체크아웃한 브랜치는 무시하고 명령 수행  
- `$git push origin master` - 내 저장소의 master 브랜치에서 모든 커밋을 수집하고, origin의 master 브랜치로 가서 부족한 커밋을 채워넣는다.
- `$git push origin <source>:<destination>` - 커밋의 근원이 되는 source와 목적지가 되는 destination을 다르게 하고싶을 때
- `$git push origin foo^:master` - foo의 부모위치에서 원격으로 커밋을 업로드하고 destination인 master 브랜치를 갱신
- `$git push origin master:newBranch` - push할 destination이 없는 경우 newBranch처럼 새 브랜치명 적으면 만들어줌

### git fetch의 인자들
- `$git fetch origin foo` - 원격저장소의 foo브랜치에서 현재 로컬에 없는 커밋들을 가져와 로컬의 'origin/foo' 브랜치 아래에 추가. 즉, 커밋들을 foo 브랜치에서만 내려받은 후 로컬의 origin/foo 브랜치에만 적용
- `$git fetch origin foo~1:bar` - foo~1을 origin의 place로 지정하고 커밋들을 내려받아 bar(로컬브랜치)에 추가. bar가 없는 경우 알아서 만들어줌
- `$git fetch`- 인자 없이 fetch를 하면 원격저장소에서 모든 원격브랜치들로 커밋들을 내려받는다.

### source 인자 없으면
- `$git push origin :foo` - 원격저장소의 foo브랜치 삭제 (foo에 null을 push한 개념)
- `$git fetch origin :bar` - 로컬저장소에 bar 브랜치를 만든다. (기괴하다..)

### git pull의 인자들
- `$git pull origin master:foo` - 로컬에 foo브랜치를 만들고, 원격저장소의 master로부터 로컬의 foo브랜치에 커밋들을 받는다. 그리고 foo브랜치를 현재 체크아웃된 bar브랜치에 병합한다.



# 4. Reference

>Pull Request를 이용한 개발 흐름의 적용 (후기)  
https://blog.outsider.ne.kr/1199

>Pro Git - 전반적인 git 학습  
https://git-scm.com/book/ko/v2

>git 실습  
https://learngitbranching.js.org/

>누구나 쉽게 이해할 수 있는 Git 입문  
https://backlog.com/git-tutorial/kr/

>git 초보를 위한 pull request 방법  
https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/

>Git & GitHub Crash Course For Beginners  
https://www.youtube.com/watch?v=SWYqp7iY_Tc

>터미널 꾸미기  
https://beomi.github.io/2017/07/07/Beautify-ZSH/

>카카오의 코드리뷰  
http://tech.kakao.com/2016/02/04/code-review/

>네이버의 Gerrit을 이용한 코드리뷰  
https://d2.naver.com/helloworld/6033708

>코드리뷰 가이드라인  
http://flyburi.com/m/576
