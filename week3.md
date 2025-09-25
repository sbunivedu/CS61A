```scheme
#lang simply-scheme

(define (square x) (* x x))

(define (squares sent)
  (if (empty? sent)
      '()
      (se (square (first sent))
          (squares (bf sent)) )))

(trace squares)

; (squares '(1 2 3 4 5)) => '(1 4 9 16 25)
```

```scheme
#lang simply-scheme

(define (sort sent)
  (if (empty? sent)
      '()
      (insert (first sent)
              (sort (bf sent)) )))

(define (insert num sent)
  (cond ((empty? sent) (se num sent))
        ((< num (first sent)) (se num sent))
        (else (se (first sent) (insert num (bf sent)))) ))

(trace sort)
(trace insert)

; (sort '(6 23 5 100 7)) => '(5 6 7 23 100)
```
