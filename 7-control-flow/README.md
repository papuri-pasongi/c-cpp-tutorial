# 제어 흐름

프로그램의 실행을 제어할 수 있는 방법인 조건문, 반복문에 대해 다루고, 그 기능들을 사용하기 위한 개념들을 배웁니다.

## 불 대수(boolean algebra)

불 대수는 논리학에서 `진리값(truth value)`, 즉 `참`과 `거짓`을 `1`과 `0`에 대응시키고, 이것에 대한 연산을 덧셈과 곱셈으로 바꾼 대수 체계입니다. 불 대수의 덧셈과 곱셈은 다음 성질을 만족합니다.

| `a` | `b` | `a + b` |
| --- | --- | --- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

| `a` | `b` | `a × b` |
| --- | --- | --- |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

여기서 `1 + 1 = 1`이 되는 것을 제외하면 우리가 아는 덧셈, 곱셈과 큰 차이가 나지 않는 것을 알 수 있습니다. 여기서의 덧셈을 `논리합(logical disjunction)` 또는 `OR 연산`, 곱셈을 `논리곱(logical conjunction)` 또는 `AND 연산`이라고 부릅니다. `또는`과 `그리고`라는 이름으로 불리는 이유는 0을 거짓, 1을 참으로 교체해보면 쉽게 알 수 있습니다.

불 대수에선 이것 말고도 `부정(negation)` 또는 `NOT 연산`이라고 불리는 연산이 있습니다.

| `a` | `¬a` |
| --- | --- |
| 0 | 1 |
| 1 | 0 |

참을 거짓으로, 거짓을 참으로 바꿔주는 연산입니다. 이외에도 여러 연산이 있지만, 나중에 다루도록 하겠습니다.

> 불 대수의 덧셈과 곱셈은 교환법칙과 결합법칙 분배법칙이 모두 성립합니다. 즉, 다음 성질을 모두 만족합니다.
>
> * `a + b = b + a`
> * `a × b = b × a`
> * `a + (b + c) = (a + b) + c`
> * `a × (b × c) = (a × b) × c`
> * `c(a + b) = ca + cb`
>
> 또 `드 모르간의 법칙(De Morgan's laws)`은 불 대수에서도 성립합니다.
>
> * `¬(a + b) = ¬a × ¬b`
> * `¬(a × b) = ¬a + ¬b`

## 논리 연산자(logical operator)

위에서 배운 세 연산은 모두 C에서 연산자로 사용할 수 있습니다. 아쉽게도, 위에서 사용한 연산자와 모양이 다릅니다. C에서 논리합은 `||`, 논리곱은 `&&`, 부정은 `!` 연산자를 사용합니다.

> `|`라는 기호는 `바(bar)`라는 이름이 있습니다. 키보드에서 `Shift + ₩`로 입력할 수 있습니다.

```c
#include <stdio.h>

int main()
{
    printf("%d || %d = %d\n", 0, 0, 0 || 0);
    printf("%d || %d = %d\n", 0, 1, 0 || 1);
    printf("%d || %d = %d\n", 1, 0, 1 || 0);
    printf("%d || %d = %d\n", 1, 1, 1 || 1);
    printf("\n");
    printf("%d && %d = %d\n", 0, 0, 0 && 0);
    printf("%d && %d = %d\n", 0, 1, 0 && 1);
    printf("%d && %d = %d\n", 1, 0, 1 && 0);
    printf("%d && %d = %d\n", 1, 1, 1 && 1);
    printf("\n");
    printf("!%d = %d", 0, !0);
    printf("!%d = %d", 1, !1);
}
```
```
0 || 0 = 0
0 || 1 = 1
1 || 0 = 1
1 || 1 = 1

0 && 0 = 0
0 && 1 = 0
1 && 0 = 0
1 && 1 = 1

!0 = 1
!1 = 0

```

> `printf`에서 `\n`을 사용하면 줄바꿈을 할 수 있게 됩니다. 이것에 대한 자세한 설명은 나중에 문자열에 대해 다룰 때 하겠습니다.

