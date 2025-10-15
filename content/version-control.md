# 노트북 및 버전 관리

```{objectives}
- 노트북의 버전 관리를 더 쉽게 만들어주는 두 가지 도구를 시연합니다.
```

```{instructor-note}
- 10분 강의
- 5분 데모
```

Jupyter 노트북은 [JSON](https://en.wikipedia.org/wiki/JSON) 형식으로 저장됩니다.
이 형식을 사용하면 노트북 인터페이스를 통해 도입된 변경 사항을 비교하고 병합하기가 약간 어려울 수 있습니다.


## 버전 관리를 단순화하는 패키지 및 JupyterLab 확장

Git 및 GitHub와 더 쉽게 상호 작용할 수 있도록 여러 패키지 및 JupyterLab 확장이 개발되었습니다.

- [nbdime](http://nbdime.readthedocs.io/) (노트북 "diff" 및 "merge")는 "내용 인식" 비교 및 병합을 제공합니다.
  - 노트북 인터페이스에 Git 버튼을 추가합니다.
  - `git diff` 및 `git merge` 셸 명령어는 노트북 파일에 대해 nbdime의 비교 및 병합을 사용할 수 있지만, 노트북이 아닌 파일에 대해서는 Git의 동작을 변경하지 않습니다.
- [jupyterlab-git](https://github.com/jupyterlab/jupyterlab-git)은 Git을 사용한 버전 관리를 위한 JupyterLab 확장입니다.
  - JupyterLab 내에서 버전 관리를 위해 왼쪽 메뉴 모음에 Git 탭을 추가합니다.
- [JupyterLab GitHub](https://www.npmjs.com/package/@jupyterlab/github)는 GitHub 리포지토리에 액세스하기 위한 JupyterLab 확장입니다.
  - GitHub 리포지토리에서 노트북을 탐색하고 열 수 있는 GitHub 탭을 왼쪽 메뉴 모음에 추가합니다.

세 가지 확장 모두 JupyterLab 인터페이스 내에서 사용할 수 있으며, 우리의 [Conda 환경](https://coderefinery.github.io/installation/conda-environment/)은 [jupyterlab-git](https://github.com/jupyterlab/jupyterlab-git) 및 [nbdime](http://nbdime.readthedocs.io/)를 제공합니다. 추가 확장을 설치하려면 JupyterLab 확장 설치 및 관리에 대한 [공식 문서](https://jupyterlab.readthedocs.io/en/stable/user/extensions.html)를 참조하십시오.

---

## GitHub에서 Jupyter 노트북 비교

이를 위해서는 GitHub에서 [풍부한 Jupyter 노트북 비교](https://docs.github.com/en/repositories/working-with-files/using-files/working-with-non-code-files#working-with-jupyter-notebook-files-on-github)를 활성화해야 합니다.
- GitHub에서 아바타/이미지(오른쪽 상단)를 클릭합니다.
- "기능 미리보기"를 클릭합니다.
- "풍부한 Jupyter 노트북 비교"를 활성화합니다.

차이점을 보여주기 위해 작은 변경 사항을 만들었으며, 기능을 활성화/비활성화하여 효과를 직접 시도해 볼 수 있습니다.
<https://github.com/coderefinery/jupyter/compare/5ff55b8..fce21e6>

다음은 **"풍부한 Jupyter 노트북 비교"가 없는** 비교입니다.
```{figure} img/github-without-rich-diff.png
:alt: 작은 변경 사항이지만 "풍부한 Jupyter 노트북 비교"가 없음
:width: 100%
:class: with-border

GitHub에서 "풍부한 Jupyter 노트북 비교" 없이 본 노트북의 작은 변경 사항(잘림).
```

다음은 동일한 변경 사항이지만, 이번에는 **"풍부한 Jupyter 노트북 비교"가 활성화된** 경우입니다.
```{figure} img/github-with-rich-diff.png
:alt: "풍부한 Jupyter 노트북 비교"가 활성화된 GitHub에서 본 노트북의 작은 변경 사항
:width: 100%
:class: with-border

GitHub에서 본 동일한 변경 사항, 이번에는 "풍부한 Jupyter 노트북 비교"가 활성화됨.
```

---

## jupyterlab-git/nbdime 없이 로컬에서 변경 사항 비교

(plain-git-diff)=

```{instructor-note}
- 새 폴더 만들기
- 새 Git 리포지토리 초기화(어쨌든 시연하기에 좋음)
- "darts" 노트북을 복사(이전 에피소드에서)
- `.ipynb_checkpoints/`를 `.gitignore`에 추가
- 아래 변경 사항을 시도하기 전에 파일 스테이징 및 커밋
```

```{exercise} 강사가 일반 git diff 시연
1. 문제를 이해하기 위해 강사는 먼저 [예제 노트북](https://github.com/coderefinery/jupyter/blob/main/example/darts.ipynb)을 보여준 다음 JSON 형식의 [소스 코드](https://raw.githubusercontent.com/coderefinery/jupyter/main/example/darts.ipynb)를 보여줍니다.
2. 그런 다음 예제 노트북에 간단한 변경 사항을 도입합니다. 예를 들어 색상 변경("red" 및 "blue"를 다른 것으로 변경) 및 `fig.set_size_inches(6.0, 6.0)`에서 치수 변경.
3. 모든 셀을 실행합니다.
4. 변경 사항을 저장하고(저장 아이콘) JupyterLab 터미널에서 "일반" `git diff`를 시도하고 이것이 그다지 유용하지 않다는 것을 확인합니다. 이유를 토론합니다.
```

---

## jupyterlab-git/nbdime으로 변경 사항 비교

jupyterlab-git(nbdime 사용)을 사용하여 동일한 변경 사항을 검사해 봅시다.
이것은 우리가 만든 변경 사항만 강조 표시하므로 더 편리합니다.

```{figure} img/git.jpg
:alt: jupyterlab-git/nbdime으로 변경 사항 비교

jupyterlab-git/nbdime으로 변경 사항 비교. Git 탭을 클릭한 다음 더하기-빼기 기호를 클릭합니다.
```

---

## 명령줄에서 nbdime 사용

노트북을 비교하고 병합할 때 항상 nbdime을 사용하도록 (명령줄) Git을 구성할 수 있습니다.
```console
$ nbdime config-git --enable --global
```
이제 노트북으로 git diff 또는 git merge를 수행하면 멋진 비교 보기를 볼 수 있습니다.
자세한 내용은 [해당 문서](https://nbdime.readthedocs.io/en/latest/#git-integration-quickstart)를 참조하십시오.

---

## 참조

- [fast.ai](https://www.fast.ai/)에서 개발한 [nbdev](https://nbdev.fast.ai/getting_started.html)는 [git 친화적인 Jupyter 노트북](https://nbdev.fast.ai/tutorials/git_friendly_jupyter.html) 지원을 포함하는 노트북 중심 개발 플랫폼입니다.
- [Verdant](https://github.com/mkery/Verdant/)는 Jupyter 노트북에서 실행하는 모든 실험의 기록을 자동으로 기록하고 `.ipyhistory` JSON 파일에 저장하는 JupyterLab 확장입니다.