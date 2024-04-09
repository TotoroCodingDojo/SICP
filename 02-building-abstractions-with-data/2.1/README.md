# Introduction to Data Abstraction


Data Abstractions is the methodology that enables us to isolate how a compound data
objet is used from the details of how it is constructed from more primitive data objects.

## Example: Arithmetic Operations for Rational Numbers

```lisp
(define (add-rat x y)
    (make-rat 
        (+ (* (numer x) (denom y)) (* (numer y) (denom x)))
        (* (denom x) (denom y))
    )
)
(define (sub-rat x y)
    (make-rat 
        (- (* (numer x) (denom y)) (* (numer y) (denom x)))
        (* (denom x) (denom y))
    )
)
(define (mul-rat x y)
    (make-rat 
        (* (numer x) (numer y))
        (* (denom x) (denom y))
    )
)
(define (div-rat x y)
    (make-rat 
        (* (numer x) (denom y))
        (* (denom x) (numer y))
    )
)
(define (equal-rat? x y)
    (= 
        (* (numer x) (denom y))
        (* (numer y) (denom x))
    )
)
```
## Pairs

- can be constructed with primitive procedure `cons` (construct)
- this procedure take two arguments and returns a compound data object
  that contains the two arguments as parts
- given a pair we can extract the parts using the primitive procedures `car` `cdr`

```lisp
(define x (cons 1 2))
(car x) -> 1
(cdr x) -> 2
```

- constructors and selectors

```lisp
(define (make-rat n d) (cons n d))
(define (numer x) (car x))
(define (denom x) (cdr x))
```

```lisp
(define (print-rat x)
    (newline)
    (display (numer x))
    (display "/")
    (display (denom x))
)
```