> `\`라는 기호는 `백슬래시(backslash)`라고 불립니다. 사용하시는 폰트가 돋움같은 한국어 폰트이면 `\`는 `₩`로 보일 수도 있습니다.

C를 포함한 대부분의 프로그래밍 언어에선 어떤 값을 논리값으로 사용하게 되면, `0`을 거짓으로, `0`을 제외한 모든 값을 참으로 취급합니다. 따라서 위의 예제에서 `1`을 `2`나 `(-5)`같은 다른 값으로 바꿔도 논리 연산자의 결과값은 같습니다.

`&&` 연산자는 `||`보다 우선 순위가 높습니다(`&&`는 논리**곱** 연산자). 그래서 `1 || 2 && 5`는 `1 || (2 && 5)`로 번역됩니다. `!` 연산자는 형변환 연산자나 `sizeof` 연산자처럼 산술 연산자보다 우선순위가 높습니다. 그래서 `!1 + 5`는 `(!1) + 5`가 되어 결과값이 `5`가 됩니다.

## 단락 평가(short-circuit evaluation)

> 이 강좌에서 `~의 결과값`은 `~ is evaluated as`를 번역한 것입니다.

이항 논리 연산자들은 항상 좌항의 결과값을 먼저 계산(evaluation)하고, 우항의 결과값을 계산합니다. 그런데 만약 논리합 연산자에서 좌항이 1이거나, 논리곱 연산자에서 좌항이 0이라면, 불 대수의 다음 성질에 의해, 우항을 계산할 필요가 없어집니다.

* `1 + a = 1`
* `0 × a = 0`

우항의 결과가 참이든 거짓이든 항상 결과값이 같기 때문입니다. C에선 성능을 위해 이런 경우 우항을 계산하지 않습니다. 이런 기능을 `단락 평가(short-circuit evaluation)`이라고 합니다. 그런데 단락 평가는 예상치 못한 결과를 냅니다.

```c
#include <stdio.h>

int main()
{
    int i = 3, j;
    j = 1 || (i = 5);
    printf("%d %d", i, j);
}
```
```
3 1
```

`(i = 5)`를 통해 `i`에 `5`를 집어넣었음에도 불구하고, 값을 출력하면 `3`이 표시됩니다. 이것은 논리합 연산자의 좌항이 참이었기 때문에, `(i = 5)`가 실행되지 않았기 때문입니다.

```c
#include <stdio.h>

int main()
{
    int i = 3, j;
    j = (1 == 2) && (i = 5);
    printf("%d %d", i, j);
}
```
```
3 0
```

논리곱 연산자에서도 마찬가지로 좌항의 결과가 거짓이라면 우항을 실제로 실행하지 않습니다.

## 비교 연산자(comparison operator)

`비교 연산자(comparison operator; relational operator)`는 두 수를 비교하여 결과가 **진리값**이 되는 이항 연산자들입니다. 총 여섯가지의 비교 연산자들이 있습니다.

> C++20에선 비교 연산자가 하나 더 추가되었습니다. 나중에 다룰 예정입니다.

```c
#include <stdio.h>

int main()
{
    printf("%d %d %d\n", 3 <  5, 4 <  4, 6 <  5);
    printf("%d %d %d\n", 3 <= 5, 4 <= 4, 6 <= 5);
    printf("%d %d %d\n", 3 >  5, 4 >  4, 6 >  5);
    printf("%d %d %d\n", 3 >= 5, 4 >= 4, 6 >= 5);
    printf("%d %d %d\n", 3 == 5, 4 == 4, 6 == 5);
    printf("%d %d %d\n", 3 != 5, 4 != 4, 6 != 5);
}
```
```
1 0 0
1 1 0
0 0 1
0 1 1
0 1 0
1 0 1

```

`<`, `>`, `<=`, `>=`는 모양에서 보시면 알 수 있겠지만 부등호에서 유래한 비교 연산자들입니다. 연산자에 따라 좌항이나 우항이 반대편보다 크거나, 또는 같으면 결과값이 1이, 아니면 0이 되는 것을 확인할 수 있습니다. `==`는 양변의 식의 결과값이 같으면 결과값이 1, 같지 않으면 0이 되는 연산자입니다. 반대로, `!=`는 양변의 식의 결과값이 같지 않으면 결과값이 1, 같으면 결과값이 0이 되는 연산자입니다.

비교 연산자는 산술 연산자보다 우선순위가 낮고, 논리 연산자보다 높습니다. 그래서 식 `1 + b < 3 * 5 && 6 != c / 4`는 `(((1 + b) < (3 * 5)) && (6 != (c / 4)))`로 번역됩니다.

## 복합문(compound statement)

`복합문(compound statement)`는 여러 문장을 하나로 묶은 문장입니다. 사용 방법도 간단합니다.
```
{
    [<문장 1>]
    [<문장 2>]
    [<문장 3>]
    ...
    [<문장 n>]
}
```

모양만 보면 `int main() { ... }`에서 `int main()`만 지운 것처럼 보입니다. 다음 예시를 봐주세요.

```c
#include <stdio.h>

