## Дополнительные материалы

1\. Дополнительные [задачи на условный оператор из курса по С++ от Яндекса](https://stepik.org/lesson/13022/step/3)

2\. Используя каскад из инструкций `if-else` можно заменить любую инструкцию `switch-case`. Вот пример, основанный на задаче из урока про `switch-case`:

```c
switch (answer) { 
        case 'B': 
        case 'b': 
                printf("GOOD!\n"); 
                break;
        case 'A': 
        case 'a': 
        case 'C': 
        case 'c': 
        case 'D': 
        case 'd': 
                printf("BAD!\n");
                break;
        default: 
                printf("ERROR!\n"); 
                break;
}

// аналог на if-else
if (answer == 'B' || answer == 'b') {
        printf("GOOD!\n");
} else if (answer == 'A' || answer == 'B' || answer == 'C' ||
           answer == 'a' || answer == 'b' || answer == 'c') {
        printf("BAD!\n");
} else {
        printf("ERROR!\n");
}
```

И хотя мы можем переписать любой `switch-case` с помощью `if-else`, но так делать не рекомендуется. Во-первых, это выглядит менее красиво, во-вторых, это стрельба из пушки по воробьям.