# Writing Efficient Python Code



## Foundations for efficiencies

효율적인 코드란

1. 빠르고 실행과 결과 반환 사이의 대기 시간이 짧음
2. 효율적인 코드는 리소스를 능숙하게 할당하고 불필요한 오버헤드를 겪지 않음



=> 어쨌든 목표는 대기시간과 오버헤드를 줄이는 것임



ex)

리스트에 append하는 상황일 때

- index 접근 - least
- 요소 접근 - middle
- [name for name in names if len(name) >= 4] => 가장 효율적인 코드



The Zen of Python

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```



### Numpy

데이터 사이언스에서 중요한 모듈임

주로 Numpy 배열 많이 사용할 예정

파이썬의 일반 배열보다 Numpy 배열이 좀 더 memory save임





## Runtime



%timeit : 런타임 값 출력해줌(시간평균 + 표준 편차)



%timeit -r숫자1 -n숫자2 : 실행횟수 숫자1 / 루프 수 숫자2

%timeit -o => 변수에 저장 가능



### code profiling

프로그램의 다양한 부분이 실행되는 기간과 빈도를 설명하는데 사용되는 기술

%timeit 같은 명령 없이 코드의 개별 부분에 대한 통계를 제공