int main() {
    { int i; }
    {
        int j = 5;
        printf("%d", j);
    }
    {
        int k = 6;
        {
            int l = 7;
            printf("%d", 7);
        }
    }
    {}
}
```

`int j = 5;`나 `printf("%d", j);`는 그 자체로 하나의 문장이지만, `{ int j = 5; printf("%d", j); }` 자체도 하나의 문장이 됩니다. 복합문은 문장으로 이루어져있기 때문에, 복합문 안에 복합문을 집어넣는 것도 가능합니다. `{}`같이 빈 복합문도 있을 수 있고, `{ int i; }`처럼 문장이 하나만 들어있는 복합문도 있을 수 있습니다. 배열의 초기화 식과 비슷하다는 것에 주의해주세요.

## if문(if statement)

`조건문(conditional statement)`은 결과값이 논리값인 식이 주어졌을 때, 그 식의 결과에 따라 다른 문장을 수행하는 문장입니다. C의 대표적인 조건문은 `if문(if statement)`입니다. if문은 다음과 같이 씁니다.

```
if (<조건식>) <조건식이 참일 때 실행할 문장> [else <조건식이 거짓일 때 실행할 문장>]
```

다음 예제를 봐주세요.

```c
#include <stdio.h>

int main()
{
    if (3 < 5)
        printf("3 is smaller than 5\n");
    else
        printf("3 is not smaller than 5\n");

    if (4 == 6)
        printf("4 is equal to 6\n");
    else
        printf("4 is not equal to 6\n");
}
```
```
3 is smaller than 5
4 is not equal to 6

```

`if (...)` 안에는 조건문이 들어가는데, 그 조건문의 결과가 결과가 참이면 `<조건식이 참일 때 실행할 문장>`이, 거짓이면 `<조건식이 거짓일 때 실행할 문장>`이 실행되는 것을 알 수 있습니다.

if문에는 복합문도 집어넣을 수 있습니다.

```c
#include <stdio.h>

int main() {
    int i; scanf("%d", &i);
    if (1 <= i && i <= 10) {
        printf("true: ");
        printf("i is between 1 and 10");
    }
    else {
        printf("false: ");
        printf("i is not between 1 and 10");
    }
}
```
```
: 5
true: i is between 1 and 10
```
```
: 12
false: i is not between 1 and 10
```

> `1 <= i && i <= 10`이라고 적은 것에 주목해주세요. `1 <= i`의 결과는 논리값입니다. `i <= 10`도 마찬가지이고요. `i`가 1과 10 사이에 있으면 1, 아니면 0이 되는 식을 만들고 싶으면 `1 <= i && i <= 10`이라고 적어야 합니다. `1 <= i <= 10`은 `(1 <= i) <= 10`으로 해석되기 때문에, `i`가 1 미만이라고 해도 `0 <= 10`, `i`가 10을 초과해도 `1 <= 10`이 되어 항상 참인 식이 됩니다. **비교 연산자의 결과는 논리값**이라는 것을 항상 기억해주세요.

만약 조건식이 거짓일 때 아무것도 실행하고 싶지 않다면 `else` 자체를 생략할 수 있습니다.

```c
#include <stdio.h>

int main()
{
    if (1)
    {
        printf("Foo\n");
    }
    printf("Bar\n");
}
```
```
Foo
Bar

```

> 식 `1`을 논리값으로 해석하면 참이기 때문에 if문의 조건식으로 `1`을 사용하면 `if () { ... }` 안의 문장들은 항상 실행됩니다.

if문은 그 자체로 문장이고, if문 안에는 문장이 들어가기 때문에 if문 여러개를 겹쳐 쓸 수도 있습니다.

```c
#include <stdio.h>

