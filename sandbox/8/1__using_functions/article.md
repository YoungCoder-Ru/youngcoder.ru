# Практическое применение функций в языке Си

В этом заметке на примере программы «Игральный кубик» из прошлых уроков я бы хотел проиллюстрировать некоторые возможности использования функций. 

Вспомним нашу программу  «Игральный кубик» v.3.0 из урока про циклы
Листинг 1. Программа  «Игральный кубик» v.3.0
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void)
{
        srand(time(NULL));

        printf("#########  Devil\'s bones   #########\n");
        printf("#                                   #\n");
        printf("#   Commands:                       #\n");
        printf("#                                   #\n");
        printf("#   1 - new game                    #\n");
        printf("#   0 - exit                        #\n");
        printf("#                                   #\n");
        printf("#####################################\n\n");

        int control;
        int value = 0, 
            total_runs = 0,     // количество бросков
            sum_results = 0;    // сумма очков за всю игру
        
        do {
                printf("Enter command: ");
                scanf("%d", &control);
                
                switch (control) {
                        case 0: 
                                printf("\nGood bye!\n");
                                printf("Total number of die rolls: %d\n", total_runs);
                                printf("Sum of all die rolls: %d\n", sum_results);
                                break;
                        case 1:
                                value = 1 + rand() % 6;
                                printf("Result: %d\n", value);

                                // увеличиваем кол-во бросков на 1
                                total_runs += 1;        

                                // добавляем значение к сумме очков
                                sum_results += value; 
                                break;
                        default:
                                printf("Error! Try again...\n");
                                break;
                }

        } while (control != 0); 

        return 0;
}
```
Теперь давайте проведём небольшой =рефакторинг= этого кода, т.е. поработаем над его структурой. 

Для начала хорошо бы выделить частик кода, которые можно вынести в отдельные функции. Подобную операцию иногда называют =декомпозицей=, т.е. разделением одной большой программы на ряд небольших вспомогательных программ. Правило "Divide and conquer" работает и в программировании.

Во-первых, сразу бросаются в два куска кода, где есть последовательные вызововы функции `printf`:
1. вывод приветственный экрана;
2. вывод статистики игры;

Давайте вынесем их в отдельные функции, для этого нам нужно придумать названия для этих функций, посмотреть, есть ли какие-то параметры от которых они зависят и должны ли они вычислять какое-нибудь значение.

Ну, с именами всё просто: `show_intro_screen` и `show_exit_screen`. С возвращаемыми значениями тоже, ни та, ни другая функции ничего вычислять не должны, а должны просто выводить информацию на экран, значит это будут =void-функции=, иногда так называют функции, которые не возвращают никакого значения. Что же касается формальных параметров, то у функции `show_intro_screen` их не будет, т.к. она выводит на экран захардкоженное сообщение, которое ни от чего не зависит. А вот функция `show_exit_screen` должна выводить на экран статистику игры: общее количество бросков и сумму выпавших очков, т.е. она зависит от двух параметров. Оба параметра целые числа. 

Исходя из вышесказанного, запишем прототипы получившихся функций:
Листинг 2.
```c
void show_intro_screen(void);
void show_exit_screen(int runs, int summ);
```

Исходя из этих прототипов опишем реализации функций `show_intro_screen` и `show_exit_screen`.

Листинг 3.
```c
void show_intro_screen(void)
{
        printf("#########  Devil\'s bones   #########\n"
               "#                                   #\n"
               "#   Commands:                       #\n"
               "#                                   #\n"
               "#   1 - new game                    #\n"
               "#   0 - exit                        #\n"
               "#                                   #\n"
               "#####################################\n\n");
}


void show_exit_screen(int runs, int sum)
{
        printf("\nGood bye!\n");
    
        printf("Total number of die rolls: %d\n", runs);
        printf("Sum of all die rolls: %d\n", sum);
}
```

Тут ничего особо сложно. Я буквально просто скопировал код и внёс небольшие косметические изменения. В функции `show_intro_screen` я использовал подход, который называется string literal concatenation. Я описывал его в дополнительных материалах к уроку про `printf`.

В функции `show_exit_screen` пришлось заменил имена локальных переменных на те, что использованы в заголовке функции, хотя это можно было и не делать.

Давайте объединим наши наработки из Листингов 2 -- 4 в единый код.

Листинг 5.  «Игральный кубик» v.3.1
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void show_intro_screen(void);
void show_exit_screen(int runs, int sum);

int main(void)
{
        srand(time(NULL));

        show_intro_screen();

        int control;
        int value = 0, 
            total_runs = 0,     // количество бросков
            sum_results = 0;    // сумма очков за всю игру
        
        do {
                printf("Enter command: ");
                scanf("%d", &control);
                
                switch (control) {
                        case 0: 
                                show_exit_screen(total_runs, sum_results);
                                break;
                        case 1:
                                value = 1 + rand() % 6;
                                printf("Result: %d\n", value);

                                total_runs += 1;        
                                sum_results += value; 
                                break;
                        default:
                                printf("Error! Try again...\n");
                                break;
                }

        } while (control != 0); 

        return 0;
}


void show_intro_screen(void)
{
        printf("#########  Devil\'s bones   #########\n"
               "#                                   #\n"
               "#   Commands:                       #\n"
               "#                                   #\n"
               "#   1 - new game                    #\n"
               "#   0 - exit                        #\n"
               "#                                   #\n"
               "#####################################\n\n");
}


void show_exit_screen(int runs, int sum)
{
        printf("\nGood bye!\n");
    
        printf("Total number of die rolls: %d\n", runs);
        printf("Sum of all die rolls: %d\n", sum);
}
```

