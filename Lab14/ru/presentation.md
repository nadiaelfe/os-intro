---
## Front matter
## Front matter
lang: ru-RU
title: Презентация по лабораторной работе № 14
author: |
	Эззакат Надиа 
institute: |
	Российский Университет Дужбы Народов
date: Москва, 2021


## Formatting
toc: false
slide_level: 2
theme: metropolis
header-includes: 
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true
---

# Цель работы

Приобрести простейшие навыки разработки, анализа, тестирования и отладки приложений в ОС типа UNIX/Linux на примере создания на языке программирования С калькулятора с простейшими функциями.

# Задание

1. В домашнем каталоге создайте подкаталог ~/work/os/lab_prog.

2. Создайте в нём файлы: calculate.h, calculate.c, main.c. Это будет примитивнейший калькулятор, способный складывать, вычитать, умножать и делить, возводить число в степень, брать квадратный корень, вычислять sin, cos, tan. При запуске он будет запрашивать первое число, операцию, второе число. После этого программа выведет результат и остановится.
Реализация функций калькулятора в файле calculate.h:
////////////////////////////////////
// calculate.c
 #include <stdio.h>
 #include <math.h>
 #include <string.h>
 #include "calculate.h"
float
Calculate(float Numeral, char Operation[4])
{
float SecondNumeral;
if(strncmp(Operation, "+", 1) == 0)
{
printf("Второе слагаемое: ");
scanf("%f",&SecondNumeral);
return(Numeral + SecondNumeral);
}
else if(strncmp(Operation, "-", 1) == 0)
{
printf("Вычитаемое: ");
scanf("%f",&SecondNumeral);
return(Numeral - SecondNumeral);
}
else if(strncmp(Operation, "*", 1) == 0)
{
printf("Множитель: ");
scanf("%f",&SecondNumeral);
return(Numeral * SecondNumeral);
}
else if(strncmp(Operation, "/", 1) == 0)
{
printf("Делитель: ");
scanf("%f",&SecondNumeral);
if(SecondNumeral == 0)
{
printf("Ошибка: деление на ноль! ");
return(HUGE_VAL);
}
else
return(Numeral / SecondNumeral);
}
else if(strncmp(Operation, "pow", 3) == 0)
{
printf("Степень: ");
scanf("%f",&SecondNumeral);
return(pow(Numeral, SecondNumeral));
}
else if(strncmp(Operation, "sqrt", 4) == 0)
return(sqrt(Numeral));
else if(strncmp(Operation, "sin", 3) == 0)
return(sin(Numeral));
else if(strncmp(Operation, "cos", 3) == 0)
return(cos(Numeral));
else if(strncmp(Operation, "tan", 3) == 0)
return(tan(Numeral));
else
{
printf("Неправильно введено действие ");
return(HUGE_VAL);
}
}
Интерфейсный файл calculate.h, описывающий формат вызова функциикалькулятора:
///////////////////////////////////////
// calculate.h
 #ifndef CALCULATE_H_
 #define CALCULATE_H_
float Calculate(float Numeral, char Operation[4]);
 #endif /*CALCULATE_H_*/
Основной файл main.c, реализующий интерфейс пользователя к калькулятору:
////////////////////////////////////////
// main.c

3. Выполните компиляцию программы посредством gcc:
gcc -c calculate.c
gcc -c main.c
gcc calculate.o main.o -o calcul -lm

4. При необходимости исправьте синтаксические ошибки.

5. Создайте Makefile со следующим содержанием:

 #
 # Makefile
 #
CC = gcc
CFLAGS =
LIBS = -lm
calcul: calculate.o main.o
gcc calculate.o main.o -o calcul $(LIBS)
calculate.o: calculate.c calculate.h
gcc -c calculate.c $(CFLAGS)
main.o: main.c calculate.h
gcc -c main.c $(CFLAGS)
clean:
-rm calcul *.o *~
 # End Makefile
Поясните в отчёте его содержание.

