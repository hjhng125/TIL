# Euclid's Algorithm

> Euclid's algorithm이란 0이 아닌 두개의 정수 a, b 사이의 최대공약수를 구하는 알고리즘이다.
>
> GCD - greatest common divisor
>
> 다음은 Euclid's algorithm의 기본 원리이다.
>
> 
>
> **임의의 두 자연수 a, b가 주어졌을 때,  a가 큰 값이라고 가정하자.**
>
> * GCD(a, b) = GCD(b, a)
>
> * GCD(a, b) = GCD(-a, b)
>
> * GCD(a, b) = GCD(|a|, |b|)
>
> * GCD(a, 0) = |a|
>
> * GCD(a, k*a) = |a| for and k ∈ z
>
>   
>
>  **Euclid's algorithm pseudocode**
>
> ```
> Euclid(a, b)
> 	if b = 0 then
> 		return a
> 	else 
> 		return Euclid(b, a mod b)
> ```
>
> 
>
> **Correctness**
>
> case: a = b - Euclid(a, b) calls Euclid(b, 0), return b.
>
> case: a < b - Euclid(a, b) calls Euclid(b, a).
>
> case: a > b - Suppose d = gcd(a, b). 
>
> ​			if b = 0, then d = a. 
>
> ![image](C:\Users\HJH\Documents\GitHub\TIL\Algorithm\image.PNG)

[github/hjhng125](https://github.com/hjhng125/Algorithm/blob/master/GC_Algorithm/Euclid's%20Algorithm.cpp)



