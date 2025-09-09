1. Start DrRacket. Type "#lang simply-scheme" (without quotes) in the definition window
to select the language. Type each of the following expressions into the interactive window,
ending the line with the Enter (carriage return) key. Think about the results!
Try to understand how Scheme interprets what you type.
```scheme
3
(+ 2 3)
(+ 5 6 7 8)
(+)
(sqrt 16)
(+ (* 3 4) 5)
+
'+
'hello
'(+ 2 3)
'(good morning)
(first 274)
(butfirst 274)
(first 'hello)
(first hello)
(first (bf 'hello))
(+ (first 23) (last 45))
(define pi 3.14159)
pi
'pi
(+ pi 7)
(* pi pi)
(define (square x) (* x x))
(square 5)
(square (+ 2 3))
```

2. Predict what Scheme will print in response to each of these expressions.
Then try it and make sure your answer was correct, or if not, that you understand why!
```scheme
(define a 3)
(define b (+ a 1))
(+ a b (* a b))
(= a b)
(if (and (> b a) (< b (* a b)))
    b
    a)
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
(+ 2 (if (> b a) b a))
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1))
((if (< a b) + -) a b)
```

3. Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two
larger numbers.