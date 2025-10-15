# Jupyter 기능 예제

```{questions}
- Jupyter 사용의 다양한 측면을 연습하기 위한 혼합 예제/연습 문제
```

```{objectives}
- 위젯의 고급 사용법을 배웁니다.
- 코드를 프로파일링하고 새로운 라인 프로파일러 도구를 설치하는 방법을 배웁니다.
- pandas 데이터프레임을 사용하여 데이터 분석을 연습합니다.
- 자신만의 매직 명령을 정의하는 방법을 배웁니다.
- ipyparallel을 사용하여 Python 코드를 병렬화하는 방법을 배웁니다.
- 동일한 노트북에서 Python과 R을 혼합하는 방법을 배웁니다.
```

---

(interactive-data-fitting)=

## 대화형 데이터 피팅을 위한 위젯

`````{exercise} 대화형 데이터 피팅을 위한 위젯
위젯은 재미있을 뿐만 아니라 유용할 수도 있습니다. 다음은 노이즈가 있는 데이터를 대화형으로 피팅하는 방법을 보여주는 예제입니다.

1. 아래 셀을 실행하세요. 임의의 노이즈가 있는 가우시안 함수에 5차 다항식을 피팅합니다.
2. 마지막 두 코드 라인 주위에 `@interact` 데코레이터를 사용하여
   다항식 차수 `n`이 3에서 30까지 범위의 피팅을 시각화할 수 있도록 합니다:

```python
import numpy as np

import matplotlib.pyplot as plt
%matplotlib inline

def gaussian(x, a, b, c):
    return a * np.exp(-b * (x-c)**2)

def noisy_gaussian():
    # gaussian array y in interval -5 <= x <= 5
    nx = 100
    x = np.linspace(-5.0, 5.0, nx)
    y = gaussian(x, a=2.0, b=0.5, c=1.5)
    noise = np.random.normal(0.0, 0.2, nx)
    y += noise
    return x, y

def fit(x, y, n):
    pfit = np.polyfit(x, y, n)
    yfit = np.polyval(pfit, x)
    return yfit

def plot(x, y, yfit):
    plt.plot(x, y, "r", label="Data")
    plt.plot(x, yfit, "b", label="Fit")
    plt.legend()
    plt.ylim(-0.5, 2.5)
    plt.show()

x, y = noisy_gaussian()
yfit = fit(x, y, n=5)  # fit a 5th order polynomial to it
plot(x, y, yfit)
```
````{solution}
```python
import numpy as np

from ipywidgets import interact

import matplotlib.pyplot as plt
%matplotlib inline

def gaussian(x, a, b, c):
    return a * np.exp(-b * (x-c)**2)

def noisy_gaussian():
    # gaussian array y in interval -5 <= x <= 5
    nx = 100
    x = np.linspace(-5.0, 5.0, nx)
    y = gaussian(x, a=2.0, b=0.5, c=1.5)
    noise = np.random.normal(0.0, 0.2, nx)
    y += noise
    return x, y

def fit(x, y, n):
    pfit = np.polyfit(x, y, n)
    yfit = np.polyval(pfit, x)
    return yfit

def plot(x, y, yfit):
    plt.plot(x, y, "r", label="Data")
    plt.plot(x, yfit, "b", label="Fit")
    plt.legend()
    plt.ylim(-0.5, 2.5)
    plt.show()

x, y = noisy_gaussian()

@interact
def slider(n=(3, 30)):
    yfit = fit(x, y, n)
    plot(x, y, yfit)
```
````
`````


---

(cell-profiling)=

## 셀 프로파일링

`````{exercise} 셀 프로파일링
이 연습은 셀 프로파일링에 관한 것이지만, 매직과 셀 작업을 연습하게 될 것입니다.

1. 다음 코드를 복사하여 셀에 붙여넣으세요:
   ```python
   import numpy as np
   import matplotlib.pyplot as plt

   def step():
       import random
       return 1. if random.random() > .5 else -1.

   def walk(n):
       x = np.zeros(n)
       dx = 1. / n
       for i in range(n - 1):
           x_new = x[i] + dx * step()
           if x_new > 5e-3:
               x[i + 1] = 0.
           else:
               x[i + 1] = x_new
       return x

   n = 100000
   x = walk(n)
   ```

2. 함수를 4개의 셀로 나누세요 (편집 메뉴 또는 키보드 단축키 `Ctrl-Shift-minus` 사용).
3. `plt.plot(x)`를 사용하여 랜덤 워크 궤적을 플로팅하세요.
5. 라인 매직으로 `walk()`의 실행 시간을 측정하세요.
6. prun 셀 프로파일러를 실행하세요.
7. 코드를 느리게 만드는 작은 실수를 발견할 수 있나요?
8. 다음 연습에서는 성능 실수를 더 쉽게 노출시킬 라인 프로파일러를 설치합니다.
````{solution}
코드를 여러 셀로 분할합니다 (예: `Ctrl-Shift-minus` 사용)
```python
import numpy as np
```
```python
def step():
    import random
    return 1. if random.random() > .5 else -1.