Главная функций `main` стала гораздо чище.

Теперь давайте вынесем в отдельную функцию вычисление значения, выпавшего на кубике. Сейчас это вычисляется в строке `value = 1 + rand() % 6;`.

Назовём функцию `generate_dice_value`. В качество выходного значения она должна возвращать целое число -- количество очков, выброшенное на кубике. А что со входными параметрами? В принципе, можно оставить `void`, но я предлагаю добавить один целочисленный параметр -- количество граней у кубика, так мы сделаем код программы чуть более универсальным и расширяемым.

Листинг 6. Определение функции `generate_dice_value`
```c
int generate_dice_value(int d_size)
{
        return 1 + rand() % d_size;
}
```

Можно ли было убрать в функцию `generate_dice_value` строку 
`printf("Result: %d\n", value);`? В принципе, конечно, так можно сделать. Но, на мой вкус, не желательно.

Во-первых, тогда у функции `generate_dice_value` появляется побочный эффект. А зачем лишний раз портить функцию побочным эффектом? Незачем! 

Во-вторых, чтобы не нарушать следующую базовую рекомендацию:
% **Рекомендация:** функция должна делать решать одну конкретную задачу. И желательно, решать её очень хорошо.

Если бы я добавил в эту функцию ещё и вывод строки на экран, то это уже было бы два действия. А когда функцию выполняет два действия, то ей сложно придумать хорошее имя. Не называть же её `generate_dice_value_and_print_result`? 

Сейчас у кого-нибудь появился вопрос, а почему бы тогда не декомпозировать функцию `show_exit_screen` ещё на две функции, например: `say_goodbye` и `show_game_statistics`, а `show_game_statistics` в свою очередь декомпозировать ещё на две функции? 

В принципе, так сделать можно, никто не запрещает. Но это уже будет не элегантно, на мой взгляд, поэтому я так делать не стал. Можно ведь буквально каждую простую инструкцию обернуть в функцию, но это будет чересчур, так скажем =избыточная декомпозиция=. А как же рекомендация, спросите вы? А я отвечу: ~~кодекс -- это всего лишь свод указаний, а не жестких законов~~ это всего лишь рекомендация! 

% **Важно!** нет никаких золотых стандартов и правил, как правильно разбивать программу на подпрограммы. Единственный вариант научиться правильно это делать -- постоянно тренироваться, держа в уме главную цель декомпозиции: упростить код!

Дополнительно, я создал отдельную переменную `D_SIZE` в функции `main` в которой будет хранится количество граней на нашей игральной кости. 

Листинг 6. Программа «Игральный кубик» v.3.2
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void show_intro_screen(void);
void show_exit_screen(int runs, int sum);

int generate_dice_value(int d_size);

int main(void)
{
        srand(time(NULL));

        show_intro_screen();

        int control;
        int value = 0,
            D_SIZE = 6,         // количество граней у кубика
            total_runs = 0,     // количество бросков
            sum_results = 0;    // сумма очков за всю игру
        
        do {
                printf("Enter command: ");
                scanf("%d", &control);
                
                switch (control) {
                        case 0: 
                                show_exit_screen(total_runs, sum_results);
                                break;
                        case 1:
                                value = generate_dice_value(D_SIZE);
                                printf("Result: %d\n", value);

                                total_runs += 1;        
                                sum_results += value;
                                break;
                        default:
                                printf("Error! Try again...\n");
                                break;
                }

        } while (control != 0); 

        return 0;
}


int generate_dice_value(int d_size)
{
        return 1 + rand() % d_size;
}


void show_intro_screen(void)
{
        printf("#########  Devil\'s bones   #########\n"
               "#                                   #\n"
               "#   Commands:                       #\n"
               "#                                   #\n"
               "#   1 - new game                    #\n"
               "#   0 - exit                        #\n"
               "#                                   #\n"
               "#####################################\n\n");
}


