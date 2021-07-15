## git (local repo)

---

| git _command_ | meaning                  |
| ------------- | ------------------------ |
| init          | git의 시작               |
| status        | git 상태 확인            |
| add           | 파일 수정 이력 기록 준비 |
| commit        | 파일 변경사항 기록       |
| reset         | add, commit, push 취소   |

## github (remote repo)

---

git remote add origin (url)  
git push origin master

- git remote -v  
  github와 연결되어 있는지 확인하는 방법

## git, github으로 협업하기

---

1. 깃헙 저장소 복제: `git clone _저장소 url_**`
2. 작업 브랜치 생성 후 이동

   - 브랜치 생성: `git branch _브랜치명_`
   - 브랜치 이동: `git checkout _브랜치명_`
   - 새로운 브랜치를 생성함과 동시에 생성된 브랜치로 이동  
     `git checkout -b _브랜치명_ `

3. 작업, 커밋
4. 브랜치 최신화
   - 다른 팀원이 변경사항을 pull하고 remote repo main branch에서 merge 가 되었을 때, 나의 변경사항을 main branch에 pull 하기 전에 local repo main branch에서 remote repo의 변경사항을 merge하여 remote repo와 local repo의 싱크를 먼저 맞춰주어야 한다. 그래야 PR 남긴 변경사항을 메인 브랜치에 병합할 때 오류없이 병합할 수 있다. (오류가 나면 병합이 되지 않는다.)
   ```
   git checkout master
   git pull origin master
   git checkout _branch_
   git merge master
   // -> conflict 해결 후 commit
   ```
5. 원격 저장소에 올리기: `git push origin _브랜치명_`
6. PR 남기기

## 브랜치 삭제

---

- 하나의 브랜치 삭제

```
git branch -d <branch name>

// 브랜치 강제 삭제
git branch -D <branch name>
```

- 모든 브랜치 삭제

```
git branch | grep -v '^*' | xargs git branch -d
```