```
```python
def walk(n):
    x = np.zeros(n)
    dx = 1. / n
    for i in range(n - 1):
        x_new = x[i] + dx * step()
        if x_new > 5e-3:
            x[i + 1] = 0.
        else:
            x[i + 1] = x_new
    return x
```
`n`을 초기화하고 `walk()`를 호출합니다:
```python
n = 100000
x = walk(n)
```
랜덤 워크를 플로팅합니다.
```python
import matplotlib.pyplot as plt
plt.plot(x);
```
`%timeit` 라인 매직을 사용하여 실행 시간을 측정하고 출력을 캡처합니다:
```python
t1 = %timeit -o walk(n)
```
최상의 결과
```python
t1.best
```
`%%prun` 셀 프로파일러로 실행합니다.
```python
%%prun
walk(n)
```
````
`````


---

(installing-magic-command)=

## 라인 프로파일링을 위한 매직 명령 설치

`````{exercise} 라인 프로파일링을 위한 매직 명령 설치
매직은 `pip`를 사용하여 설치하고 `%load_ext` 매직을 사용하여 플러그인처럼 로드할 수 있습니다. 이제 더 자세한 프로파일을 얻기 위해 라인 프로파일러를 설치하고 이전 연습의 코드를 더 빠르게 할 수 있는 통찰력을 얻기를 바랍니다.

1. 이전 연습을 해결하지 않았다면 다음 코드를 복사하여 셀에 붙여넣고 실행하세요:
   ```python
   import numpy as np
   import matplotlib.pyplot as plt

   def step():
       import random
       return 1. if random.random() > .5 else -1.

   def walk(n):
       x = np.zeros(n)
       dx = 1. / n
       for i in range(n - 1):
           x_new = x[i] + dx * step()
           if x_new > 5e-3:
               x[i + 1] = 0.
           else:
               x[i + 1] = x_new
       return x

   n = 100000
   x = walk(n)
   ```

2. 그런 다음 `!pip install line_profiler`를 사용하여 라인 프로파일러를 설치합니다.
3. 다음으로 `%load_ext line_profiler`를 사용하여 로드합니다.
4. `%lprun?`으로 활성화된 새로운 매직 명령을 살펴보세요.
5. 새 셀에서 도움말 페이지에 설명된 방식으로 `walk` 및 `step` 함수에 대해 라인 프로파일러를 실행합니다.
6. 출력을 검사합니다. 이제 실수를 더 쉽게 볼 수 있나요?
````{solution}
코드를 셀에 복사하여 붙여넣습니다.

라인 프로파일러를 설치합니다.
```
!pip install line_profiler
```
IPython 확장을 로드합니다.
```python
%load_ext line_profiler
```
도움말 보기:
```
%lprun?
```
`walk` 함수에서 라인 프로파일러를 사용합니다:
```
%lprun -f walk walk(10000)
```
아하, 대부분의 시간은 `step()` 함수를 호출하는 라인에서 소요됩니다.
`step`에서 라인 프로파일러를 실행합니다:
```
%lprun -f step walk(10000)
```

```
...
     8                                           def step():
     9      9999       7488.0      0.7     52.3      import random
    10      9999       6840.0      0.7     47.7      return 1. if random.random()
...
```
아하! `step` 함수 내에서 수천 번 호출되는 `random` 모듈을 가져오는 데 많은 시간이 소요됩니다. import 문을 함수 밖으로 옮기세요!
````
`````


---

(data-analysis)=

## pandas 데이터프레임으로 데이터 분석

