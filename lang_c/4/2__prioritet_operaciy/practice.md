## Практика

- Решите [задачи с автоматической проверкой решения на Stepik](https://stepik.org/lesson/41457/step/1)

<div class="lessonStepikBlock">
    <iframe src="https://stepik.org/lesson/41457/step/1"></iframe>
</div>


- Иногда из-за особенностей выполнения арифметических действий и преобразований некоторые математические истины "перестают работать". Разберитесь, почему следующие программы выдают различные результаты:

```c
#include <stdio.h>

int main(void)
{

        int a = 8, b = 16, c = 2;

        int res_1 = a / b * c;
        int res_2 = a * c / b;

        printf("1: %d\n2: %d\n", res_1, res_2);
        return 0;
}
```

```c
#include <stdio.h>

int main(void)
{

        int a = 5, b = 7, c = 9;

        double res_1 = (double) c / (b / a);
        double res_2 = c / ((double) b / a);

        printf("1: %.2f\n2: %.2f\n", res_1, res_2);
        return 0;
}
```
Ваши объяснения этих "парадоксов" пишите в комментариях к этому уроку.