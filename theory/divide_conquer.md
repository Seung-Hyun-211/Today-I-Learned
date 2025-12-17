# Divide Conquer

주어진 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제를 재귀를 통해 해결하는 방법<br>
문제를 둘 이상의 부분 문제로 나누는 방법이 있어야 하며, 반대로 조합해 원래 답을 계산할 수 있어야 한다.

### 구성요소
* divide : 문제를 더 작은 문제로 분할
* merge : 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합
* base case : 분할하지 않고 곧장 풀 수 있는 작은 문제
----
### 예시
#### 1~n의 합

FastSum(n) = 2 * FastSum(n/2) + (n/2)^(n/2)
```
int FastSum(int n)
{
    if(n == 1)
        return 1;
    if(n % 2 == 1)
        return FastSum(n-1) + n;

    return 2 * FastSum(n/2) + (n/2)*(n/2);
}
```

#### 거듭제곱

A^m = A^(m/2)*A^(m/2)
```
int pow(const int& A, int m)
{
    if(m == 0)
        return 1;
    if(m % 2 == 1)
        return pow( A, m-1 ) * A;
    
    half = pow( A, m/2 );
    return half*half;
}
```