`````{exercise} pandas 데이터프레임으로 데이터 분석
데이터 과학 및 데이터 분석은 Jupyter의 주요 사용 사례입니다. 이 연습에서는 데이터프레임과 고급 `pandas` 데이터 탐색 라이브러리의 다양한 내장 분석 방법에 익숙해질 것입니다. 노벨상에 대한 정보가 포함된 데이터 세트는 파일 브라우저로 볼 수 있습니다.

1. 파일 브라우저에서 `data/` 하위 폴더로 이동하여 `nobels.csv` 데이터 세트를 두 번 클릭하여 시작합니다. 그러면 JupyterLab의 내장 데이터 브라우저가 열립니다.
2. 데이터, 열 이름 등을 살펴보세요.
3. 자신의 노트북에서 `pandas` 모듈을 가져오고 데이터 세트를 *데이터프레임*으로 로드합니다:
```python
import pandas as pd
nobel = pd.read_csv("data/nobels.csv")
```

4. 데이터프레임의 "share" 열에는 상을 공유한 노벨상 수상자 수가 포함되어 있습니다. 다음을 사용하여 이 열의 통계를 살펴보세요.
```python
nobel["share"].describe()
```

5. `describe()` 메서드는 데이터 유형에 대해 스마트합니다. 다음을 시도해보세요:
```python
nobel["bornCountryCode"].describe()
```

    - 가장 많은 노벨상을 수상한 국가는 어디이며 몇 개입니까?
    - 데이터 세트에는 몇 개국이 표시됩니까?
6. 이제 수상자의 나이를 분석합니다. 먼저 "born" 열을 datetime 형식으로 변환해야 합니다:
```python
nobel["born"] = pd.to_datetime(nobel["born"],
                               errors ='coerce')
```

7. 다음으로 수상 연도에서 출생 날짜를 빼고 "age"라는 새 열에 삽입합니다:
```python
nobel["age"] = nobel["year"] - nobel["born"].dt.year
```
 - 이제 `head()` 메서드를 사용하여 처음 10개 항목의 "surname"과 "age"를 인쇄합니다.

8. 이제 두 가지 다른 방법으로 결과를 플로팅합니다:
```python
nobel["age"].plot.hist(bins=[20,30,40,50,60,70,80,90,100], alpha=0.6);
nobel.boxplot(column="age", by="category")
```

9. 스웨덴 출신 노벨상 수상자는 누구입니까? `nobel.loc[CONDITION]` 문을 사용하여 적절한 조건으로 `nobel` 데이터프레임에서 관련 행을 추출할 수 있는지 확인하십시오.

10. 마지막으로 강력한 `groupby()` 메서드를 사용하여 국가별 노벨상 수를 분석하고 고급 `seaborn` 플로팅 라이브러리로 시각화합니다.
 - 먼저 `nobel` 데이터프레임에 1을 포함하는 "number" 열을 추가합니다(아래 계산을 활성화하기 위해).
 - 그런 다음 4개 국가를 추출하고(아래 대체) 데이터프레임의 하위 집합을 만듭니다:
```python
countries = np.array([COUNTRY1, COUNTRY2, COUNTRY3, COUNTRY4])
nobel2 = nobel.loc[nobel['bornCountry'].isin(countries)]
```
 - 다음으로 `groupby()` 및 `sum()`을 사용하고 결과 데이터프레임을 검사합니다:
```python
nobels_by_country = nobel2.groupby(['bornCountry',"category"], sort=True).sum()
```
 - 다음으로 `pivot_table` 메서드를 사용하여 데이터프레임을 스프레드시트와 같은 구조로 재구성하고 결과를 표시합니다:
```python
table = nobel2.pivot_table(values="number", index="bornCountry", columns="category", aggfunc=np.sum)
```
 - 마지막으로 히트맵을 사용하여 시각화합니다:
 ```python
