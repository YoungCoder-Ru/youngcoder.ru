## Дополнительные материалы

1\. В уроке про типы данных я упоминал, что тип `double` называют типом =двойной точности (double precision)=, и что он в некотором смысле лучше, чем тип `float` . Чтобы увидеть разницу, запустите следующую программу:

```c
#include <stdio.h>

int main(void)
{
        int  int_result = 1/10;
        float float_result = 1.0/10.0;
        double double_result = 1.0/10.0;

        printf("Type: int:\n%d\n\n", int_result);

        printf("Type: float\n");
        printf("precision 4\t-- %.4f\n", float_result);
        printf("precision 12\t-- %.12f\n", float_result);
        printf("precisson 16\t-- %.16f\n", float_result);
        
        printf("\n=====================================\n");

        printf("Type: double\n");
        printf("precision 4\t-- %.4f\n", double_result);
        printf("precision 12\t-- %.12f\n", double_result);
        printf("precisson 16\t-- %.16f\n", double_result);
       

        return(0);
}
```

Причина подобного результата в том, что количество памяти выделенное для хранения любой переменной ограничено. Поэтому все вещественные числа хранятся в памяти компьютера приближенно. Для типа `float` памяти выделяется обычно меньше, чем для типа `double`. Поэтому возникают такие забавные моменты. Отдельно обратите внимание на результат деления целых чисел, записанный в переменную `z`. Об этом мы поговорим в четвёртом уроке.


2\. Функция `printf` в результате своей работы возвращает целое число (тип `int`) -- количество выведенных на экран символов.

Используя эту информацию, давайте проверим, что escape-последовательности, хотя и записываются в формат строке несколькими символами, всё же при выводе считаются за один символ.

Строка `Hello, World!` состоит из `13` символов, а в строке `Hello, World!\n` формально `15` символов. 

Скомпилируйте и запустите следующую программу:

```c
#include <stdio.h>

int main(void)
{
        int count = 0;

        // присваиваем возвращаемое функцией printf значение переменной count
        count = printf("Hello, World!\n"); 
        
        printf("%d\n", count); // выводим значение count на экран
        
        return 0;
}
```
Если наша гипотеза верна, то последовательность `\n` будет считаться за один символ и программа выведете на экран значение `14` (`13` обычных символов и `1` управляющий символ);

3\. Иногда строка форматирования может получиться настолько длинной, что не умещается на экране без горизонтальной прокрутки. Есть два способа, как можно это исправить.

Первый способ (в лоб) заключается в том, чтобы разбить вызов функции `printf` на несколько отдельных вызовов.

Например, так сделано в Листинге 9 для заголовка таблицы:
```c
printf(" YoungCoder.Ru Course Statistics \n");
printf("---------------------------------\n");
```

Второй способ основан на знании того, как стандарт языка Си предписывает компиляторам обрабатаывать несколько подряд идущих строк (строковых литералов). Если вдруг компилятор встречает несколько строк, которые разделены только пробелами, табуляциями и/или переносами, то он объединяет в одну строку.

Таким образом, эти две вызова функции можно было бы записать используя лишь один вызов `printf`:
```c
printf(" YoungCoder.Ru Course Statistics \n"
       "---------------------------------\n");
```

Это фича называется =string literal concatenation=. 

Чтобы проверить, что это работает, скомпилируйте и запустите следующую программу:
```c
#include <stdio.h>

int main(void)
{       
        // слишком длинная строка
        printf(" YoungCoder.Ru Course Statistics \n---------------------------------\n\n");

        // первый способ
        printf(" YoungCoder.Ru Course Statistics \n");
        printf("---------------------------------\n\n");

        // второй способ
        printf(" YoungCoder.Ru Course Statistics \n"
               "---------------------------------\n\n");
        
        return 0;
}
```

4\. Обратите внимание на следующий любопытный факт. У функции `printf` нет строго фиксированного количества параметров. Их может быть один, два, двадцать. Такие функции называются =функциями с переменными количеством параметров=. В базовой части курса мы не будем изучать, как можно самому создавать функции с переменным количеством параметров.