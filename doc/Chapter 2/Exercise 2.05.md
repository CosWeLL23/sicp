## [Chapter 2](../index.md#2-Building-Abstractions-with-Data)

### Exercise 2.5

Show that we can represent pairs of nonnegative integers using only numbers and arithmetic operations if we represent the pair _a_ and _b_ as the integer that is the product 2<sup>a</sup> 3<sup>b</sup>. Give the corresponding definitions of the procedures `cons`, `car`, and `cdr`.

### Solution

([Code](../../src/Chapter%202/Exercise%202.05.scm))

Из данного задания прямо следует определение процедуры `cons`:

```scheme
(define (cons a b)
  (* (expt 2 a)
     (expt 3 b)))
```

Теперь нужно каким-то способом узнать, как определить селекторы на основе такого конструктора. Метод тыка мне очень здесь помог. Моё любимое число — 23 `¯\_(ツ)_/¯`, следовательно, использовал в паре в качестве примера числа 2 и 3.

```scheme
(define pair (cons 2 3))

pair
; => 108
```

Тут нужно заметить, что такой конструктор всегда возращает просто число. Так как в конструкторе основаниями множителей всегда являются двойка и тройка (это не те двойка и тройка, которые я в конструктор подставил), первое, что я попробовал, поделить получившееся значение на 2 или 3 и обнаружил, что 108 всего 3 раза последовательно делится на 3 без остатка, и 2 раза на 2.

```
1. 108 / 3 = 36
2. 36 / 3 = 12
3. 12 / 3 = 4

1. 108 / 2 = 54
2. 54 / 2 = 27
```

Для пущей уверенности еще несколько примеров:

```scheme
(define pair-3-5 (cons 3 5))
(define pair-5-10 (cons 5 10))
(define pair-10-5 (cons 10 5))

pair-3-5
; => 1944

pair-5-10
; => 1889568

pair-10-5
; => 248832
```

|  | 1944 / 2 | 1944 / 3 | 1 889 568 / 2 | 1 889 568 / 3 | 248 832 / 2 | 248 832 / 3
:-:|:--------:|:--------:|:-------------:|:-------------:|:-----------:|:-----------:
 1 |    972   |   648    |    944 784    |    629 856    |   124 416   |   82 944
 2 |    486   |   216    |    472 392    |    209 952    |    62 208   |   27 648
 3 |  **243** |    72    |    236 196    |     69 984    |    31 104   |    9 216
 4 |  121.5   |    24    |    118 098    |     23 328    |    15 552   |    3 072
 5 |          |   **8**  |   **59 049**  |      7 776    |     7 776   |  **1 024**
 6 |          | 2.(6)    |   29 524.5    |      2 592    |     3 888   |  341.(3)
 7 |          |          |               |        864    |     1 944   |
 8 |          |          |               |        288    |       972   |
 9 |          |          |               |         96    |       486   |
10 |          |          |               |       **32**  |     **243** |
|  |          |          |               |     10.(6)    |     121.5   |

На основе полученных данных можно построить следующие конструкторы:

```scheme
(define (reminder a b) (mod a b))
(define (inc x) (+ x 1))

(define (num-of-divisions num div)
  (define (iter cur count)
    (if (= 0 (reminder cur div))
        (iter (/ cur div) (inc count))
        count))
  (iter num 0))

(define (car n) (num-of-divisions n 2))
(define (cdr n) (num-of-divisions n 3))

(car pair-3-5)
; => 3
(cdr pair-3-5)
; => 5

(car pair-5-10)
; => 5
(cdr pair-5-10)
; => 10

(car pair-10-5)
; => 10
(cdr pair-10-5)
; => 5
```