import seaborn as sns
sns.heatmap(table,linewidths=.5);
```
  - `sns.heatmap`에 대한 도움말 페이지를 보고 플롯의 각 셀에 개수 번호로 주석을 다는 입력 매개변수를 찾을 수 있는지 확인하십시오.
````{solution}
```python
import numpy as np
import pandas as pd
nobel = pd.read_csv("data/nobels.csv")
```
```python
nobel["share"].describe()
```
```python
nobel["bornCountryCode"].describe()
```
- 미국은 275개의 상을 받았습니다.
- 76개국이 하나 이상의 상을 받았습니다.

```python
nobel["born"] = pd.to_datetime(nobel["born"], errors ='coerce')
```
열 추가
```python
nobel["age"] = nobel["year"] - nobel["born"].dt.year
```
성과 나이 인쇄
```python
nobel[["surname","age"]].head(10)
```
```python
nobel["age"].plot.hist(bins=[20,30,40,50,60,70,80,90,100],alpha=0.6);
```
```python
nobel.boxplot(column="age", by="category")
```
스웨덴 출신 노벨상 수상자는 누구입니까?
```python
nobel.loc[nobel["bornCountry"] == "Sweden"]
```
마지막으로 강력한 `groupby()` 메서드를 사용해 보세요.
행당 노벨상 수가 포함된 추가 열 추가(통계에 필요)
```python
nobel["number"] = 1.0
```
추가 분석을 위해 몇 개 국가 선택
```python
countries = np.array(["Sweden", "United Kingdom", "France", "Denmark"])
nobel2 = nobel.loc[nobel['bornCountry'].isin(countries)]
```
```python
table = nobel2.pivot_table(values="number", index="bornCountry",
                           columns="category", aggfunc=np.sum)
table
```
```python
import seaborn as sns
sns.heatmap(table,linewidths=.5, annot=True);
```
````
`````


---

(own-magic-command)=

## 자신만의 사용자 지정 매직 명령 정의

`````{exercise} 자신만의 사용자 지정 매직 명령 정의
`IPython.core` 라이브러리의 `@register_cell_magic` 데코레이터를 사용하여 새 매직 명령을 만들 수 있습니다. 여기서는 C++ 코드를 컴파일하고 실행하는 셀 매직 명령을 만듭니다.
이 예제를 사용하려면 컴퓨터에 GNU `g++` 컴파일러가 설치되어 있어야 합니다.

> 이 예제는 Cyrille Rossant의 [IPython Minibook](http://ipython-books.github.io/), Packt Publishing, 2015에서 채택되었습니다.

1. 먼저 `register_cell_magic`을 가져옵니다.
```python
from IPython.core.magic import register_cell_magic
```

2. 다음으로 다음 코드를 셀에 복사하여 붙여넣고 실행하여 새 셀 매직 명령을 등록합니다:
```python
@register_cell_magic
def cpp(line, cell):
    """Compile, execute C++ code, and return the standard output."""

    # We first retrieve the current IPython interpreter instance.
    ip = get_ipython()
    # We define the source and executable filenames.
    source_filename = '_temp.cpp'
    program_filename = '_temp'
    # We write the code to the C++ file.
    with open(source_filename, 'w') as f:
        f.write(cell)
    # We compile the C++ code into an executable.
    compile = ip.getoutput("g++ {0:s} -o {1:s}".format(
        source_filename, program_filename))
    # We execute the executable and return the output.
    output = ip.getoutput('./{0:s}'.format(program_filename))
    print('\n'.join(output))
```

 - 이제 `%%cpp`를 사용하여 매직을 사용할 수 있습니다.

3. 일부 C++ 코드를 셀에 작성하고 실행해 보세요.

4. 다른 노트북에서 매직을 사용하려면 끝에 다음 함수를 추가한 다음 셀을 PYTHONPATH의 파일에 작성해야 합니다. 파일 이름이 `cpp_ext.py`이면 `%load_ext cpp_ext`로 로드할 수 있습니다.
```python
def load_ipython_extension(ipython):
    ipython.register_magic_function(cpp,'cell')
```
````{solution}
```python
from IPython.core.magic import register_cell_magic
```
`load_ipython_extension` 함수를 추가하고 셀을 `cpp_ext.py`라는 파일에 씁니다:
```python
%%writefile cpp_ext.py
def cpp(line, cell):
    """Compile, execute C++ code, and return the standard output."""

    # We first retrieve the current IPython interpreter instance.
    ip = get_ipython()
    # We define the source and executable filenames.
    source_filename = '_temp.cpp'
    program_filename = '_temp'
    # We write the code to the C++ file.
    with open(source_filename, 'w') as f:
        f.write(cell)
    # We compile the C++ code into an executable.
    compile = ip.getoutput("g++ {0:s} -o {1:s}".format(
        source_filename, program_filename))
    # We execute the executable and return the output.
    output = ip.getoutput('./{0:s}'.format(program_filename))
    print('\n'.join(output))

def load_ipython_extension(ipython):
    ipython.register_magic_function(cpp,'cell')
