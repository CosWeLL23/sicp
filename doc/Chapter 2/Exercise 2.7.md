## Chapter 2

### Exercise 2.7

Alyssa's program is incomplete because she has not specified the implementation of the interval abstraction. Here is a definition of the interval constructor:

```scheme
(define (make-interval a b) (cons a b))
```

Define selectors `upper-bound` and `lower-bound` to complete the implementation.

### Solution

Для задания предоставлены процедуры для работы с интервалами:

```scheme
(define (add-interval x y)
  (make-interval (+ (lower-bound x) (lower-bound y))
                 (+ (upper-bound x) (upper-bound y))))

(define (mul-interval x y)
  (let ((p1 (* (lower-bound x) (lower-bound y)))
        (p2 (* (lower-bound x) (upper-bound y)))
        (p3 (* (upper-bound x) (lower-bound y)))
        (p4 (* (upper-bound x) (upper-bound y))))
    (make-interval (min p1 p2 p3 p4)
                   (max p1 p2 p3 p4))))

(define (div-interval x y)
  (mul-interval x 
                (make-interval (/ 1.0 (upper-bound y))
                               (/ 1.0 (lower-bound y)))))

(define (make-interval a b) (cons a b))
```
Завершим реализацию, дописав селекторы интервалов:

```scheme
(define (upper-bound interval)
  (max (car interval) (cdr interval)))

(define (lower-bound interval)
  (min (car interval) (cdr interval)))

(define intrvl1 (make-interval 1 10))
(define intrvl2 (make-interval 10 20))
(define intrvl3 (make-interval 2 -3))

(upper-bound intrvl2)
; => 20
(lower-bound intrvl3)
; => -3

(add-interval intrvl1 intrvl2)
; => (11 . 30)

(mul-interval intrvl2 intrvl3)
; => (-60 . 40)

(div-interval intrvl1 intrvl3)
; => (-3.333333333333333 . 5)
(div-interval intrvl3 intrvl1)
; => (-3 . 2)
```