int main() {
    int i; scanf("%d", &i);
    if (i < 0)
        printf("i is smaller than 0");
    else {
        if (i <= 10)
            printf("i is between 0 and 10");
        else
            printf("i is bigger than 10");
    }
}
```
```
: -2
i is smaller than 0
```
```
: 7
i is between 0 and 10
```
```
: 15
i is bigger than 10
```

if-else문 자체는 하나의 문장이기 때문에, 위 예제는 다음과 같이 복합문 없이 쓸 수 있습니다.

```c
#include <stdio.h>

int main() {
    int i; scanf("%d", &i);
    if (i < 0)
        printf("i is smaller than 0");
    else if (i <= 10)
        printf("i is between 0 and 10");
    else
        printf("i is bigger than 10");
}
```

## 포인터 초기화하기

초기화되지 않은 변수는 정의되지 않은 행동을 유발할 수 있기 때문에, 반드시 0 등으로 초기화하는 것이 좋습니다. 지금까지의 예제에선 포인터에 이미 있는 변수나 배열의 주소를 대입하거나, 포인터를 초기화하지 않거나 둘 중 하나였는데요, 포인터도 0으로 초기화하는 것이 좋습니다.

```c
int main()
{
    int *i = 0;
}
```

포인터를 0으로 초기화하면 장점이 있는데요, if문 등에서 조건식에 포인터를 사용했을 때, 포인터가 가지고 있는 주소가 0이면 그 조건식이 거짓이 된다는 것입니다. 다음 예제를 봐주세요.

```c
#include <stdio.h>

int main()
{
    int *i = 0, j; scanf("%d", &j);

    if (j > 10) i = &j;

    if (i)
        printf("The pointer is pointing to some integer: %d", *i);
    else
        printf("The pointer is not pointing anywhere");
}
```
```
: 5
The pointer is not pointing anywhere
```
```
: 25
The pointer is pointing to some integer: 25
```

입력받은 값이 10 초과이면 포인터에 `j`의 주소를 대입하는 프로그램입니다. 맨 처음에 `0`으로 초기화되어있었기 때문에, 조건식으로 사용하면 `i`가 어떤 변수를 가리키는지, 안 가리키는지 알 수 있습니다.

> C++에서도 포인터에 0을 대입할 수는 있지만 0을 직접적으로 대입하는 것은 좋지 않은 습관입니다. C++에서 빈 포인터를 초기화하는 방법은 나중에 다루도록 하겠습니다.

## 조건 연산자(conditional operator)

[5장](../5-basic-operators)에서 연산자의 분류에 대해 설명했을 때, 피연산자가 세 개인 `삼항 연산자(ternary operator)`가 있다고 소개했습니다. 단항 연산자나 이항 연산자와 달리, C의 삼항 연산자는 딱 하나인데요, 바로 `조건 연산자(conditional operator)`입니다. 조건 연산자는 다음과 같이 씁니다.

```
<조건식> ? <조건식이 참일 때 결과값을 얻을 식> : <조건식이 거짓일 때 결과값을 얻을 식>
```

다음 예제를 봐주세요.

```c
#include <stdio.h>

int main()
{
    int i; scanf("%d", &i);
    int j = (i % 2 == 0) ? 20 : 10;
    printf("%d", j);
}
```
```
: 16
20
```
```
: 27
10
```

`i % 2 == 0`라는 식은 `i`가 짝수이면 참, 홀수이면 거짓이 되는 식입니다. 조건 연산자의 `<조건식>`에 이 식을 넣으니, `i`의 값에 따라 `j`가 `20` 또는 `10`이 되는 것을 확인할 수 있습니다.

조건 연산자도 if문과 마찬가지로, `<조건식이 참일 때 결과값을 얻을 식>`과 `<조건식이 거짓일 때 결과값을 얻을 식>` 중 한 쪽만 실행합니다. 다음 예제를 봐주세요.

```c
#include <stdio.h>

