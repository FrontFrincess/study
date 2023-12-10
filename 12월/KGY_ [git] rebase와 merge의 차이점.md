오늘 처음으로 회사 레포에 완성된 마크업 페이지를 디벨롭에 적용하는 과정에서...

예상치 못한 오류가 발생했다! >>> ⚠️git conflict

그러던 중 옆에 사수 분께서 rebase라는 새로운 방식을 언급해주셔서,,, (하지만 나는 처음 보는 명령어임)

문득 merge와 rebase의 차이점에 대해 궁금하여 해당 포스팅을 준비해보았습니다!

## **1\. git merge(병합)**

merge는 **두 개의 다른 브랜치를 하나로 합치는 과정**을 의미합니다.

```
git checkout main		// main에서 병합 진행
git merge feature-branch	// 현재 작업중인 브랜치를 main에 병합
```

git merge는 **새로운 병합 commit을 생성**하고, 각각의 브랜치에서의 변경 내용을 포함합니다.

[##_Image|kage@bwOCWZ/btsBzK7bxrj/YwBXhTVN1qv1mRaImZ6fr1/img.png|CDM|1.3|{"originWidth":784,"originHeight":428,"style":"alignCenter"}_##]

두 개 이상의 개발 히스토리를 하나로 합치는 작업이기 때문에, merge를 하게 되면 각각의 개발자가 작업한 **history가 모두 보존**된다는 특징을 가지고 있습니다.

merge는 사용법이 단순하고 history가 연대순으로 기록되기 때문에 이해하기 쉽고 추적하기 용이하다는 장점이 존재합니다.

하지만, 하나의 git에서 같이 개발하는 동료가 늘어날 수록 merge commit이 기하급수적으로 늘어나기 때문에, history를 깔끔하게 관리하기 힘들며, commit log 또한 굉장히 지저분해질 가능성이 높습니다.

[##_Image|kage@br9RwT/btsBvfAHgnz/PH3wcMa5lPLVPPWZkwikEk/img.png|CDM|1.3|{"originWidth":793,"originHeight":619,"style":"alignCenter","width":673,"height":525}_##]

## **2\. git rebase**

rebase는 말그대로 base를 옮기는 과정인데, **현재의 branch의 commit들을 다른 branch의 최신 commit 위로 옮기는 과정**을 뜻합니다.

```
git checkout feature-branch	// 작업 중인 branch로 이동
git rebase main			// base를 옮기고 싶은 brach를 입력
```

rebase는 한 branch에서 다른 branch로 변경 사항을 통합하는 다른 방법입니다.

즉, rebase는 모든 변경 사항을 하나의 패치로 **압축**합니다.

그런 다음 base를 옮길 타겟 branch에 패치를 통합합니다.

**\*\* 주의**

master에서 rebase하는 것은 피하도록 하자!

다른 사람이 해당 branch에서 작업중일 수도 있기 때문에 (영향이 가고, 다른 사람들이 접근할 수 있음) 해당 branch에서 rebase는 큰 화를 불러일으킨다.

[##_Image|kage@W4HTV/btsBveu1tS9/YEIBrZjFBWOtPnwyPtHwd1/img.png|CDM|1.3|{"originWidth":774,"originHeight":456,"style":"alignCenter","width":681,"height":401}_##]

rebase의 경우에 history가 선형적으로 깔끔하게 유지가 됩니다.

그렇기 때문에 branch를 깔끔하게 유지할 수 있으며, history가 더 간결해질 수 있다는 장점이 존재합니다.

하지만, 소수의 commit으로 기능을 축소하면 내용이 숨겨질 수 있으며, 진행되었던 history가 변경되기 때문에 공동 작업 중인 팀원들에게 주의가 필요합니다.

또한, remote branch로 rebase를 하려면 `git push --force` 필요하다는 단점이 존재합니다. (강제로 push하는 과정에서 conflict가 발생할 수 있음)

## **3\. 정리**

[##_Image|kage@dvqwYc/btsBzzklG6V/WcxaZikXr6veVDmR9F76xK/img.png|CDM|1.3|{"originWidth":619,"originHeight":435,"style":"alignCenter","width":511,"height":359}_##]

merge는 history를 보존하고,

rebase는 history를 재작성한다.

commit log를 어떻게 관리하고 git을 어떻게 사용하는가에 따라 알맞게 rebase와 merge를 사용하면 된다.

\[참고\]

[https://www.freecodecamp.org/korean/news/an-introduction-to-git-merge-and-rebase-what-they-are-and-how-to-use-them-131b863785f/](https://www.freecodecamp.org/korean/news/an-introduction-to-git-merge-and-rebase-what-they-are-and-how-to-use-them-131b863785f/)
