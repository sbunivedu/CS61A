# Data Abstraction and Sequences

```scheme
#lang simply-scheme

(define (total hand)
  (if (empty? hand)
      0
      (+ (butlast (last hand))
         (total (butlast hand)) )))

; (total '(3h 10c 4d)) => 17

; This function calls butlast in two places. What do those two invocations mean? Compare it with a modified version:

(define (total1 hand)
  (if (empty? hand)
      0
      (+ (rank (one-card hand))
         (total1 (remaining-cards hand)) )))

(define rank butlast)

(define suit last)

(define one-card last)

(define remaining-cards butlast)

; (total1 '(3h 10c 4d)) => 17

; Both total and total1 implements the same function. total1 is much easier to understand
; because it is self-documenting.

(define (make-card rank suit)
  (word rank (first suit)))

(define make-hand se)

(total1 (make-hand (make-card 3 'heart)
                       (make-card 10 'club)
                       (make-card 4 'diamond)))
```

```
#lang simply-scheme

(define (total hand)
  (if (empty? hand)
      0
      (+ (card-rank (one-card hand))
         (total (remaining-cards hand)) )))

; We have changed the internal representation so that a card is now just a number between 1 and 52.
(define (make-card rank suit)
  (cond ((equal? suit 'heart) rank)
        ((equal? suit 'spade) (+ rank 13))
        ((equal? suit 'diamond) (+ rank 26))
        ((equal? suit 'club) (+ rank 39))
        (else (error "say what?")) ))

(define make-hand se)

(define one-card last)

(define remaining-cards butlast)

; new selectors that work with the new representation

(define (card-rank card)
  (remainder card 13))

(define (card-suit card)
  (nth (quotient card 13) '(heart spade diamond club)))

(define nth item)

(total (make-hand (make-card 3 'heart)
                  (make-card 10 'club)
                  (make-card 4 'diamond)))
; => 17 as before

;(card-rank 49) => 10
;(card-suit 49) => 'diamond
```

#lang simply-scheme

(define (total hand)
  (if (empty? hand)
      0
      (+ (card-rank (one-card hand))
         (total (remaining-cards hand)) )))

; We have changed the internal representation so that a card is now just a number between 1 and 52.
(define (make-card rank suit)
  (cond ((equal? suit 'heart) rank)
        ((equal? suit 'spade) (+ rank 13))
        ((equal? suit 'diamond) (+ rank 26))
        ((equal? suit 'club) (+ rank 39))
        (else (error "say what?")) ))

(define make-hand se)

(define one-card last)

(define remaining-cards butlast)

; new selectors that work with the new representation

(define (card-rank card)
  (remainder card 13))

(define (card-suit card)
  (nth (+ (quotient card 13) 1) '(heart spade diamond club)))

(define nth item)

(total (make-hand (make-card 3 'heart)
                  (make-card 10 'club)
                  (make-card 4 'diamond)))
; => 17 as before

;(card-rank 49) => 10
;(card-suit 49) => 'club
;(card-rank (make-card 10 'club)) => 10
;(card-suit (make-card 10 'club)) => 'club
;(card-suit (make-card 4 'diamond)) => 'diamond