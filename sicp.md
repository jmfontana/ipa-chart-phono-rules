#SICP-Notebook

##Section 1.1

Expressions are formed by a list, enclosed by parentheses, of the operator, followed by its operands. The output is the value of the procedure's application. For example, (+ 1 2) with operator +, operands 1 and 2, and a value of 3.

```scheme
(define (var-name arg1 arg2) (body)) is shorthand for (define var-name (lambda (arg1 arg2) (body))
```

as in

```scheme
(define (square x) (* x x)) being shorthand for (define square (lambda (x) (* x x)))
```

Similarly, in the case of application

```scheme
(square 5) has the same values as ((lambda (x) (* x x)) 5)
```

Evaluation:

To evaluate a combination: (i) evaluate the subexpressions of the combination; (ii) Apply the procedure that is the value of the leftmost subexpression to the arguments that are the value of the other subexpressions.

In order words, scheme follows has an applicative-order evaluation (in comparison to a "fully expand and then reduce" alternative given by normal-order evaluation).

Thus, 

```scheme
(* (+ 2 (* 4 6)) 
  (+ 3 5 7))
(* (+ 2 24)
  15)
(* 26 15)
```


####Ex 1.1.
* 10
* 12
* 8
* 3
* 6
* \-\-
* \-\-
* 19
* \#f
* 4
* 16
* 6
* 16


####Ex. 1.2.
```scheme
(/ (+ 5 4 
      (- 2 
	 (- 3 (+ 6
		 (/ 4 5)))))
   (* 3 
      (- 6 2)
      (- 2 7)))
```

####Ex. 1.3.

```scheme
(define (sqr x) (* x x)) ;the square of a number
(define (sqr-big a b c) (cond ((> a b c) (+ (sqr a) (sqr b)))
				 ((> a c b) (+ (sqr a) (sqr c)))
				 ((> b c a) (+ (sqr b) (sqr c)))
 				 ((> b a c) (+ (sqr b) (sqr a)))
				 ((> c b a) (+ (sqr c) (sqr b)))
				 ((> c a b) (+ (sqr c) (sqr a)))
				)) ;pick the largest two numbers out of three and add their squares.
```

####Ex. 1.4.
"a-plus-abs-b" takes two arguments. If the second argument is a positive integer, the two arguments are added. If the second argument is negative, the second arguments is substracted from the first. This has the same effect as taking the absolute value of the 2nd argument, as negative integers are substracted and positive ones added.

####Ex. 1.5. 
* Applicative-order: We get stuck in a loop, as *(test 0 (p))*  will not get to the if-statement before evaluating *(p)*, which is *(p)* again ad infinitum.

* Normal-order: The interpreter will not evaluate *(p)* beforehand and begin by evaluating the if-statement. Since the condition is met *(= x 0)*, the value of this procedure is *0*.


####Ex. 1.11.
```scheme
;f is defined by the rule that f(n) = n if n<3 and f(n) = f(n - 1) + 2f(n - 2) + 3f(n - 3) if n> 3
(define (f n)
  (cond ((< n 3) n)
        ((or (> n 3) (= n 3))
         (+ (f (- n 1))
            (* 2 (f (- n 2)))
            (* 3 (f (- n 3)))))
        (else 'undefined))) ;for good measure
```

####Ex. 1.12.
```scheme
;get the sum of a row from Pascal's triangle.
(define (row n)
  (cond ((= n 1) n)
        ((> n 1) (+ (row (- n 1))
                    (row (- n 1))))
        (else 'undefined)))
```

####Ex. 1.20. 
Using pseudo-Scheme the operations for applicative-order (gdc 206 40) encompass 5 steps ignoring the remainder-expansions. Otherwise 10 with 4 calls of remainder.

```scheme
(gcd 40 (remainder 206 40)) ;I'll start skipping this step
(gcd 40 6)
(gcd 6 4)
(gcd 4 2)
(gcd 2 0)
2
```

For normal-order (gdc 206 40) we get 18 calls in total, which I won't write down as it won't be very enlightening. 

```scheme
(gcd 40 (remainder 206 40))
(gcd (remainder 206 40) (remainder 40 (remainder 206 40)))
...
2
```




##Section 1.2
