### Example Programs

```scheme
(define (buzz n)
  (cond ((equal? (remainder n 7) 0) 'buzz)
	((member? 7 n) 'buzz)
	(else n)))
```

```scheme
#lang simply-scheme

(define (plural wd)
  (word wd 's))

(define (plural1 wd)
  (if (equal? (last wd) 'y)
      (word (bl wd) 'ies)
      (word wd 's)))

;(plural 'computer) => 'computers
;(plural 'boy) => 'boys
;(plural 'fly) => 'flys
;(plural1 'book) => 'books
;(plural1 'fly) => 'flies
;(item 4 'computer) => 'p
```

```scheme
#lang simply-scheme

(define (pigl wd)
  (if (pl-done? wd)
      (word wd 'ay)
      (pigl (word (bf wd) (first wd)))))

(define (pl-done? wd)
  (vowel? (first wd)))

(define (vowel? letter)
  (member? letter '(a e i o u)))

;(pigl 'scheme) => 'emeschay
;(pigl 'rythmn) => infinite recursion
```