```
확장 로드:
```python
%load_ext cpp_ext
```
cpp 매직에 대한 도움말 얻기:
```
%%cpp?
```
C++로 된 Hello World 프로그램
```cpp
%%cpp
#include <iostream>
using namespace std;

int main()
{
    cout << "Hello, World!";
    return 0;
}
```
````
`````


---

(ipyparallel)=

## ipyparallel을 사용한 병렬 파이썬

`````{exercise} ipyparallel을 사용한 병렬 파이썬
전통적으로 파이썬은 병렬 프로그래밍을 잘 지원하지 않는 것으로 간주되며([ "GIL" 참조](https://en.wikipedia.org/wiki/Global_interpreter_lock)), "적절한" 병렬 프로그래밍은 OpenMP 및 MPI를 활용할 수 있는 Fortran 또는 C/C++와 같은 "헤비듀티" 언어에 맡겨야 합니다.

그러나 IPython은 이제 연구원에게 유용할 수 있는 다양한 스타일의 병렬 처리를 지원합니다. 특히 `ipyparallel`을 사용하면 모든 유형의 병렬 응용 프로그램을 대화식으로 개발, 실행, 디버깅 및 모니터링할 수 있습니다. `ipyparallel`의 가능한 사용 사례는 다음과 같습니다.
- 여러 간단한 접근 방식을 사용하여 당혹스러울 정도로 병렬인 알고리즘을 신속하게 병렬화합니다.
- 동적 로드 밸런싱을 사용하여 CPU 세트에서 작업 세트를 실행합니다.
- (MPI를 사용할 수 있는) 새로운 병렬 알고리즘을 대화식으로 개발, 테스트 및 디버깅합니다.
- (원격 및/또는 분산될 수 있는) 대규모 데이터 세트를 IPython을 사용하여 대화식으로 분석하고 시각화합니다.

이 연습은 시작에 불과하며, 자세한 내용은 [공식 문서](https://ipyparallel.readthedocs.io/en/latest/) 및 [이 상세한 튜토리얼](https://github.com/DaanVanHauwermeiren/ipyparallel-tutorial)을 참조하십시오.

1. 먼저 `conda` 또는 `pip`를 사용하여 `ipyparallel`을 설치합니다. JupyterLab 내에서 터미널 창을 열고 설치를 수행합니다.
2. `ipyparallel`을 설치한 후 "IPython 클러스터"를 시작해야 합니다. 터미널에서 `ipcluster start`로 이 작업을 수행합니다.
3. 그런 다음 노트북에서 `ipyparallel`을 가져오고 `Client` 인스턴스를 초기화하고 엔진에서 직접 실행하기 위한 *DirectView* 개체를 만듭니다:
```python
import ipyparallel as ipp
client = ipp.Client()
print("Number of ipyparallel engines:", len(client.ids))
dview = client[:]
```
4. 이제 병렬 엔진을 시작했습니다. 각 엔진에서 간단한 것을 실행하려면 `apply_sync()` 메서드를 사용해 보세요:
```python
dview.apply_sync(lambda : "Hello, World")
```
5. 정수 제곱의 직렬 평가는 아래 코드 스니펫에서 볼 수 있습니다.
```python
serial_result = list(map(lambda x:x**2, range(30)))
```
 - DirectView 인스턴스의 `map_sync()` 메서드를 사용하여 이를 엔진의 병렬 계산으로 변환합니다. `%%timeit -n 1`을 사용하여 직렬 및 병렬 버전을 모두 시간 측정합니다.

6. 이제 몬테카를로 방법을 사용하여 파이 평가를 병렬화합니다. 먼저 모듈을 로드하고 `random` 모듈을 엔진으로 내보냅니다:
```python
from random import random
from math import pi
dview['random'] = random
```
그런 다음 셀에서 다음 코드를 실행합니다. `mcpi` 함수는 $\\pi$를 계산하는 몬테카를로 방법입니다. 1,000만 개의 샘플 크기(`int(1e7)`)로 `%timeit -n 1`을 사용하여 이 함수의 실행 시간을 측정합니다.
```python
def mcpi(nsamples):
    s = 0
    for i in range(nsamples):
        x = random()
        y = random()
        if x*x + y*y <= 1:
            s+=1
    return 4.*s/nsamples