int main()
{
    int i = 5;
    int j = (1 != 2) ? 6 : (i = 7);
    printf("%d %d", i, j);
}
```
```
5 6
```

식 `(1 != 2)`가 참이기 때문에, `(i = 7)`은 실행되지 않습니다.

## while문(while statement)

컴퓨터의 가장 강력한 점 중 하나는 주어진 일을 아주 많은 횟수로 꾸준히 할 수 있다는 점입니다. 십만명 정도를 대상으로 하는 통계를 낸다고 했을 때, 사람이 하려고 하면 몇 천명도 되지 않아 지칠 것입니다. 하지만 컴퓨터는 십만명이 아니라 십억명이라고 해도 지치지 않고 통계에 필요한 계산을 수행할 수 있습니다. `반복문(loop)`은 이렇게 반복되는 작업을 수행하기 위해 사용되는 문장입니다.

while문은 C의 반복문 중 하나입니다. while문은 다음과 같이 씁니다.

```
while (<조건식>) <조건식이 참일 때 실행할 문장>
```

while문은

1. `<조건식>`의 결과값이 거짓이라면 문장을 종료하고, 참이라면 2로 갑니다.
2. `<조건식이 참일 때 실행할 문장>`을 실행합니다.
3. 1로 돌아갑니다.

다음 예제를 봐주세요.

```c
#include <stdio.h>

int main()
{
    int i; scanf("%d", &i);
    while (i > 0)
        printf("%d ", i--);
}
```
```
: 5
5 4 3 2 1 
```

`5 4 3 2 1 `이 출력된 과정은 다음과 같습니다.

1. 사용자한테서 `5`를 입력받아 `i`에 `5`가 들어갑니다.
2. `5 > 0`의 결과가 참이므로,
3. `printf("%d ", i--);`가 실행되어 `5 `가 출력되고 `i`가 `4`가 됩니다.
4. `4 > 0`의 결과가 참이므로,
5. `printf("%d ", i--);`가 실행되어 `4 `가 출력되고 `i`가 `3`이 됩니다.
6. `3 > 0`의 결과가 참이므로,
7. `printf("%d ", i--);`가 실행되어 `3 `이 출력되고 `i`가 `2`가 됩니다.
8. `2 > 0`의 결과가 참이므로,
9. `printf("%d ", i--);`가 실행되어 `2 `가 출력되고 `i`가 `1`이 됩니다.
10. `1 > 0`의 결과가 참이므로,
11. `printf("%d ", i--);`가 실행되어 `1 `이 출력되고 `i`가 `0`이 됩니다.
12. `0 > 0`의 결과가 거짓이므로, while문이 종료됩니다.

즉, while문은 `조건식 -> 실행 -> 조건식 -> 실행 -> ... -> 조건식 -> 종료`의 과정을 거칩니다.

## break문(break statement)

break문은 반복문 안에서 조건문의 결과에 상관없이 반복문을 종료할 수 있는 문장입니다. while문도 반복문이기 때문에 while문 안에서 break를 사용할 수 있습니다. break문은 다음과 같이 사용합니다.

```
break;
```

즉, break문은 식이 필요없는 문장입니다.

```c
#include <stdio.h>

int main()
{
    int i; scanf("%d", &i);
    while (i > 0)
    {
        printf("%d ", i);
        if (i == 6)
        {
            printf("foo");
            break;
        }
        i -= 1;
    }
}
```
```
: 5
5 4 3 2 1 
```
```
: 8
8 7 6 foo
```

8을 입력했을 때 `8 7 6 foo`가 출력된 과정은 다음과 같습니다.

1. 사용자한테서 `8`를 입력받아 `i`에 `8`이 들어갑니다.
2. `8 > 0`의 결과가 참이므로,
3. `printf("%d ", i);`가 실행되어 `8 `이 출력됩니다.
4. `8 == 6`의 결과가 거짓이므로, if문 안쪽이 실행되지 않습니다.
5. `i -= 1;`에 의해 `i`가 `7`이 됩니다.
6. `7 > 0`의 결과가 참이므로,
7. `printf("%d ", i);`가 실행되어 `7 `이 출력됩니다.
8. `7 == 6`의 결과가 거짓이므로, if문 안쪽이 실행되지 않습니다.
9. `i -= 1;`에 의해 `i`가 `6`이 됩니다.
10. `6 > 0`의 결과가 참이므로,
11. `printf("%d ", i);`가 실행되어 `6 `이 출력됩니다.
12. `6 == 6`의 결과가 참이므로,
13. `printf("foo");`가 실행되어 `foo`가 출력됩니다.
13. `break;`가 실행되어 while문이 종료됩니다.

이렇게 프로그래머가 원하는 시점에 반복문의 실행을 종료할 수 있습니다.

## do-while문(do-while statement)

do-while문도 C의 반복문 중 하나입니다. do-while문은 다음과 같이 씁니다.

```
do <실행할 문장> while (<조건식>);
```

> do-while문 끝에 붙어있는 세미콜론에 주의해주세요. 반드시 세미콜론이 붙어야 합니다.

do-while문은

1. `<실행할 문장>`을 실행합니다.
2. `<조건식>`의 결과값이 거짓이라면 문장을 종료하고, 참이라면 3으로 갑니다.
3. `<실행할 문장>`을 실행합니다.
4. `2`로 돌아갑니다.

즉, while문과 다르게 맨 처음이 조건문이 아니라, `실행 -> 조건식 -> 실행 -> ... -> 조건식 -> 종료`의 과정을 거칩니다.

```c
#include <stdio.h>

