## Практика

- Решите [задачи с автоматической проверкой решения на Stepik](https://stepik.org/lesson/41090/step/1)

<div class="lessonStepikBlock">
    <iframe src="https://stepik.org/lesson/41090/step/1"></iframe>
</div>

### Исследовательские задачи для хакеров

1\. Разберитесь, почему следующая программа работает некорректно.

```c
#include <stdio.h>
#include <stdlib.h> // чтобы работала функция abs()

int main(void)
{              
        int a = -12345;
        int b = -2147483647;
        int x = -2147483648;
        int o = -2147483649;
        
        printf("abs(%d) = %d\n", a, abs(a));
        printf("abs(%d) = %d\n", b, abs(b));
        printf("abs(%d) = %d\n", x, abs(x));
        printf("abs(%d) = %d\n", o, abs(o));

        return 0;
}
```