```
이제 `DirectView` 개체와 샘플 수를 취하고 엔진 간에 샘플 수를 나누고 각 엔진에서 샘플 하위 집합으로 `mcpi()`를 호출하는 불완전한 함수를 가져옵니다. 함수를 완성하고(`____` 필드 대체) $10^7$개의 샘플로 호출하고 시간을 측정하고 직렬 호출과 비교하십시오. `mcpi()`로.
```python
def multi_mcpi(dview, nsamples):
    # get total number target engines
    p = len(____.targets)
    if nsamples % p:
        # ensure even divisibility
        nsamples += p - (nsamples%p)

    subsamples = ____//p

    ar = view.apply(mcpi, ____)
    return sum(ar)/____
```

최종 참고: 파이썬 코드를 병렬화하는 것이 종종 가치가 있지만, 파이썬 코드에서 더 높은 성능을 얻는 다른 방법이 있습니다. 특히 [Numpy](http://www.numpy.org/)와 같은 빠른 수치 패키지를 사용해야 하며, [Numba](https://numba.pydata.org/)를 사용한 적시 컴파일 및/또는 [Cython](http://cython.org/)의 C-확장을 통해 상당한 속도 향상을 얻을 수 있습니다.
````{solution}
터미널을 열고 `ipcluster start`를 실행하고 엔진이 시작될 때까지 몇 초 기다립니다.
모듈 가져오기, 클라이언트 및 DirectView 개체 만들기:
```python
import ipyparallel as ipp
client = ipp.Client()
dview = client[:]
dview
```
```
<DirectView [0, 1, 2, 3]>
```
제곱 람다 함수의 직렬 평가 시간 측정:
```python
%%timeit -n 1
serial_result = list(map(lambda x:x**2, range(30)))
```
DirectView 인스턴스의 `map_sync` 메서드 사용:
```python
%%timeit -n 1
parallel_result = list(dview.map_sync(lambda x:x**2, range(30)))
```
통신 오버헤드로 인해 속도 향상이 없을 수 있습니다.

대신 파이 계산에 집중하십시오. 모듈 가져오기, `random` 모듈을 엔진으로 내보내기:
```python
from random import random
from math import pi
dview['random'] = random
```
```python
def mcpi(nsamples):
    s = 0
    for i in range(nsamples):
        x = random()
        y = random()
        if x*x + y*y <= 1:
            s+=1
    return 4.*s/nsamples
```
```python
%%timeit -n 1
mcpi(int(1e7))
```
```
3.05 s ± 97.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
샘플을 분할하고 청크를 엔진에 디스패치하는 함수:
```python
def multi_mcpi(view, nsamples):
    p = len(view.targets)
    if nsamples % p:
        # ensure even divisibility
        nsamples += p - (nsamples%p)

    subsamples = nsamples//p

    ar = view.apply(mcpi, subsamples)
    return sum(ar)/p
```
```python
%%timeit -n 1
multi_mcpi(dview, int(1e7))
```
```
1.71 s ± 30.4 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
속도 향상이 보입니다!
````
`````


---

(mixing-python-r)=

## 파이썬과 R 혼합

`````{exercise} 파이썬과 R 혼합
이제 목표는 pandas 데이터프레임을 정의하고 이를 R 셀로 전달하여 R 플로팅 라이브러리로 플로팅하는 것입니다.

1. 먼저 필요한 패키지를 설치해야 합니다:
```
!conda install -c r r-essentials
!conda install -y rpy2
```
2. 파이썬 커널에서 R을 실행하려면 rpy2 확장을 로드해야 합니다:
```
%load_ext rpy2.ipython
```

3. 코드 셀에서 다음 코드를 실행하고 pandas 데이터프레임의 기본 플롯 메서드로 플로팅합니다:
```python
import pandas as pd
df = pd.DataFrame({
    'cups_of_coffee': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
    'productivity': [2, 5, 6, 8, 9, 8, 0, 1, 0, -1]
})
```

4. 이제 다음 R 코드를 가져와 `%%R` 매직 명령을 사용하여 위에서 정의한 pandas 데이터프레임을 전달하고 플로팅합니다(방법을 알아보려면 `%%R?` 사용):
```R
library(ggplot2)
ggplot(df, aes(x=cups_of_coffee, y=productivity)) + geom_line()
```

5. 높이, 너비, 단위 및 해상도에 대한 플래그를 사용하여 보기 좋은 그래프를 만드세요.
````{solution}

```python
import pandas as pd
df = pd.DataFrame({
 'cups_of_coffee': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
 'productivity': [2, 5, 6, 8, 9, 8, 0, 1, 0, -1]
})
```
```python
%load_ext rpy2.ipython
```
```python
%%R -i df -w 6 -h 4 --units cm -r 200
# the first line says 'import df and make default figure size 5 by 5 inches
# with resolution 200. You can change the units to px, cm, etc. as you wish.
library(ggplot2)
ggplot(df, aes(x=cups_of_coffee, y=productivity)) + geom_line();
```
````
`````


