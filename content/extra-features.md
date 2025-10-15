# 셸 명령어, 매직 및 위젯

```{questions}
- 코드, 텍스트, 출력 외에 다른 기능이 있나요?
```

```{objectives}
- 도움말에 접근하는 방법을 배웁니다.
- 매직과 셸 명령어를 사용하는 방법을 배웁니다.
- 위젯을 사용하는 방법을 배웁니다.
```


## 추가 기능

### 도움말 접근

물음표를 사용하여 객체에 대한 도움말을 얻을 수 있습니다:
```
import numpy as np
np.sum?
```

또는 물음표 두 개를 사용하여 소스 코드도 볼 수 있습니다:
```
np.sum??
```

패턴과 일치하는 모듈의 모든 이름을 나열합니다:
```
?np.*sum*
```

출력:
```
np.cumsum
np.einsum
np.einsum_path
np.nancumsum
np.nansum
np.sum
```

`%quickref`는 기능 및 단축키의 빠른 참조 카드를 보여줍니다:
```python
%quickref
```

---


### 셸 명령어

- "!"를 앞에 붙여 셸 명령어를 실행할 수 있습니다.
  - Windows에서는 GitBash에서 다음 옵션을 활성화해야 합니다:
  `Windows 명령 프롬프트에서 Git 및 선택적 Unix 도구 사용`
- 셀 명령어가 상호 작용을 요구하지 않는지 확인하세요.

```
!echo "hello"
```

출력:
```
hello
```

셸 명령어의 출력을 캡처할 수도 있습니다:
```
notebooks = !ls *.ipynb
```

- 일반적인 리눅스 셸 명령어는 *매직*으로도 사용할 수 있습니다: %ls, %pwd, %mkdir, %cp, %mv, %cd, *등*.
- 셸 명령어를 사용하는 것은 새로운 아이디어를 테스트할 때 유용할 수 있지만 **재현 가능한 노트북을 위해서는 셸 명령어에 주의해야 합니다**(다른 컴퓨터에서도 작동할까요?).

---

### 매직

매직은 Jupyter의 기능을 크게 확장하는 간단한 명령어 언어입니다.

매직에는 두 가지 종류가 있습니다:

 - **라인 매직**: % 문자 하나를 앞에 붙이고 인수가 현재 줄의 끝까지만 확장되는 명령어입니다.
 - **셀 매직**: 마커로 두 개의 퍼센트 문자(%%)를 사용하고, 전체 셀을 인수로 받습니다(셀의 첫 번째 줄에 사용해야 함).

`%lsmagic`은 사용 가능한 모든 라인 및 셀 매직을 나열합니다:
```python
%lsmagic
```

물음표는 도움말을 보여줍니다:
```
%sx?
```

추가 매직을 설치하거나 생성할 수도 있습니다.

---

### 위젯

위젯은 노트북에 더 많은 상호 작용을 추가하여 데이터, 매개변수 등의 변경 사항을 시각화하고 제어할 수 있게 합니다.

```python
from ipywidgets import interact
```

