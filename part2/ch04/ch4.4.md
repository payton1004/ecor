---
title: "ch4.4"
---

<h2> 4.4 행렬대수 기초 및 R </h2>

----------

<h3> 4.4.1 대각합 </h3>

정방행렬 A 의 대각합(trace) = 대각선 값들의 합

tr(A) 로 표기한다.



```r
A <- matrix(1:9, nrow=3, byrow=T)
A
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    4    5    6
## [3,]    7    8    9
```

```r
sum(diag(A))
```

```
## [1] 15
```

**대각합의 속성**

> tr(kA) = k * tr(A)

> tr(A + B) = tr(A) + tr(B)

> tr(A') = tr(A)

> tr(AB) = tr(BA)

<br>
----------

<h3> 4.4.2 행렬식 </h3>

행렬에 행렬식(deteminant) 계산 결과는 스칼라 값

det(A) 또는 |A| 로 표기한다.

2차 정방행렬에서는 ad-bc (기억나~니?) 인데, 

3차 이상은... 복잡하다.

그냥 det 함수를 쓰자 =_=;;


```r
A <- matrix(c(5,2,7,6,3,-3,1,0,0), nrow=3)
det(A)
```

```
## [1] -27
```


**행렬식과 관련 속성**

> |A'| = |A|

> |AB| = |A||B|

> 대각행렬 A 이면, |A| = a<sub>11</sub>a<sub>22</sub>  ...  a<sub>mm</sub> 이다. (대각의 곱)

> 한 행이라도 모두 0 이면 |A| = 0

> 두 행 또는 두 열이 동등하면 |A| = 0

> 한 행이나 한 열이 다른 행이나 열의 곱이면 |A| = 0

<br>
----------

<h3> 4.4.3 계수와 비특이행렬 </h3>

계수(rank) 란 행렬의 선형독립인 행(또는 열) 의 최대 수


만약 |A| = 0 이면,

A 는 특이행렬(singular matrix) 이고,

|A| != 0 이면 비특이행렬 (nonsingular matrix) 이다.


다시 계수를 설명하자면,

다른 행이나 열이 어떤 행이나 열의 스칼라 곱으로 표현 가능하지 

않은 갯수이다.


S = [1 2]
     2 4]
     
의 계수인 r(S) = 1 이다.

2번째 행은 1번째 행의 *2 으로 구해지기 때문.

S 행렬은 2 x 2 차수이나 계수는 1이다.

n = 2 > r(S) = 1 이므로 S 는 특이행렬이다.

R 에서는 

dim() 함수로 행렬 차수 조사

rankMatrix() 함수로 행렬 계수 조사 

단, Matrix 패키지에 포함되어 있어서 rankMatrix 사용하려면,

library() 함수로 패키지 실행부터 해야한다.


```r
library(Matrix)

M <- matrix(c(4,2,7,5,3,-3,1,0,2), nrow=3)
M
```

```
##      [,1] [,2] [,3]
## [1,]    4    5    1
## [2,]    2    3    0
## [3,]    7   -3    2
```

```r
dim(M)
```

```
## [1] 3 3
```

```r
rankMatrix(M) # M 행렬의 계수
```

```
## [1] 3
## attr(,"method")
## [1] "tolNorm2"
## attr(,"useGrad")
## [1] FALSE
## attr(,"tol")
## [1] 6.661e-16
```


**행렬 계수 속성**

> 0 행렬은 계수가 0 이다.

> m x n 행렬이면, r(A) <= min(m,n)

> r(AB) <= min(r(A), r(B))

> r(A) = r(A') = r(AA') = r(A'A)

> 행렬계수는 비특이행렬의 전승, 후승에 의해 변하지 않는다.

> m x n 행렬일때,
  A 가 비특이행렬(|A| != 0) 이면 r(A) = n 이고,
  A 가 특이행렬(|A| = 0) 이면 r(A) < n 이다.
  
<br>
----------

<h3> 4.4.4 선형종속 </h3>

선형종속 (linear dependence) 

n 개의 벡터{x1, x2, .. xn} 과 n 개의 스칼라 {k1, k2, .. kn}에서 

k1x1 + k2x2 + ... + knxn = 0 을 충족하는 

0이 아닌 스칼라 집합이 있다면 선형종속이다.

쉽게 말해,

어떤 벡터 xi 는 다른 벡터의 조합으로 나타낼 수 있을 경우.

그렇지 않은 경우를 선형독립 (linear independence) 라고 한다.



```r
X <- matrix(c(4,5,2,14,3,9,6,21,8,10,7,28,1,2,9,5), nrow=4, byrow=T)

# 각 열벡터 생성
x1 <- X[,1]  
x2 <- X[,2]
x3 <- X[,3]
x4 <- X[,4]

# 선형 종속 검증 
x1 + 2*x2 + 0*x3 - x4 
```

```
## [1] 0 0 0 0
```


<br>
----------

<h3> 4.4.5 역행렬 </h3>

n x n 정방행렬의 역행렬(inverse matrix) 는 

행렬 A 의 전승 또는 후승(앞에서 곱 또는 뒤에서 곱) 으로 항등행렬 *I* 를 만드는 행렬

A^^-1^^ 로 표기한다.


역행렬이 존재하기 위해서는 **비특이행렬**이어야 한다.


여인자행렬 (cofactor matrix) C = {c<sub>ij</sub>},

A 의 수반행렬 (adjoint matrix) adjA = C' 로 정의하면,

A 의 역행렬은 

A^^-1^^ = 1/|A| * adjA


ex) A = [3 2; 1 0] 행렬일때, 
    행렬식은 -2 로, 0 이 아니므로 (특이행렬이 아님으로)
    역행렬이 존재한다.


```r
A <- matrix(c(3,2,1,0), nrow=2, byrow=T)
A
```

```
##      [,1] [,2]
## [1,]    3    2
## [2,]    1    0
```

```r
# 여인자행렬 cofactor matrix
C <- matrix(c(0,-1,-2,3), nrow=2, byrow=T)
C
```

```
##      [,1] [,2]
## [1,]    0   -1
## [2,]   -2    3
```

```r
# 수반행렬 adjoint matrix
adjA <- t(C)
adjA
```

```
##      [,1] [,2]
## [1,]    0   -2
## [2,]   -1    3
```

```r
Ainv_1 <- 1 / det(A) * adjA 
Ainv_1
```

```
##      [,1] [,2]
## [1,]  0.0  1.0
## [2,]  0.5 -1.5
```

```r
Ainv_2 <- solve(A)
Ainv_2
```

```
##      [,1] [,2]
## [1,]  0.0  1.0
## [2,]  0.5 -1.5
```


**역행렬 속성**

> 역행렬이 존재하면 그것이 유일하다.

> (A')^^-1^^ = (A^^-1^^)'

> (AB)^^-1^^ = B^^-1^^A^^-1^^

> |A^^-1^^| = 1/|A|

