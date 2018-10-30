# Extended Euclidean Algorithm

> Extended Euclidean algorithm은 *ax + by = gcd(a, b)*를 만족하는 해 *x, y*를 구하는 알고리즘이다.
>
> **x, y를 구하는 방법으로는, **
>
> (1) *ax + by = gcd(a, b)는*
>
> _bx' + (a - (a/b) * b) * y' = gcd(b, a%b) = gcd(a, b)를 이용하여 구할 수 있다._ 
>
> * (a - (a/b) * b)는 a%b를 풀어 쓴 것으로 나머지를 무시하고 몫을 구하는 연산이다.
>
> 
>
> (2) _위의 과정을 a와 b꼴로 정리하면_
>
> _ay' + b * (x' - (a/b) * y') = gcd(a, b)이다._
>
> *ax + by = gcd(a, b)* 와 _ay' + b * (x' - (a/b) * y')_를 비교해보면 
>
> **x = y'**
>
> **y = x' - (a/b) * y'**
>
> 
>
> **Extended Euclidean Algorithm pseudocode** 
>
> ```
> Extended_Euclid(*x, *y, a, b)
> 	if(b = 0)
> 		*x <- 1
> 		*y <- 0
> 		return a
> 	
> 	ans <- gec(&x', &y', b, a mod b)
> 	
> 	*x <- y'
> 	*y <- x' - (a div b) mul y'
> 	
> 	return ans
> ```

Source code : [github/hjhng125](https://github.com/hjhng125/Algorithm/blob/master/GC_Algorithm/Extended_Euclid.c)

