# CSS Selector

>선택자는 어떤 태그에 CSS를 입혀줄 것인지 선언하는 것이다.
>
>속성은 선택한 태그에서 어떠한 부분을 바꿀 것인지 값은 그 속성에 대해 어떻게 바꿔줄 건지 정해주는 것이다.

 

### Type of Selector

> * Type Selector : h1, p, div, span 등 HTML 요소를 선택하는 선택자.
>
> * ID Selecotr : 특정 값을 ID 속성의 값으로 갖는 요소를 선택하는 선택자로 속성값 앞에 #을 붙여 ID임을 나타낸다.
>
> * Descendant Selector : 특정 요소의 하위에 있는 요소를 선택하는 선택자.
>
>   ex) div blockquote : div 요소의 하위에 있는 blockquote 요소 선택.
>
>   ​                                    이때 div와 blockquote사이에 요소가 더 있어도 선택됨.
>
> * Class Selector :  특정 값을 class 속성 값으로 갖는 요소를 선택하는 선택자로 속성값 앞에 .을 붙여 class임을 나타낸다.
>
> * Comma Combinator : 여러개의 속성을 선택할 때 ,를 이용하여 묶는다.
>
> * Universal Selector : 모든 HTML 요소를 선택하는 선택자로 * 로 나타낸다.
>
> * Sibling Selector : 어떤 요소의 형제 요소를 선택하는 선택자.
>
>   ex) h1 ~ p : h1 요소의 형제 요소 중 p 요소를 선택(다중 선택)
>
> * Adjacent Sibling Selector : 인접 형제 선택자는 어떤 요소의 형제 요소 중 첫번째 요소를 선택하는 선택자다. 
>
>   ex) h1 + p : h1 요소의 형제 요소 중 첫번째 p 요소를 선택(한 개 선택)
>
> * Child Selector : 특정 요소의 자식 요소를 선택하는 선택자.
>
>   ex) div > blockquote : div 요소의 자식 중 blockquote를 선택. 
>
> * First Child Pseudo-selector : first-child는 첫번째 요소를 선택합니다.
>
> * Only Child Pseudo-selector : only-child는 어떤 요소의 자식이 하나밖에 없을 때 적용합니다.
>
> * Last Child Pseudo-selector : last-child는 요소의 마지막 자식을 선택합니다.
>
> * Nth Child Pseudo-selector : nth-child는 요소의 n번째 자식을 선택합니다.
>
> * Nth Last Child Selector : nth-last-child는 요소의 뒤에서부터 n번째 자식을 선택합니다.
>
> * First-of-type : 특정 타입의 첫번째 요소를 선택합니다.
>
> * Nth-of-type : 특정 타입의 n번째 요소를 선택합니다.
>
> * Only-of-type : 같은 유형의 형제가 없을 때 사용합니다.
>
> * Last-of-type : 특정 타입의 마지막 요소를 선택합니다.
>
> * Empty Selector : 자식을 가지고 있지 않은 요소를 선택합니다.
>
> * Negation Pseudo-class : 부정 선택자와 같지 않은 것들을 선택합니다.
>
>   ex) p:not(.choo) : class name이 choo가 아닌 p들을 선택.
>
> * Attribute Selector : 특정 속성을 가진 요소들을 선택. [attribute]
>
> * Attribute Value Selector : 특정 속성 값을 가지는 요소들을 선택. [attribute = "value"]
>
>   * Attribute Starts With Selector :  특정 단어로 시작하는 속성값. [attribute ^= "value"]
>   * Attribute Ends With Selector : 특정 단어로 끝나는 속성값. [attribute $= "value"]
>   * Attribute Wildcard Selector : 특정 단어를 포함하는 속성값. [attibute *- "value"]

### CSS Selector & Web Crawling

> 웹 크롤링과 CSS 선택자는 매우 깊은 연관이 있다. 크롤링으로 특정 태그를 추출하기 위해서는 적절한 타입의 CSS 선택자 문법을 사용해야 하기 때문에 위의 각 선택자를 잘 알아두어야 할 것이다. 