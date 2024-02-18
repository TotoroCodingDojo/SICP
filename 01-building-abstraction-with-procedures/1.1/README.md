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

It should be clear that the possiblity of associating values with symbols and later retriving them
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
