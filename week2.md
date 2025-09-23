Using functions as arguments
```scheme
#lang racket

(define pi 3.141592654)
(define (square-area r) (* r r))
(define (circle-area r) (* pi r r))
(define (sphere-area r) (* 4 pi r r))
(define (hexagon-area r) (* (sqrt 3) 1.5 r r))

;(circle-area 3) => 28.274333886

(define (area shape r) (* shape r r))

(define square 1)
(define circle pi)
(define sphere (* 4 pi))
(define hexagon (* (sqrt 3) 1.5))

;(area circle 3) => 28.274333886

(define (sumsquare a b)
  (if (> a b)
      0
      (+ (* a a) (sumsquare (+ a 1) b)) ))

(define (sumcube a b)
  (if (> a b)
      0
      (+ (* a a a) (sumcube (+ a 1) b)) ))

;(sumsquare 2 5) => 54
```

Generalize the sum of a series
```scheme
#lang racket

(define (sumsquare a b)
  (if (> a b)
      0
      (+ (* a a) (sumsquare (+ a 1) b)) ))

(define (sumcube a b)
  (if (> a b)
      0
      (+ (* a a a) (sumcube (+ a 1) b)) ))

;(sumsquare 2 5) => 54

(define (sum fn a b)
  (if (> a b)
      0
      (+ (fn a) (sum fn (+ a 1) b))))

(define (square x) (* x x))

;(sum square 2 5) => 54
; Note that square is not in "()", we are passing "square" to "sum" as a thing.
```

Another example of generalizing a pattern involving functions:
```scheme
#lang simply-scheme

(define (evens nums)
  (cond ((empty? nums) '())
        ((= (remainder (first nums) 2) 0)
         (se (first nums) (evens (bf nums))) )
        (else (evens (bf nums))) ))

(define (ewords sent)
  (cond ((empty? sent) '())
        ((member? 'e (first sent))
         (se (first sent) (ewords (bf sent))) )
        (else (ewords (bf sent))) ))

(define (pronouns sent)
  (cond ((empty? sent) '())
        ((member? (first sent) '(I me you he she it him her we us they them))
         (se (first sent) (pronouns (bf sent))) )
        (else (pronouns (bf sent))) ))

(define (keep pred sent)
  (cond((empty? sent) '())
       ((pred (first sent))
        (se (first sent) (keep pred (bf sent))))
       (else (keep pred (bf sent)))))

(define (eword? wd)
  (member? 'e wd))

;(evens '(3 5 1 4 9 8 6 7)) => '(4 8 6)
;(ewords '(got to get you into my life)) => '(get life)
;(pronouns '(I really want to dance with you)) => '(I you)

;(keep even? '(3 5 1 4 9 8 6 7)) => '(4 8 6)
;(keep eword? '(got to get you into my life)) => '(get life)
;(keep (lambda (wd) (member? 'e wd)) '(got to get you into my life)) => '(get life)

; higher order function that returns a function

(define (makeadder num)
  (lambda (x) (+ x num)))

(define add3 (makeadder 3))
(define add5 (makeadder 5))

;(makeadder 3) => #<procedure>
;((makeadder 3) 5) => 8
;(add3 5) => 8
;(add5 (add3 2)) => 10
;((makeadder 9) 7) => 16
```

Functions as return values
```scheme
#lang simply-scheme

(define (compose f g)
  (lambda (x)
    (f (g x))))

(define second (compose first bf))

;((compose first bf) '(she loves you)) => 'loves
;(second '(I want to hold your hand)) => 'want

(define (twice f)
  (compose f f))

(define (square x) (* x x))

;((twice square) 3) => 81
```

```scheme
#lang simply-scheme

;;; Note: all versions work only for quadratics with real roots
;;; Straightforward but slow way:

(define (roots a b c)
  (se (/ (+ (- b) (sqrt (- (* b b) (* 4 a c)))) (* 2 a))
      (/ (- (- b) (sqrt (- (* b b) (* 4 a c)))) (* 2 a)) ))

;(roots 1 -5 6) => '(3 2)
```

Use an internal helper function
```scheme
#lang simply-scheme

;;; Using a subprocedure to eliminate the repeated computation:

(define (roots a b c)
  (define (roots1 d)
    (se (/ (+ (- b) d) (* 2 a))
        (/ (- (- b) d) (* 2 a)) ))
  (roots1 (sqrt (- (* b b) (* 4 a c)))))
```

Use lambda
```scheme
#lang simply-scheme

;;; Using lambda to avoid naming the subprocedure:

(define (roots a b c)
  ((lambda (d)
     (se (/ (+ (- b) d) (* 2 a))
         (/ (- (- b) d) (* 2 a))))
   (sqrt (- (* b b) (* 4 a c)))))
```

Use let to rearrange
```scheme
#lang simply-scheme

;;; Using let to rearrange the above:

(define (roots a b c)
  (let ((d (sqrt (- (* b b) (* 4 a c)))))
    (se (/ (+ (- b) d) (* 2 a))
        (/ (- (- b) d) (* 2 a)))))
```

More optimization
```scheme
#lang simply-scheme

;;; More optimization:

(define (roots a b c)
  (let ((d (sqrt (- (* b b) (* 4 a c))))
	(-b (- b))
	(2a (* 2 a)))
    (se (/ (+ -b d) 2a)
	(/ (- -b d) 2a))))
```

```scheme
#lang simply-scheme

(every (lambda (letter) (word letter letter)) 'purple)
(every (lambda (number) (if (even? number) (word number number) number))
       '(781 5 76 909 24))
(keep even? '(781 5 76 909 24))
(keep (lambda (letter) (member? letter 'aeiou)) 'bookkeeper)
(keep (lambda (letter) (member? letter 'aeiou)) 'syzygy)
(keep (lambda (letter) (member? letter 'aeiou)) '(purple syzygy))
(keep (lambda (wd) (member? 'e wd)) '(purple syzygy))
```