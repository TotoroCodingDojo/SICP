# Formulating Abstractions with Higher Order Procedures

## Construction Procedures using lambda

```lisp
(lambda (x) (+ x 4))
```

In general, lambda is used to create procedures in the same way as
define, except that no name is specified for the procedure:

```lisp
(lambda (⟨formal-parameters⟩) ⟨body⟩)
```

## Use `let` to create local variable

```lisp
(let ((⟨var1 ⟩ ⟨exp1 ⟩)
    (⟨var2 ⟩ ⟨exp2 ⟩)
    ...
    (⟨varn ⟩ ⟨expn ⟩))
    ⟨body⟩)
```

Let is only syntactic sugar and is equivalent to following lambda:

```lisp
((lambda (⟨var1 ⟩ . . . ⟨varn ⟩)
    ⟨body⟩)
⟨exp1 ⟩
...
⟨expn ⟩)
```


## Procedures as General Methods

### Finding roots of equations by the half-interval method

- <https://en.wikipedia.org/wiki/Bisection_method>

```lisp
(define (close-enough? x y) (< (abs (- x y)) 0.001))
(define (search f neg-point pos-point)
    (let (
            (midpoint (average neg-point pos-point))
        )
        (if (close-enough? neg-point pos-point)
            midpoint
            (let (
                    (test-value (f midpoint))
                )
                (cond 
                    ((positive? test-value) (search f neg-point midpoint))
                    ((negative? test-value) (search f midpoint pos-point))
                    (else midpoint)
                )
            )
        )
    )
)

(define (half-interval-method f a b)
    (let (
            (a-value (f a))
            (b-value (f b))
        )
        (cond 
            ((and (negative? a-value) (positive? b-value)) (search f a b))
            ((and (negative? b-value) (positive? a-value)) (search f b a))
            (else (error "Values are not of opposite sign" a b))
        )
    )
)

(half-interval-method 
    (lambda (x) (- (* x x x) (* 2 x) 3))
    1.0
    2.0
)
```

### Finding fixed points of functions

- <https://en.wikipedia.org/wiki/Fixed_point_(mathematics)>

- Fixed point is the value for function such that $f(c) = c$, that is applying the transformation on the value does not change.

So if you have some iterative function $f(x_n)$ to find a new value $x_{n+1}$ which will converge to some value.
We can say that value is also fixed point for that iterative function.

So for finding square root we can also use this method.

$$
y^2 = x_0 \\
y = \frac{x_0}{y} \\
y_{n+1} = \frac{x_0}{y_n}
$$

But this expression will oscillate and not converge. This can be dealt with adding y to both sides.

$$
2y = y + \frac{x_0}{y} \\
y = \frac{1}{2}\left(y + \frac{x_0}{y} \right) \\
y_{n+1} = \frac{1}{2}\left(y_n + \frac{x_0}{y_n} \right)
$$

And we reached the same expression as using the newton's method.
