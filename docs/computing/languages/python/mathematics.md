
## The ```math``` module
The ```math``` module contains many useful mathematics functions.
```python
import math
out = math.sin(math.pi)
print(out)
```
> 1.2246467991473532e-16

## Floor division
Ignores the remainder following a division operation.
```python
print(3//2)
```
> 1

## Fractions
Fraction objects can be created and manipulated.
```python
from fractions import Fraction
print(Fraction(1,2))
print(Fraction(3,4) + 5 + 2.7)
```
> 1/2
> 8.45

## Complex numbers
Use ```j``` instead of ```i``` to create and manipulate complex numbers.
```python
img1 = 1 + 1j
img2 = 2 - 3j
img3 = 4 + 0j
print(type(img1))
print(img1 - img2)
print(img3 / img1)
print(img1.real) # Get real value
print(img1.imag) # Get imaginary value
print(img1.conjugate()) # Get complex conjugate
print(abs(img2)) # Get the magnitude of the complex number
```
> <class 'complex'>\
> (-1+4j)\
> (2-2j)\
> 1.0\
> 1.0\
> (1-1j)\
> 3.605551275463989

NB: Cannot use floor division (```//```) or modulus (```%```) operations on complex numbers.
See ```cmath``` for further complex number operations.

## Symbolic Mathematics
The ```sympy``` allows for symbolic mathematics operations to be performed.
```python
from sympy import Symbol, symbols, factor, expand, pprint
x = Symbol('x') # For single symbol definitions.
y = Symbol('y')

a, b, c = symbols('a, b, c') # For multiple symbol definitions.
print(a*x**2 + a*x**2 +b*x + c - c) # Simplification

factorised_expression = factor(x**2 - y**2) # Factorisation
print(factorised_expression)

expanded_expression = expand((x+y)**3) # Bracket expansion
print(expanded_expression)
pprint(expanded_expression) # Prettier printing
```
```
2*a*x**2 + b*x 
(x - y)*(x + y) 
x**3 + 3*x**2*y + 3*x*y**2 + y**3 
 3      2        2    3 
x  + 3⋅x ⋅y + 3⋅x⋅y  + y 
```

### Series Expansions
```python
from sympy import Symbol, pprint, init_printing

def print_series(n : int) -> str:
    ''' Print a series expansion up to n.

    1. Ensure the successive terms are printed in ascending powers of x.
    2. Create a first term, then redefine by adding successive terms.
    3. Print the series.
    '''

    # Print the series in ascending powers of x.
    init_printing(order='rev-lex')

    x = Symbol('x')
    series = x
    for power in range(2, n+1):
        series = series + (x**power)/power
    pprint(series)

print_series(int(10))
```
```
     2    3    4    5    6    7    8    9    10
    x    x    x    x    x    x    x    x    x
x + ── + ── + ── + ── + ── + ── + ── + ── + ───
    2    3    4    5    6    7    8    9    10
```
### Substitution
Once an expression has been created, we can substitute values into the symbols to get solutions.
```python
from sympy import symbols

x, y = symbols('x, y')
equation = x+y
solution = equation.subs({x:7, y:12})
print(solution)

solution = equation.subs({x:7, y:x+9})
print(solution)
```
> 19 \
> x + 16

Note that ```.subs``` takes  dictionary as input, which is very handy.
```python
from sympy import symbols

values = {'m': 10, 'a': 5}

m, a = symbols('m, a')
equation = m*a
force = equation.subs(values)
print(force)
```
> 50

### Sympification
A more direct approach to create and interpret expressions involves using ```sympify```.
```python
from sympy import sympify

raw = input('Input Equation: ')
interpretation = sympify(raw)
print(interpretation)
```
```
Input Equation: x*2+y**2-z**0.5 
2*x + y**2 - z**0.5
```

### Solving Equations
The ```solve``` function may be used to solve equations. Sympy always assumes that the equation equals zero (i.e. ax^2 + bx + c = 0). Note that the solver will account for imaginary results.
```python
from sympy import sympify, solve

equation = sympify('x**2 + 5*x + 5')
print(solve(equation))

equation = sympify('5*x**2 - 3*x + 5')
print(solve(equation))
```
> [-5/2 - sqrt(5)/2, -5/2 + sqrt(5)/2] \
> [3/10 - sqrt(91)*I/10, 3/10 + sqrt(91)*I/10]

### Rearranging Equations
```python
from sympy import symbols, solve, pprint
s, u, a, t = symbols('s, u, a, t')
equation = u*t + (1/2)*a*(t**2) - s
solution = solve(equation, t, dict=True)
pprint(solution)
solution = solve(equation, a, dict=True)
pprint(solution)
```
```
⎡⎧           ______________⎫  ⎧           ______________⎫⎤
⎢⎪          ╱            2 ⎪  ⎪          ╱            2 ⎪⎥
⎢⎨   -u - ╲╱  2.0⋅a⋅s + u  ⎬  ⎨   -u + ╲╱  2.0⋅a⋅s + u  ⎬⎥
⎢⎪t: ──────────────────────⎪, ⎪t: ──────────────────────⎪⎥
⎣⎩             a           ⎭  ⎩             a           ⎭⎦
⎡⎧   2.0⋅(s - t⋅u)⎫⎤
⎢⎪a: ─────────────⎪⎥
⎢⎨         2      ⎬⎥
⎢⎪        t       ⎪⎥
⎣⎩                ⎭⎦
```
NB: The above equations are presented more clearly in the actual terminal output.

### Plotting
You can also plot functions directly using sympy.
```python
from sympy.plotting import plot
from sympy import Symbol
x = Symbol('x')
equation = x**2 + 3*x + 5
plot(equation)
```
```
    140 |
        |                                                      /
        |                                                     /
        |                                                    /
        |                                                   /
        |                                                  /
        |                                                 /
        |                                                /
        |                                               /
        |\                                             /
     70 |-\-------------------------------------------/---------
        |  \                                         /
        |   \                                      ..
        |    ..                                   /
        |      \                                 /
        |       ..                             ..
        |         ..                         ..
        |           ..                     ..
        |             ...               ...
        |                .....     .....
      0 |_______________________________________________________
         -10                        0                          10
```

### Limits
Sympy can also be used to evaluate limits.
```python
from sympy import Limit, Symbol, S
x = Symbol('x')
out = Limit(1/x, x, S.Infinity)
print(out)
```
```
Limit(1/x, x, oo, dir='-')
0 # i.e. 1/x tends to 0 as x appraches infinity
```
NB: The ```S``` class contains infinity, as well as other useful maths functions. 
The ```dir='-'```tag inidcates the direction in which the limit is approached.
```python
from sympy import Limit, Symbol, S
x = Symbol('x')
pos = Limit(1/x, x, 0, dir='+').doit()
neg = Limit(1/x, x, 0, dir='-').doit()
print(pos, neg)
```
```
oo -oo
```

### Derivatives and Integrals
Differentiation
```python
from sympy import Symbol, Derivative, sympify
fn = sympify('x**3')
der = Derivative(fn, 'x').doit()
print(der)
```
```
3*x**2
```
Indefinite Integration
```python
from sympy import Integral, sympify
fn = sympify('x**3')
x = sympify('x')
integ = Integral(fn, x).doit()
print(integ)
```
```
x**4/4
```
NB: the integral method is sensitive to string inputs, hence why x had to sympified prior to its usage.
Definite Integration (i/e. within limits)
```python
from sympy import Integral, sympify
fn = sympify('x**3')
integ = Integral(fn, ('x', 0, 4)).doit()
print(integ)
```
```
64
```
#### [Back](README.md)
