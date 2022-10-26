# Math Function

## ceil

- 向上取整

- Example: `ceil(2.4)`

- Output: `3`

## floor

- 向下取整

- Example: `floor(2.6)`

- Output: `2`

## percentage

- 浮点数转百分数

- Example: `percentage(0.5)`

- Output: `50%`

## round

- 指定位数,四舍五入

- Parameters:

- number: 浮点数
- decimalPlaces: 需要保留的位数
- Returns: number

- `Example`: `round(1.67)`

- `Output`: `2`

- `Example`: `round(1.67, 1)`

- `Output`: `1.7`

## sqrt

- 开更号运算, 单位不变

- parameters: number - 浮点数.

- returns: number

- example:

```less

 sqrt(25cm)
```

- output:

```css
 5cm
 ```

- example:

```less
 sqrt(18.6%)
 ```

- output:

```css
4.312771730569565%;
```

## abs

- 绝对值, 单位不变

- parameters: `number` - 浮点数

- returns: `number`

- example #1: `abs(25cm)`

- output: `25cm`

- example #2: `abs(-18.6%)`

- output: `18.6%`

## sin

- 正弦运算

- assumes radians on numbers without units.

- parameters: number - 浮点数

- returns: number

example:

```less
sin(1); // sine of 1 radian
sin(1deg); // sine of 1 degree
sin(1grad); // sine of 1 gradian
```

output:

```css
0.8414709848078965; // sine of 1 radian
0.01745240643728351; // sine of 1 degree
0.015707317311820675; // sine of 1 gradian
```

## asin

calculates arcsine (inverse of sine) function.

returns number in radians e.g. a number between -π/2 and π/2.

parameters: number - floating point number from [-1, 1] interval.

returns: number

example:

asin(-0.8414709848078965)
asin(0)
asin(2)
output:

-1rad
0rad
nanrad

## cos

- 余弦运算
- assumes radians on numbers without units.

- parameters: number - 浮点数

- returns: number

example:

```less
cos(1) // cosine of 1 radian
cos(1deg) // cosine of 1 degree
cos(1grad) // cosine of 1 gradian
```

output:

```css
0.5403023058681398 // cosine of 1 radian
0.9998476951563913 // cosine of 1 degree
0.9998766324816606 // cosine of 1 gradian
```

## acos

- 反余弦运算

- returns number in radians e.g. a number between 0 and π.

- parameters: number - a floating point number from [-1, 1] interval.

- returns: number

example:

```less
acos(0.5403023058681398)
acos(1)
acos(2)
```

output:

```css
1rad
0rad
nanrad
```

## tan

- 正切运算

- Assumes radians on numbers without units.

- Parameters: number - 浮点数

- Returns: number

Example:

```less

tan(1) // tangent of 1 radian
tan(1deg) // tangent of 1 degree
tan(1grad) // tangent of 1 gradian
```

Output:

```css
1.5574077246549023   // tangent of 1 radian
0.017455064928217585 // tangent of 1 degree
0.015709255323664916 // tangent of 1 gradian
```

## atan

- 正切逆运算

- Returns number in radians e.g. a number between -π/2 and π/2.

- Parameters: number - 浮点数

- Returns: number

Example:

```less
atan(-1.5574077246549023)
atan(0)
round(atan(22), 6) // arctangent of 22 rounded to 6 decimal places
```

Output:

```css
-1rad
0rad
1.525373rad;
```

## pi

- `π`
- Returns π (pi);

- Parameters: none

- Returns: number

Example:

```
pi()
```

Output:

```
3.141592653589793
```

## pow

Returns the value of the first argument raised to the power of the second argument.

Returned value has the same dimension as the first parameter and the dimension of the second parameter is ignored.

Parameters:

number: base -浮点数
number: exponent - 浮点数
Returns: number

Example:

pow(0cm, 0px)
pow(25, -2)
pow(25, 0.5)
pow(-25, 0.5)
pow(-25%, -0.5)
Output:

1cm
0.0016
5
NaN
NaN%

## mod

- 模运算
- 参数:
  - number: 浮点数.
  - number: 浮点数
- Returns: number

Example:

```less
mod(0cm, 0px)
mod(11cm, 6px);
mod(-26%, -5);
```

Output:

```css
NaNcm;
5cm
-1%;
```

## min

- 最小值

- Example: `min(5, 10);`

- Output: `5`

- Example: `min(3px, 42px, 1px, 16px);`

- Output: `1px`

## max

- 最大值

- Example: `max(5, 10)`

- Output: `10`

- Example: `max(3%, 42%, 1%, 16%)`

- Output: `42%`