int main()
{
    int i; scanf("%d", &i);
    do
    {
        printf("%d ", i);
        i -= 1;
    } while (i > 5);
}
```
```
: 5
5 
```
```
: 8
8 7 6 
```

조건식이 `i > 5`임에도 불구하고 `5 `가 출력된 이유는 do-while문이 맨 처음에는 조건식의 결과값을 고려하지 않기 때문입니다.

do-while문도 반복문이기 때문에, break문을 사용하는 것이 가능합니다.

```c
#include <stdio.h>

int main()
{
    int i = 5;
    do
    {
        printf("%d", i);
        break;
    } while (i > 0);
}
```
```
5
```

## for문(for statement)

마지막으로 소개할 반복문은 for문입니다. for문은 사용하는 방법이 두 가지입니다.

```
for (<변수 선언문> [<조건식>]; [<반복식>]) <실행문>
```
```
for ([<초기식>]; [<조건식>]; [<반복식>]) <실행문>
```

for문은

1. `<변수 선언문>`이나 `<초기식>`을 실행합니다.
2. `<조건식>`의 결과값이 거짓이라면 문장을 종료하고, 참이라면 3으로 갑니다.
3. `<실행문>`을 실행합니다.
4. `<반복식>`을 실행합니다.
5. 2로 돌아갑니다.

즉, 양 쪽 다 다음과 같은 while문으로 치환할 수 있습니다.

```c
{
    <변수 선언문> | <초기식>;
    while (<조건식>)
    {
        <실행문>
        <반복식>;
    }
}
```

> 앞으로 문법 설명에서 `|`는 `또는`으로 해석해주세요.

예시를 보겠습니다.

```c
#include <stdio.h>

int main()
{
    for (int i = 0; i < 5; ++i)
        printf("%d ", i);
}
```
```
0 1 2 3 4 
```

이 예제에선 for문 안에서 변수 선언문을 사용했기 때문에, for문의 사용 방법 두 개 중 위의 문법이 적용됨을 알 수 있습니다. 더 쉬운 이해를 위해, 위 예제 속 for문을 while문으로 치환해보겠습니다.

```c
#include <stdio.h>

int main()
{
    {
        int i = 0;
        while (i < 5)
        {
            printf("%d ", i);
            ++i;
        }
    }
}
```

`0 1 2 3 4 `가 출력된 과정이 보이시나요?

1. `0 < 5`의 결과는 참이기 때문에,
2. `printf("%d ", i);`가 실행되어 `0 `이 출력됩니다.
3. `++i`가 실행되어 `i`가 `1`이 됩니다.
4. `1 < 5`의 결과는 참이기 때문에,
5. `printf("%d ", i);`가 실행되어 `1 `이 출력됩니다.
6. `++i`가 실행되어 `i`가 `2`가 됩니다.
7. `2 < 5`의 결과는 참이기 때문에,
8. `printf("%d ", i);`가 실행되어 `2 `가 출력됩니다.
9. `++i`가 실행되어 `i`가 `3`이 됩니다.
10. `3 < 5`의 결과는 참이기 때문에,
11. `printf("%d ", i);`가 실행되어 `3 `이 출력됩니다.
12. `++i`가 실행되어 `i`가 `4`가 됩니다.
13. `4 < 5`의 결과는 참이기 때문에,
14. `printf("%d ", i);`가 실행되어 `4 `가 출력됩니다.
15. `++i`가 실행되어 `i`가 `5`가 됩니다.
16. `5 < 5`의 결과는 거짓이기 때문에, while문이 종료됩니다.

동일한 프로그램을 변수 선언문 대신 초기식을 사용하여 만들어보겠습니다.

```c
#include <stdio.h>

