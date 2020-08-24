# Git Rebase


브랜치를 나누어 작업하던 내용을 합칠때 일반적으로 `merge` 명령어를 사용한다. 하지만 소스 컨트리뷰터가 pull request 하는 경우에는 `rebase` 사용을 권장한다. 
또한 팀 환경에서 기능 개발 후 마스터 `merge` 전에 `rebase`를 사용하면 히스토리 관리가 깔끔해진다.
 
![new feature](https://tech.10000lab.xyz/resources/images/git_rebase_new_branch.jpg)

위 그림은 일반적으로 팀 개발을 하게되면 마주하게 되는 상황이다. 브랜치를 생성해서 개발을 하고 커밋을 한뒤 보면 다른 작업자가 개발을 완료해 마스터에 다른 커밋들이 올라와있게 된다.
일반적으로는 머지를 하게되지만, 마스터의 변경된 사항들을 적용하여 지금 브랜치에서 개발이 더 필요한 경우에는 리베이스가 적합하다. 
리베이스는 말그대로 위 그림의 `ddf19c2` 지점의 new_feature 브랜치의 베이스를 옮기는 것이다. 
그냥 옮기는 것 뿐만 아니라 현재 브랜치의 커밋 사항 중 일부는 합칠 것인지, 남길것인지, 추가로 수정할 것인지 지정할 수 도 있다.

![rebased](https://tech.10000lab.xyz/resources/images/git_rebase_rebased.jpg)

- 현재 master 브랜치라면 : `git rebase master new_feature`
- 현재 new_feature 브랜치라면 : `git rebase master`
 
  
리베이스를 해서 베이스를 `ddf19c2` 에서 마스터의 최상단 커밋으로 재배치했다. 리베이스를 하는 과정에서 개발중이던 브랜치와 같은 곳을 수정하게 되면 충돌이 발생하게 된다. (`git rebase --abort`로 리베이스 이전 상태로 되돌릴 수 있음)
merge 와 마찬가지로 git status로 충돌 지점을 확인하고 해결한 후 스테이지 영역에 올리고 `git rebase --continue` 명령어를 사용한다. (수정된 파일이 이미 커밋된 내용과 동일한 경우 `git rebase --skip`)
리베이스는 베이스 지점을 옮길 뿐 작업이 모두 완료되면 `git merge [target branch]`로 마스터에 머지를 해서 작업을 마무리해야 한다.


---
### 주의 사항


리베이스로 작업을 하면 중앙 서버로 푸시를 할 때 origin 서버에 있는 내용과 현재 로컬에 있는 내용의 머지 베이스가 달라서 에러가 발생하게 된다. 이 경우 `git push origin new_feature --force-with-lease`
혹은 `git push -f`로 강제 푸시해서 해결한다. (`--force-with-lease`를 사용하는 경우 혹시라도 리모트 서버와 다른 변경사항이 있는지 알려주기 때문에 내용을 덮어쓰지 않고 변경사항만 등록이 된다)



---
[git rebase](https://tech.10000lab.xyz/git/git-rebase-workflow.html)
