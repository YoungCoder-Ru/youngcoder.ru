# Символьные строки

Начнём, как обычно, с задачи.

> Различные буквы встречаются в текстах с разной частотой. Например, буква О встречается гораздо чаще, чем буква Х. На этом факте основан один из способов взлома простых шифров –- частотный криптоанализ. Чтобы им воспользоваться, необходимо посчитать частоту вхождения каждого из символов в зашифрованном тексте.

Первая сложность: Нужно хранить минимум 26 чисел (для английского алфавита).

Вторая сложность: Один символ мы хранить умеем. Для этого используется тип `char`. А вот как сохранить целое предложение? Для этого используются символьные массивы.

В этом уроке поближе познакомимся с типом `char` и изучим отдельный вид массивов -- символьные строки. Кроме того, попутно изучим несколько полезных функций для работы со строками.