void show_exit_screen(int runs, int sum)
{
        printf("\nGood bye!\n");
    
        printf("Total number of die rolls: %d\n", runs);
        printf("Sum of all die rolls: %d\n", sum);
}
```

В принципе, пока можно и остановиться. Можно, а зачем? 

Пока есть возможность, я хочу рассказать вам ещё пару штук.

## Глобальные переменные 

Все переменные, которые мы объявляет внутри функций являются локальными. Они видны только в пределах той функции в которой они объявлены. Но, вообще говоря, мы могли бы объявить переменную вне любых функций, т.е. даже вне функции `main`. Такие переменные называются =глобальными переменными=.

У них две основные особенности:
1. глобальным переменным присваивается значение `0` по умолчанию, если они не инициализированы каким-либо другим значением.
2. глобальные переменные доступны в любой функции и это, с одной стороны удобно, а с другой довольно опасно.

Давайте напишем очередную версию программы  «Игральный кубик», но с использование глобальных переменных. Глобальными сделаем переменные `D_SIZE`, `total_runs` и `sum_results`.

Листинг 7: Программа «Игральный кубик» v.3.3
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void show_intro_screen(void);
void show_exit_screen(void);

int generate_dice_value(void);

int D_SIZE = 6,     // количество граней у кубика
    total_runs,     // количество бросков
    sum_results;    // сумма очков за всю игру, 

int main(void)
{
        srand(time(NULL));

        show_intro_screen();

        int control;
        int value = 0;
        
        do {
                printf("Enter command: ");
                scanf("%d", &control);
                
                switch (control) {
                        case 0: 
                                show_exit_screen();
                                break;
                        case 1:
                                value = generate_dice_value();
                                printf("Result: %d\n", value);

                                total_runs += 1;        
                                sum_results += value;
                                break;
                        default:
                                printf("Error! Try again...\n");
                                break;
                }

        } while (control != 0); 

        return 0;
}


int generate_dice_value(void)
{
        return 1 + rand() % D_SIZE;
}


void show_intro_screen(void)
{
        printf("#########  Devil\'s bones   #########\n"
               "#                                   #\n"
               "#   Commands:                       #\n"
               "#                                   #\n"
               "#   1 - new game                    #\n"
               "#   0 - exit                        #\n"
               "#                                   #\n"
               "#####################################\n\n");
}


void show_exit_screen(void)
{
        printf("\nGood bye!\n");
    
        printf("Total number of die rolls: %d\n", total_runs);
        printf("Sum of all die rolls: %d\n", sum_results);
}
```

Что изменилось? 
Во-первых, в функции больше не надо передавать параметры. Во всех прототипах вместо списка параметров указано ключевое слово `void`. Передавать параметры между функциям больше не нужно, т.к. все эти переменные являются глобальными и доступны всем функциям в этом файле.

Во-вторых, т.к. глобальные переменные по умолчанию инициализируются нулевыми значениями, а не мусором, как локальные переменные, то при объявлении переменных `total_runs` и `sum_results` опущена инициализация нулями.

При этом, теперь из заголовков функций непонятно, какие переменные в них используются, необходимо читать полное описание функций. С другой стороны, если мы в какой-нибудь из функций случайно поменяем значение глобальной переменной, то это изменение затронет все функции. Это, конечно, не очень приятно.

Есть ли способ защитить значение переменной от изменения внутри программы? Да, такой способ есть.

## Квалификатор `const`

Чтобы защитить переменную от изменения, ей можно добавить =квалификатор типа `const`=. Ключевое словое `const` нужно записать перед типом переменной. Компилятор, когда увидит, что у какой-то переменной добавлен квалификатор `const` начнёт отслеживать, чтобы никто не попытался что-нибудь записать записать в эту переменную. Если заметит такую попытку на этапе компиляции, то сразу же выдаст ошибку.

Вот простой пример:

Листинг 8: Пример использование квалификатора `const`
```c
int main(void)
{
    const int players = 2;
    players = 3;   //  ОШИБКА! Пытаемся изменить переменную с квалификатором const
    
    return 0;
}
```

Обратите внимание, что `const` можно использовать не только с глобальными, но и с локальными переменными. В том числе и с переменными из параметров функций.

Что ж, давайте защитим нашу переменную `D_SIZE` от случайного изменения какими-либо функциями.

Листинг 9: Программа «Игральный кубик» v.3.4 с защитой глобальной переменной `D_SIZE`
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void show_intro_screen(void);
void show_exit_screen(void);

int generate_dice_value(void);


