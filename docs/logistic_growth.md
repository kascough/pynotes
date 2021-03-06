# Logistic Growth

A symbolic solution to the well-known logistic equation to model population growth. In the example below we will compute the first adn second derivative of the function and we will find the inflection point.

$$ f(x) = \frac{L}{1+e^{-k(x-x_0)}}$$


x$_0$ = x-coordinate of the inflection point, the x-value of the sigmoid's midpoint

$L$ = maximum value or carrying capacity

$k$ = growth rate



```python
import numpy as np
import matplotlib.pyplot as plt
import sympy

```


```python
L = 10
x0 = 10
k = 1
x = sympy.symbols('x')
logistic_expr = L / (1 + sympy.exp(-k*(x-x0)))

```


```python
# Convert symbolic expression of sigmoid function to lambda function
f_logistic = sympy.lambdify(x, logistic_expr, 'numpy')

```


```python
# Plot graph of sigmoid function
lower_xlim = 0
upper_xlim = 20
step = 0.1
xvec = np.arange(lower_xlim,upper_xlim, step)
yvec = f_logistic(xvec)

plt.figure()
plt.plot(xvec,yvec)
plt.title('Logistic function')
plt.show()

```


![png](logistic_growth_files/logistic_growth_4_0.png)



```python
# Compute symbolic expression for first derivative of sigmoid function
logistic_expr_prime = sympy.diff(logistic_expr, x, 1)
print(logistic_expr_prime)

```

    10*exp(10 - x)/(exp(10 - x) + 1)**2



```python
# Convert symbolic expression of sigmoid first derivative to lambda function
f_logistic_prime = sympy.lambdify(x, logistic_expr_prime, 'numpy')

```


```python
# Plot graph of first derivative of sigmoid function
yvec_prime = f_logistic_prime(xvec)

plt.figure()
plt.plot(xvec,yvec_prime)
plt.title('First derivative of logistic function')
plt.show()

```


![png](logistic_growth_files/logistic_growth_7_0.png)



```python
# Compute second derivative of sigmoid function
logistic_expr_twoprime = sympy.diff(logistic_expr, x, 2)
print(logistic_expr_twoprime)

```

    -10*(exp(10 - x) - 2*exp(20 - 2*x)/(exp(10 - x) + 1))/(exp(10 - x) + 1)**2



```python
# Convert symbolic expression of sigmoid second derivative to lambda function
f_logistic_twoprime = sympy.lambdify(x, logistic_expr_twoprime, 'numpy')

```


```python
# Plot graph of second derivative of logistic function

yvec_twoprime = f_logistic_twoprime(xvec)

plt.figure()
plt.plot(xvec,yvec_twoprime)
plt.title('Second derivative of logistic function')
plt.show()

```


![png](logistic_growth_files/logistic_growth_10_0.png)



```python
# Let's compute some practical values from all this work
idx_highest_slope = np.argmax(yvec_prime)
x_highest_slope = xvec[idx_highest_slope]
y_highest_slope = yvec[idx_highest_slope]

# Positive acceleration
idx_highest_accel_pos = np.argmax(yvec_twoprime)
x_highest_accel_pos = xvec[idx_highest_accel_pos]
y_highest_accel_pos = yvec[idx_highest_accel_pos]

# Negative acceleration
idx_highest_accel_neg = np.argmin(yvec_twoprime)
x_highest_accel_neg = xvec[idx_highest_accel_neg]
y_highest_accel_neg = yvec[idx_highest_accel_neg]

```


```python
# Plot logistic function with metrics
plt.figure()
plt.plot(xvec,yvec)
plt.scatter(x_highest_slope, y_highest_slope, marker='o', color='r')
plt.scatter(x_highest_accel_pos, y_highest_accel_pos, marker='v', color='k')
plt.scatter(x_highest_accel_neg, y_highest_accel_neg, marker='s', color='k')
plt.title('Logistic function')
plt.show()

```


![png](logistic_growth_files/logistic_growth_12_0.png)


## Observations

- The first derivative was useful to identify the part of fastest growth rate.

- The second derivatives are usually know as acceleration, but in this context the two resulting points seem to be quite useful to objectively determine the portion of the curve that exhibits linear growth (part between the black triangle and the black square).

- Do you see any use for the third derivative?


## References

Logistic equation in Wikipedia: <https://www.wikiwand.com/en/Logistic_function>