int main()
{
    int i;
    for (i = 0; i < 5; ++i)
        printf("%d ", i);
}
```

이 예제 속 for문을 while문으로 치환하면 다음과 같이 변합니다.

```c
#include <stdio.h>

int main()
{
    int i;
    {
        i = 0;
        while (i < 5)
        {
            printf("%d ", i);
            ++i;
        }
    }
}
```

위 예제와 다르게 `int i;`가 위로 올라가긴 했지만, 실행엔 별로 차이가 없을 것입니다. 동일하게 `0 1 2 3 4 `를 출력한다는 것을 알 수 있습니다.

for문도 반복문이기 때문에 break문을 사용할 수 있습니다.

```c
#include <stdio.h>

int main()
{
    for (int i = 5; i >= 0; --i)
    {
        if (i == 2)
            break;
        printf("%d ", i);
    }
}
```
```
5 4 3 
```

위 예제의 for문을 while문으로 치환해보겠습니다.

```c
#include <stdio.h>

int main()
{
    {
        int i = 5;
        while (i >= 0)
        {
            if (i == 2)
                break;
            printf("%d ", i);
            --i;
        }
    }
}
```

`5 4 3 `이 왜 출력되는지 감이 잡히시나요? for문은 복잡하지만 아주 많이 사용되기 때문에, 분석하기 힘들실 땐 while문으로 치환해보시면서 for문에 익숙해지려고 노력해주세요.

for문은 배열과 함께 사용될 때 가장 강력합니다. [6장](../6-pointers-and-array)에서 `sizeof` 연산자로 배열의 길이를 구하는 방법을 배웠습니다. 이걸 for문과 함께 사용하면 배열의 모든 요소를 출력하는 코드를 쉽게 만들 수 있습니다.

```c
#include <stdio.h>

int main()
{
    int arr[] = { 1, 1, 2, 3, 5, 8, 13, };
    printf("int arr[] = { ");
    for (int i = 0; i < sizeof arr / sizeof(int); ++i)
        printf("%d, ", arr[i]);
    printf("};");
}
```
```
int arr[] = { 1, 1, 2, 3, 5, 8, 13, };
```

사용자의 입력을 배열에 입력받을 때도 for문을 활용할 수 있습니다.

```c
#include <stdio.h>

int main()
{
    int arr[5];
    int len = sizeof arr / sizeof(int);
    for (int i = len - 1; i >= 0; --i)
        scanf("%d", arr + i);

    for (int i = 0; i < len; ++i)
        printf("%d ", arr[i] * 2);
}
```
```
: 18 62 17 26 20
40 52 34 124 36
```
```
: 18
: 62 17
: 26 20
40 52 34 124 36
```

> 위 예제에선 `arr[4]`, `arr[3]`, `arr[2]`, ... 순서대로 사용자의 입력을 받고 있습니다.

> `arr[i]`가 `*(arr + i)`랑 동치라는 것을 기억해주세요. `&arr[i]`는 `arr + i`가 됩니다.

> [6장](../6-pointers-and-array)에서 `scanf`가 사용자의 입력을 스페이스나 줄바꿈으로 구분한다고 언급한 것을 기억해주세요.

for문의 초기식, 조건식, 반복식은 모두 생략이 가능합니다. 초기식이나 반복식이 생략되는 것은 쉽게 이해할 수 있습니다. `for (; <조건식>;) <실행문>`처럼 쓰면 `while (<조건식>) <실행문>`이 되는 것과 큰 차이가 없습니다.

```c
for ([초기식]; <조건식>; [반복식]) <실행문> 

->

{
    [초기식];
    while (<조건식>)
    {
        <실행문>
        [반복식];
    }
}
```

여기서 `[초기식]`과 `[반복식]`만 생략하면 되니까요. 변수 선언문이 포함된 경우도 마찬가지입니다.

```c
for (<변수 선언문> <조건식>; [반복식]) <실행문>

