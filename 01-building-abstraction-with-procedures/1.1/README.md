# The Elements of Programming

- primitive expressions
- means of combination
- means of abstraction

## Expressions

- lisp uses prefix notation for expressions

```
486
> 486

(+ 137 349)
> 486
```

## Naming and the Environment

```lisp
(define size 2)
```

It should be clear that the possibility of associating values with symbols and later retrieving them
means that the interpreter must maintain some sort of memory that keep track of name object pairs.
This memory is called the *environment*.

## Evaluating Combinations

To evaluate a combination do the following:
- evaluate the subexpression of the combination
- apply the procedure (that is the value of the leftmost subexpression the **operator**)
  to the arguments that are the values of the other subexpressions(the **operands**)

This general evaluation rule will not apply to `special forms` like

- `define`

Each special form has it's own evaluation rule.

## Compound Procedures

- procedure definitions, giving name to a compound operation and then referred as a unit

```lisp
(define (square x) (* x x))
```

General form of a procedure definition:

```lisp
(define (<name> <formal parameters>) <body>)
```

## The substitution model for procedure application

To apply a compund procedure to arguments, evaluate the body of the procedure with each formal parameter by
replaced by the corresponding argument.

> The purpose of substitution model is to help us think about procedure application, not to provide a description
> of how the interpreter really works.

### Applicative order vs Normal Order

- Normal Order Evaluation
    - fully expand and then reduce
- Applicative order Evaluation
    - evaluate the arguments and then apply
    - more efficient
    - generally used in interpreters

## Conditional Expressions and Predicates

`cond` is also a special form.
Expressions which evaluates to `true` or `false` are called predicates.

```lisp
(cond (<p1> <e1>)
      (<p2> <e2>)
      .
      .
      .
      (<p3> <e3>))
```

Example using absoulute function,

$$
|x| = \begin{cases}
    x & \text{if } x > 0 \\
    0 & \text{if } x = 0 \\
    -x & \text{if } x < 0
\end{cases}
$$

```lisp
(define (abs x)
        ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x)))
```

`if` general expression

```lisp
(if <predicate> <consequent> <alternative>)
```

### Logical Operators

- `(and <e1> ... <en>)`
- `(or <e1> ... <en>)`
- `(not <e>)`

## Example: Square Root By Newtons Method

Newtons method is used to find roots of a function.
That is the value for which the function value is $0$.

So if we want to find square root of $x_0$, finding the
root of function $f(x) = x^2 - x_0$ will give the answer.

### Newton's Method

**Intuition:** Given a function, now for every value in the domain of that function
the tangent to that point intersect the x-axis at a point which is different, but
only for the point which are root the tangent intersect only at one point.

Let $x_n$ be your current guess, then

$$
f'(x_n) = \frac{0 - 0f(x_n)}{x_{n+1} - x_n}
$$

solving for $x_{n+1}$ till we get a good approximation will give the result.

Given $f(x)$, the root of function can be obtained using:

$$
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$$

### For square root

$$
f(x) =  x^2 - x_0, \\
f'(x) = 2x, \\
x_{n+1} = x_n - \frac{x_n^2 - x_0}{2x_n} = \frac{x_n^2 + x_0}{2x_n} = \frac{1}{2}\left(x_n + \frac{x_0}{x_n}\right)
$$

$$
\text{Improved Guess} = \frac{1}{2}\left(\text{Guess} + \frac{x_0}{\text{Guess}}\right)
$$

```lisp
(define (square-iter guess x)
    (if (good-enough? guess x)
        guess
        (square-iter (improve guess x) x)
    )
)
```

```lisp
(define (good-enough? guess x)
    (<
        (abs (- (* guess guess) x))
        0.001
    )
)
```

```lisp
(define (improve guess x)
    (average guess (/ x guess))
)
```

```lisp
(define (sqrt x)
    (sqrt-iter 1.0 x)
)
```

```lisp
(define (sqrt x)
    (define (good-enough? guess)
        (< (abs (- (square guess) x)) 0.001))
    (define (improve guess)
        (average guess (/ x guess)))
    (define (sqrt-iter guess)
        (if (good-enough? guess)
            guess
            (sqrt-iter (improve guess))))
    (sqrt-iter 1.0))
```