---

(word-count-widgets)=

## 위젯을 사용한 단어 수 분석

`````{exercise} 위젯을 사용한 단어 수 분석
이 연습은 이전 강의의 [단어 수 프로젝트](https://github.com/coderefinery/word-count)를 사용합니다.

1. `data/` 디렉토리 아래를 살펴보세요. 책의 단어 수 통계가 포함된 네 개의 .dat 파일을 볼 수 있습니다. 하나를 열어볼 수 있습니다.
2. 런처를 열고 새 텍스트 파일을 엽니다.
3. 아래 코드를 텍스트 파일에 복사하여 붙여넣고 `zipf.py` 파일로 저장합니다(구문 강조가 활성화되는 방식 참고).
   ```python
   def load_word_counts(filename):
       """
       Load a list of (word, count, percentage) tuples from a file where each
       line is of the form "word count percentage". Lines starting with # are
       ignored.
       """
       counts = []
       with open(filename, "r") as input_fd:
       	     for line in input_fd:
             if not line.startswith("#"):
             	   fields = line.split()
                   counts.append((fields[0], int(fields[1]), float(fields[2])))
       return counts

   def top_n_word(counts, n):
       """
       Given a list of (word, count, percentage) tuples,
       return the top n word counts.
       """
       limited_counts = counts[0:n]
       count_data = [count for (_, count, _) in limited_counts]
       return count_data

   def zipf_analysis(input_file, n=10):
       counts = load_word_counts(input_file)
       top_n = top_n_word(counts, n)
       return top_n
   ```

4. 새 zipf 모듈을 가져오고 함수 중 하나의 docstring을 살펴보세요:
   ```
   import zipf
   zipf.top_n_word?
   ```

5. 처리된 데이터 파일에 대해 `zipf_analysis()` 함수를 실행합니다. 출력을 플로팅하고 다음 코드를 사용하여 1/N 함수와 비교합니다:
   ```python
   import matplotlib.pyplot as plt
   %matplotlib inline

   nmax = 10
   z = zipf.zipf_analysis("data/isles.dat", nmax)
   n = range(1,nmax+1)
   z_norm = [i/z[0] for i in z]
   plt.plot(n,z_norm)
   inv_n = [1.0/i for i in n]
   plt.plot(n, inv_n)
   ```

6. 예를 들어 다음 코드를 사용하여 Zipf의 법칙을 분석하기 위한 대화형 위젯을 추가합니다:
   ```python
   from ipywidgets import interact
   import matplotlib.pyplot as plt
   %matplotlib inline

   nmax = 10
   @interact(p=-1.0)
   def zipf_plot(p):
       plt.clf()
       n = range(1,nmax+1)
       for f in ["data/isles.dat", "data/last.dat", "data/abyss.dat", "data/sierra.dat"]:
           z = zipf.zipf_analysis(f, nmax)
           z_norm = [i/z[0] for i in z]
           plt.plot(n,z_norm)
       inv_n = [i**p for i in n]
       plt.plot(n, inv_n)
   ```

7. 위 코드에 `nmax` 위젯 매개변수를 추가하여 x축에 표시되는 단어 수를 제어합니다(예: `nmax=(6,14)`). 두 슬라이더를 모두 사용해 보세요.
````{solution}
단어 수와 역 거듭제곱 모두에 대한 슬라이딩 막대가 있는 위젯 코드:
```python
from ipywidgets import interact
import matplotlib.pyplot as plt
%matplotlib inline

@interact(nmax=(6,14), p=-1.0)
def zipf_plot(nmax, p):
    plt.clf()
    #plt.figure()
    n = range(1,nmax+1)
    for f in ["data/isles.dat", "data/last.dat", "data/abyss.dat",
              "data/sierra.dat"]:
        z = zipf.zipf_analysis(f, nmax)
        z_norm = [i/z[0] for i in z]
        plt.plot(n,z_norm)
    inv_n = [i**p for i in n]
    plt.plot(n, inv_n)
```
````
`````