> `ipywidgets` 패키지는 표준 CodeRefinery conda 환경에 포함되어 있지만, 위젯이 작동하는 데 문제가 발생하면 공식 [설치 지침](https://ipywidgets.readthedocs.io/en/latest/user_install.html)을 참조하십시오.

#### `interact`를 함수로 사용
```python
def f(x, y, s):
    return (x, y, s)

interact(f, x=True, y=1.0, s="Hello");
```

#### `interact`를 데코레이터로 사용
```python
@interact(x=True, y=1.0, s="Hello")
def g(x, y, s):
    return (x, y, s)
```

> ## 작동하지 않나요? 확장을 설치해야 합니다.
>
> 위젯 인터페이스를 설치해야 합니다. JupyterLab은 모듈식이며 일부 부분은 확장으로 설치해야 합니다. 일반적으로 명령을 셸(JupyterLab 셸도 괜찮음)에 복사하여 붙여넣으십시오. [설치 지침](https://coderefinery.github.io/installation/jupyter/#jupyterlab-extension-manager)을 참조하십시오.
>
> 설치 후에는 페이지를 새로고침하여 활성화해야 합니다(pip 또는 conda로 설치한 경우 전체 JupyterLab 서버를 다시 시작해야 함).
{: .discussion}

---

```{challenge} 몇 가지 유용한 매직 명령어
pi 계산 노트북을 사용하여 몇 가지 매직 명령어를 사용하는 연습을 하십시오.
셀 매직은 셀의 첫 번째 줄에 있어야 한다는 것을 기억하십시오.
1. `num_points`에 대한 for-loop가 있는 셀(다트 던지기)에서 ``%%timeit`` 셀 매직을 추가하고 셀을 실행하십시오.
2. 같은 셀에서 대신 `%%prun` 셀 프로파일링 매직을 시도해 보십시오.
3. 코드에 버그를 도입해 보십시오(예: 잘못된 변수 이름 사용: `points.append((x, y2, True))`)
   - 셀을 실행하십시오.
   - 예외가 발생한 후 새 셀에서 `%debug` 매직을 실행하여 대화형 디버거로 들어갑니다.
   - 도움말 메뉴를 보려면 `h`를 입력하고, 키워드에 대한 도움말을 보려면 `help <keyword>`를 입력하십시오.
   - `x`의 값을 인쇄하려면 `p x`를 입력하십시오.
   - `q`를 입력하여 디버거를 종료하십시오.
4. `%lsmagic`의 출력을 살펴보고, 관심 있는 매직 명령어에 대한 도움말을 보려면 물음표와 이중 물음표를 사용하십시오.
```

````{challenge} 위젯으로 놀아보기

위젯을 사용하여 데이터를 대화형으로 탐색하거나 분석할 수 있습니다.

1. pi 근사 예제로 돌아가서 이전에 작성한 코드를 재사용하는 새 셀을 만듭니다. 하지만 이번에는 코드를 함수에 넣습니다. 이렇게 하면 세부 정보가 "숨겨지고" 나중에 또는 다른 노트북에서 함수를 재사용할 수 있습니다:
   ```python
   import random
   from ipywidgets import interact, widgets

   %matplotlib inline
   from matplotlib import pyplot


   def throw_darts(num_points):
       points = []
       hits = 0
       for _ in range(num_points):
           x, y = random.random(), random.random()
           if x*x + y*y < 1.0:
               hits += 1
               points.append((x, y, True))
           else:
               points.append((x, y, False))
       fraction = hits / num_points
       pi = 4 * fraction
       return pi, points


   def create_plot(points):
       x, y, colors = zip(*points)
       pyplot.scatter(x, y, c=colors)


   def experiment(num_points):
       pi, points = throw_darts(num_points)
       create_plot(points)
       print("approximation:", pi)
   ```
2. `experiment` 함수를 `num_points`를 2000으로 설정하여 호출해 보십시오.
3. 포인트 수를 대화형으로 변경할 수 있는 셀을 추가합니다:
   ```python
   interact(experiment, num_points=widgets.IntSlider(min=100, max=10000, step=100, value=1000))
   ```
   `Error displaying widget: model not found`가 발생하면 페이지를 새로고침해야 할 수 있습니다.
4. 슬라이더를 앞뒤로 드래그하여 결과를 관찰하십시오.
5. 위젯의 다른 흥미로운 용도를 생각해 볼 수 있습니까?
````

```{discussion} RShiny는 ipywidgets에 대한 훌륭한 R 대안/솔루션입니다.
[RShiny](https://shiny.rstudio.com)는 R 개발자에게 흥미로울 수 있는 ipywidgets에 대한 훌륭한 R 대안/솔루션입니다.

예를 들어, [예제 갤러리](https.shiny.rstudio.com/gallery)를 참조하십시오.
```

---

```{keypoints}
- Jupyter 노트북에는 유용할 수 있는 여러 추가 기능이 있습니다.
```