const int D_SIZE = 6;     // количество граней у кубика
int total_runs,     // количество бросков
    sum_results;    // сумма очков за всю игру

int main(void)
{
        srand(time(NULL));

        show_intro_screen();

        int control;
        int value = 0;
        
        do {
                printf("Enter command: ");
                scanf("%d", &control);
                
                switch (control) {
                        case 0: 
                                show_exit_screen();
                                break;
                        case 1:
                                value = generate_dice_value();
                                printf("Result: %d\n", value);

                                total_runs += 1;        
                                sum_results += value;
                                break;
                        default:
                                printf("Error! Try again...\n");
                                break;
                }

        } while (control != 0); 

        return 0;
}


int generate_dice_value(void)
{
        return 1 + rand() % D_SIZE;
}


void show_intro_screen(void)
{
        printf("#########  Devil\'s bones   #########\n"
               "#                                   #\n"
               "#   Commands:                       #\n"
               "#                                   #\n"
               "#   1 - new game                    #\n"
               "#   0 - exit                        #\n"
               "#                                   #\n"
               "#####################################\n\n");
}


void show_exit_screen(void)
{
        printf("\nGood bye!\n");
    
        printf("Total number of die rolls: %d\n", total_runs);
        printf("Sum of all die rolls: %d\n", sum_results);
}
```

Теперь переменная `D_SIZE` под защитой квалификатора `const`.

% **ВАЖНО!** Старайтесь не использовать глобальные переменные без крайней необходимости. А если используете, то будьте предельно аккуратны.

Можно ли оставить программу в таком виде? Формально -- можно, ведь всё работает. Но мне текущий вариант как-то не нравится. Не хочется мне связывать с глобальными переменными. Рассказал о них только для того, чтобы вы были в курсе, что такой механизм имеется.

Поэтому на основе Листинга 6 я сделаю новую версию программы: без глобальных переменных, но с квалификатором `const` для защиты переменной `D_SIZE`. Кроме того, я изменю названия некоторых переменных на более удачные варианты.


Листинг 10: Программа «Игральный кубик» v.3.5 
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void show_intro_screen(void);
void show_exit_screen(int runs, int sum);

int generate_dice_value(int dice_size);

int main(void)
{       
        const int DICE_SIZE = 6;   // количество граней у кубика      
        int total_runs = 0,     // количество бросков
            sum_results = 0,    // сумма очков за всю игру
            cmd,                // текущая команда 
            value;              // текущее значение на кубике

        srand(time(NULL));
        show_intro_screen();

        do {
                printf("Enter command: ");
                scanf("%d", &cmd);
                
                switch (cmd) {
                        case 0: 
                                show_exit_screen(total_runs, sum_results);
                                break;
                        
                        case 1:         
                                value = generate_dice_value(DICE_SIZE);
                                printf("Result: %d\n", value);

                                total_runs += 1;        
                                sum_results += value;
                                break;
                        
                        default:
                                printf("Error! Try again...\n");
                                break;
                }

        } while (cmd != 0); 

        return 0;
}


int generate_dice_value(int dice_size)
{
        return 1 + rand() % dice_size;
}


void show_intro_screen(void)
{
        printf("#########  Devil\'s bones   #########\n"
               "#                                   #\n"
               "#   Commands:                       #\n"
               "#                                   #\n"
               "#   1 - new game                    #\n"
               "#   0 - exit                        #\n"
               "#                                   #\n"
               "#####################################\n\n");
}


void show_exit_screen(int runs, int sum)
{
        printf("\nGood bye!\n");
    
        printf("Total number of die rolls: %d\n", runs);
        printf("Sum of all die rolls: %d\n", sum);
}
```

Конечно, здесь есть ещё над чем поработать, но давайте пока остановимся и подведём промежуточные итоги.

Сразу заметно, что код стал объёмнее (количество строк увеличилось) Это, конечно, не очень приятно. 

НО теперь код имеет некоторое подобие внутренней структуры. Он написан не одним сплошным куском, а состоит из отдельных небольших функций, каждая из которых решает свою локальную небольшую задачу. Две из них можно назвать "интерфейсными", а одну "игровой". 
И хотя общий объём кода вырос, каждый отдельный кусочек кода довольно прост и понятен. А большинство второстепенных деталей убрано из главной функции `main`.

Во-вторых, эта программа более универсальна, а её код будет легче поддерживать и расширять, например, если мы захотим добавить использование нескольких кубиков, включая "кубики" с различным количеством граней.

Напоследок ещё раз напоминаю, **не пренебрегайте декомпозицией даже в ваших небольших учебных программах, но избегайте бестолковой декомпозиции**. Побольше практикуйтесь, ведь практика -- путь к совершеству!