->

{
    <변수 선언문>
    while (<조건식>)
    {
        <실행문>
        [반복식];
    }
}
```

근데 여기서 조건식을 생략해버리면 while문 안에 조건식이 들어있지 않게 되어 문법 오류가 발생합니다. 그래서 조건식이 생략되면 while문으로 치환했을 때 **조건식에 `1`이 들어있는 것**으로 생각합니다. 예시를 보겠습니다.

```c
#include <stdio.h>

int main()
{
    for (int i = 0;;i += 2)
    {
        printf("%d ", i);
    }
}
```

이 예제를 while문으로 치환하면 다음과 같이 쓸 수 있습니다.

```c
#include <stdio.h>

int main()
{
    {
        int i = 0;
        while (1)
        {
            printf("%d ", i);
            i += 2;
        }
    }
}
```

이 프로그램을 `Ctrl + F5`를 눌러 실행해보면

```
0 2 4 6 8 10 12 14 16 18 20 ...
```

프로그램이 멈추지 않고 끝나는 것을 확인할 수 있습니다.

## 무한 루프(infinite loop)

반복문은 조건식이 참이면 주어진 문장을 계속 수행합니다. 조건식으로 `1`이나 `2`이나, `20 < 100`처럼 결정값이 항상 참으로 간주되는 값인 식을 사용하면 반복문은 영원히 반복되게 됩니다. 이런 반복문을 `무한 루프(infinite loop)`라고 부릅니다. 방금 다룬 예제도 무한 루프를 사용한 프로그램입니다.

프로그램의 종류에 따라 무한 루프를 사용해야하는 경우도 있지만, 대부분의 프로그램의 경우엔 무한 루프를 사용할 일이 없습니다. `무한 루프에 갇히다`라는 표현이 있을 정도로 무한 루프가 실행 중인 상황은 대게 버그가 발생한 경우입니다. 예를 들어, 1부터 10까지의 정수를 거꾸로 출력하는 프로그램을 만들어보려고 합니다.

```c
#include <stdio.h>

int main()
{
    for (int i = 10; i >= 1; --i)
        printf("%d ", i);
}
```

그런데, 원래 `--i`라고 써야 하는 걸 `++i`라고 써버렸습니다. `i >= 1`은 항상 `1`이 됩니다. 반복문의 조건식이 항상 참이니 무한 루프가 됩니다. 이처럼 무한 루프가 발생한 경우엔 보통 좋지 않은 일이 일어났을 가능성이 높습니다. 그래서 조건식에 항상 참이 되는 식을 넣지는 않았는지 확인하고, 혹시 `1`을 꼭 넣어야 한다면 다음 예제처럼 break문을 적절히 사용할 필요가 있습니다.

```c
#include <stdio.h>

int main()
{
    printf("Summation calculator\n");
    int sum = 0;
    while (1)
    {
        printf("Gimme a number: ");
        int i; scanf("%d", &i);
        if (!i)
            break;
        sum += i;
    }
    printf("Sum: %d", sum);
}
```
```
Summation calculator
Gimme a number: 7
Gimme a number: 18
Gimme a number: 25
Gimme a number: 16
Gimme a number: 19
Gimme a number: -34
Gimme a number: 14
Gimme a number: 0
Sum: 65
```

> 위 예제처럼 break문이 실행될 수 있는 가능성이 충분히 있으면 조건식이 `1`이라고 하더라도 무한 루프라고 부르지 않습니다.

> 식 `!i`에 주목해주세요. 부정 연산자는 참인 값을 거짓으로, 거짓인 값을 참으로 바꿔줍니다. 즉, `0`이 아닌 식은 `0`으로, `0`인 식은 `1`로 바꿔주는 연산자입니다. 그래서 `i`가 `0`이 아니면 조건식의 결과값이 `0`이 되어 if문 안쪽이 실행되지 않고, `i`가 `0`이면 조건식의 결과값이 `1`이 되어 if문 안쪽이 실행됩니다. 이렇게 사용자가 `0`을 입력하면 반복문을 종료하게 만들 수 있습니다.

[다음: 함수](../8-functions)