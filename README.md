# develop branch
>클라우드캐시 개발용 브랜치입니다. 모든 작업은 이 브랜치에 병합합니다.  
>master 브랜치는 서비스의 각 배포 단계에만 fast-forward로 최신화하고 버전명을 tag로 붙여 관리합니다.

## 코드리뷰 workflow

1. CloudCash 저장소(원본저장소) 생성

2. Develop 브랜치 생성 (깃헙에서)

3. Develop 브랜치를 각자의 저장소로 Fork (깃헙에서)

4. 각자의 원격저장소(개인 github저장소)에서 각자의 local저장소로 develop 브랜치만 Clone
   - $git clone -b develop - -single-branch branchURL (반드시 본인 원격저장소URL ! )

5. 모든 브랜치 확인
   - $git branch -a

6. (최초 1회만)CloudCash 저장소의 최신화된 develop브랜치와 local의 develop브랜치를 동기화해주기 위해 원격저장소 설정
   - $git remote add CCproject(별명) CloudCash저장소URL -> 본인저장소URL로 하지 말것.
   - $git remote -v 로 원격저장소 확인

7. Local에서 develop 브랜치는 master브랜치라 생각하고 작업용 브랜치 생성
   - $git checkout -b ryanwork1

------------------------------------------------------------
8. 작업
------------------------------------------------------------

9. 변경된 내용 확인 $git status

10. 변경된 내용 add, commit, push
   - $git add .
   - $git commit -m '커밋내용'
   - $git push origin ryanwork1

11. CloudCash 저장소로 Pull Request (깃헙에서)

12. 관리자는 코드 리뷰 후 develop 브랜치로 Merge (깃헙에서)

------------------------------------------------------------
13. — 다른 팀원이 작업하여 원본저장소에 커밋 추가했을시 — 
------------------------------------------------------------

14. 설정된 원본저장소(CloudCash저장소)의 develop 브랜치로 부터 local의 develop브랜치 동기화. (우선 HEAD가 develop으로 가도록하고!)
    - $git checkout develop
    - $git fetch CCproject(별명) develop
    - $git branch -a 로 브랜치 확인
    - $git merge CCproject/develop
    - 또는 merge 대신 $git rebase CCproject/develop
    - 또는 Pull로 한번에 동기화 $git pull CCproject develop

15. Merge 된 이후에는 ryanwork1 브랜치 삭제
    - $git branch -d ryanwork1

16. 이후 작업
    - CloudCash저장소(원본저장소)에서 local저장소로 Pull (동기화)
    - 작업용 브랜치를 만드는 7번부터 반복한다.
  
    
      
        
          
          
## 기타 참고사항 (수정중)
- $git log —branches —graph —decorate —oneline 명령으로 gui비슷하게 볼 수 있다.

- 이전 버전으로 되돌아가는 방법 reset과 revert 차이는 여기서 확인
   - https://www.popit.kr/%EA%B0%9C%EB%B0%9C%EB%B0%94%EB%B3%B4%EB%93%A4-git-back-to-the-future/

- Git stash 란?
- 다른 브랜치로 checkout을 해야하는데 아직 현재 브랜치에서 작업이 끝나지 않은 경우에 커밋을 하기 애매하다.
이런 경우 Stash를 이용하면 작업중이던 파일을 임시로 저장해두고 현재 브랜치의 상태를 마지막 커밋의 상태로 초기화 할 수 있다.
그 후에 다른 브랜치로 이동하고 작업을 끝낸 후에 작업중이던 브랜치로 복귀한 후에 이전에 작업하던 내용을 복원할 수 있다.
즉, stash를 사용하여 작업중인 파일을 숨겨둘 수 있다.
   - $git stash
   - $git stash list
   - $git stash apply — 최근의 stash 내용으로 다시 복원
   - $git stash drop — 최근 stash 삭제
   - $git stash pop — stash 복원 및 삭제

- 모든 commit에서 특정 파일 제거하기
   - $git filter-branch - -tree-filter 'rm -f passwords.txt' HEAD

- 이미 공개저장소에 push 한 커밋을 rebase하면 절대 안된다.
   - Push 하기 전에 정리하기 위해 rebase를 하는 것은 괜찮다.
   - 절대 공개하지 않고 혼자 rebase하는 경우도 괜찮다.
   - 하지만 이미 공개된 커밋을 rebase하면 망한다.