6. С помощью gdb выполните отладку программы calcul (перед использованием
gdb исправьте Makefile):
– Запустите отладчик GDB, загрузив в него программу для отладки:
gdb ./calcul
– Для запуска программы внутри отладчика введите команду run:
run
– Для постраничного (по 9 строк) просмотра исходного код используйте команду list:
list
– Для просмотра строк с 12 по 15 основного файла используйте list с параметрами:
list 12,15
– Для просмотра определённых строк не основного файла используйте list
с параметрами:
list calculate.c:20,29
– Установите точку останова в файле calculate.c на строке номер 21:
list calculate.c:20,27
break 21
– Выведите информацию об имеющихся в проекте точка останова:
info breakpoints
– Запустите программу внутри отладчика и убедитесь, что программа остановится в момент прохождения точки останова:
run
5
-
backtrace
– Отладчик выдаст следующую информацию:
 #0 Calculate (Numeral=5, Operation=0x7fffffffd280 "-")
at calculate.c:21
 #1 0x0000000000400b2b in main () at main.c:17
а команда backtrace покажет весь стек вызываемых функций от начала программы до текущего места.
– Посмотрите, чему равно на этом этапе значение переменной Numeral, введя:
print Numeral
На экран должно быть выведено число 5.
– Сравните с результатом вывода на экран после использования команды:
display Numeral
– Уберите точки останова:
info breakpoints
delete 1

7. С помощью утилиты splint попробуйте проанализировать коды файлов calculate.c и main.c.

## Выполнение Работы

1. В домашнем каталоге создал подкаталог ~/work/os/lab_prog.

![каталог](image/1.jpg)

2. Создала в нём файлы: calculate.h, calculate.c, main.c.

![calculate.h файл](image/2.3.jpg)

![calculate.c файл](image/2.1.jpg)

![main.c файл](image/2.4.jpg)

3. Выполнилаа компиляцию программы посредством gcc

![компиляция gcc](image/3.jpg)

4. Исправила синтаксические ошибки в файле main.c (удалила & передала  operator в линии scanf("%s",&Operation);)

5. Создала Makefile со следующим содержанием

![Makefile](image/5.jpg)

6. С помощью gdb выполнил отладку программы calcul, (перед использованием gdb исправьте Makefile)

- Запустил отладчик GDB, загружил в него программу для отладки: gdb ./calcul

![gdb](image/6.1.jpg)

- Для запуска программы внутри отладчика ввел команду run: run

![run](image/6.2.jpg)

- Для постраничного (по 9 строк) просмотра исходного код использовал команду list:
list 

![list](image/6.3.jpg)

- Для просмотра строк с 12 по 15 основного файла использовал list с параметрами:
list 12,15

![list](image/6.4.jpg)

- Для просмотра определённых строк не основного файла использовал list с параметрами:
list calculate.c:20,29

![list](image/6.5.jpg)

- Установила точку останова в файле calculate.c на строке номер 21:
list calculate.c:20,27
break 21

![list](image/6.6.jpg)

- Вывела информацию об имеющихся в проекте точка останова:
info breakpoints

![breakpoints](image/6.7.jpg)

- Запустила программу внутри отладчика и убедитесь, что программа остановится в момент прохождения точки останова:
run
5
-
backtrace

![run](image/6.8.jpg)

- Посмотрила, чему равно на этом этапе значение переменной Numeral, введя:
print Numeral

![print](image/6.9.jpg)

- Сравнила с результатом вывода на экран после использования команды:
display Numeral

![display](image/6.10.jpg)

- Убрала точки останова:
info breakpoints
delete 1

![delete](image/6.11.jpg)


7. С помощью утилиты splint попробовала проанализировать коды файлов calculate.c и main.c.

![splint calculate.c](image/7.1.jpg)

![splint main.c](image/7.2.jpg)
 
## Вывод

Я научился приобретать простейшие навыки разработки, анализа, тестирования и отладки приложений в таких ОС, как UNIX / Linux на примере создания калькулятора с простейшими функциями на языке программирования C. 

# Спасибо за внимание
