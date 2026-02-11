---
title: "Numeric Precision: What It Is, and Why It Matters"
author: M Cooper Healy
date: 2026-02-06
---

At Nabis, numeric precision isn’t an academic footnote — it’s a core reliability requirement. Every price we display, every revenue share we calculate, and every promotion we tailor relies on numbers that must be consistent, accurate, and trustworthy throughout complex pipelines. Even tiny errors in representation or rounding can cascade into incorrect pricing, misaligned incentives, and ultimately a poor partner or customer experience. Getting number precision right protects trust, avoids bugs that are painful to debug, and ensures our platform scales with the financial rigor our brands and retailers depend on.

# What's the problem?

As a thought exercise, take the following calculation:

$$\frac{1}{3} + \frac{2}{3} = 1$$

This is trivial with fractions, but becomes a bit odd-looking with decimals.

$$0.333... + 0.666... = 0.999... = 1$$

So far, no issues. However, we are relying heavily on 'repeating decimals' in order to represent these fractions correctly as decimal numbers[^2]. Let's explore what happens if we are limited in how many digits we can represent. For this experiment, let's pick five significant digits:

$$0.33333 + 0.66666 = 0.99999 \neq 1$$

This is a contrived example, but illustrates the point that in any given base (in this case base 10), there are numbers that can only be represented with infinite digits.

## A Surprising Calculation

Let's take a moment to make a trivial calculation: adding one tenth to two tenths:

```js
> 0.1 + 0.2
0.30000000000000004
```

What happened here?

Just like our earlier example of adding one third to two thirds,
we are running into an issue with a number that requires an infinite number of digits to be stored properly in binary.
The binary `float32` encoding of `0.1` is `00111101110011001100110011001101`, which equals $0.100000001490116119384765625$.

This error is really really small, but repeated calculations can begin to cause issues:

```js
> total = 0.1
0.1
> for (let i=0; i < 1000; i++) total += 0.1;
100.09999999999859
```

By adding 10 cents at a time, $\$100.00$ has become $\$100.09$, which is clearly an issue.

For many calculations, even this is well within the bounds of acceptability.
For financial calculations, which have legal ramifications and must be exact,
this isn't an option.

# What is a `number` anyway?

So why does this happen?

Javascript (and by extension TypeScript) use 64-bit floating point numbers as the backing type for number.
We will use 32-bit floating point numbers throughout this document for brevity and clarity.

The operant difference is that 64-bit floating point numbers have more precision.

As we have seen in recent bugs in our monetary calculations in NY, this precision is still not enough.

This may raise a further question as to what that means.

Floating point numbers are, essentially, scientific notation using binary numbers.

In the event that the reader is unfamiliar with the purpose and usage of scientific notation, the following section breaks down its main advantages.

# Scientific Notation

Scientific notation is often seen in STEM-related academic settings, and is of the form $6.022 \times 10^{23}$. (For those curious, this is Avogadro’s number, or the number of molecules present in a mole of gas at one atmosphere of pressure and 0°C) It is intended as a convenient way to show arbitrarily large or small numbers at a given, useful precision. In our example (Avogadro’s number), we have a precision of four significant digits, which is plenty for most chemical calculations. If we were to write this number in regular notation, it would take the following, borderline useless, form: $602200000000000000000000$

While this gives a more obvious view of the scale involved, it removes two valuable things:

1. Definite precision (are the zeros really zeros? This is actually an open question)
2. The ability to fit it in formulas (a 50-character formula is hard to parse, and harder still to keep the number of zeros straight)

Scientific notation gives us the ability to be precise about both the scale and the precision that we care about. In this case, we are saying that we explicitly only care about four significant digits (which is now the implicit upper bound for any calculation we do with this number), and that we are operating with a number in the sextillions.

## Exponent

The exponent simply refers to the exponent used in the second term of the number in scientific notation. In this case, $6.022 \times 10^{23}$ has an exponent of $23$.

## Mantissa

The mantissa can be essentially thought of as the leading number in the scientific notation seen earlier. For example, in $6.022 \times 10^{23}$, the mantissa is $.022$. The six is omitted, because mantissa is the name specifically of the portion of the number that follows the decimal point. This may seem useless, since the six is rather important to the value of the number, in this case. This is true! However, if we operate in binary, the usefulness of this distinction becomes clear.

# The Representation of Floating Point Numbers

Since the number will be represented in binary, let’s do that conversion now:
$$01100110111111110000101010101000$$
This is (likely) incredibly unhelpful.

To break this down into a more useful format, let’s separate the parts of the number:

`0 11001101 11111110000101010101000`

## The Sign

The first number here, `0`, is the sign. This should be familiar from any previous experience with signed numbers in binary format. For those unfamiliar, `0` indicates positive, `1` indicates negative.

## The Exponent

The next segment of the number is the exponent: `11001101`

This evaluates to the decimal number $205$. According to the IEEE-754 standard, we subtract $127$ to get $78$, which tells us that the target number is $M \times 2^78$, where $M$ is our mantissa (which we have not yet encountered).

## The Mantissa

Following this, we get the final twenty-three bits, which form the mantissa:

`11111110000101010101000`

This can be seen as a binary-equivalent of the mantissa format we’ve seen earler. Note that the mantissa only includes digits present _after_ the point. In binary, We can assume that the digit _before_ the point is _always_ $1$, since it can only be either $1$ or $0$, and a leading $0$ could be expressed by shifting the entire number left and decreasing the exponent.

Thus, we can envision the number as follows (using a ficitonal binary-point notation):[^1]

$$1.11111110000101010101000$$

Which can be expressed as:

$$1 + \frac{1}{2} + \frac{1}{4} + \frac{1}{8}... = 1.9925127029418945$$

## Putting It All Together

This gives us a final floating point value of:
$$1.9925127029418945 \times 2^{78}$$

Quite simply, this number is wrong.

The above calculates out as follows:
$1.9925127029418945 \times 2^{78} = 602200013124147498450944 \neq 602200000000000000000000 = 6.022 \times 10^{23}$$

# What Are The Alternatives?

So if floating point numbers aren't an option for financial calculations, what _are_ the options?

## Binary Coded Decimal

This section is under construction, since BCD is not a planned solution to the problem, but may be an informative digression.

## Fixed Point Numbers

Fixed point numbers are essentially integers, with an implicit decimal point.

This is commonly encountered in the form of using integer values of cents to represent money, which is a fixed point number with two decimals of precision.

```ts
class Currency {
  cents: number;

  constructor(cents: number) {
    this.cents = Math.floor(cents); // remove any partial cents
  }

  add(other: Currency) {
    this.cents += other.cents;
    return this;
  }

  subtract(other: Currency) {
    this.cents -= other.cents;
    return this;
  }

  toNumber() {
    return this.cents / 100;
  }

  toString() {
    const sign = this.cents < 0 ? "-" : "";
    const dollars = Math.abs(this.cents / 100);
    const formattedDollars = dollars.toLocaleString("en-us", {
      minimumFractionDigits: 2,
    });
    return `${sign}\$${formattedDollars}`;
  }
}
```

## Maximum Size

Since the backing type of a fixed point number is an integer, the maximum value that can be represented is the maximum value of an integer divided by $10$ raised to the power of the number of digits of precision chosen, given in the following formula:

$$18446744073709551615 \div 10^n$$

In the case of two digits of precision (cent-level precision), this means that the largest dollar value representable is $\$184,467,440,737,095,516.15$, or approximately $184$ quadrillion dollars.

## Fixed Precision

As would be expected, fixed point numbers have a fixed precision, and cannot store values with more granularity than the precision chosen.

In the case of cents, it means that the result of a calculation with some fractional cent component will need to be rounded to the nearest cent immediately.

It is for this reason that a higher precision is gererally recommended, so as to not incur constant rounding throughout calculations, as each intermediate value is rounded to the nearest cent, which can allow error to accrue. (Much of the finance industry attempts to account for this error with banker's rounding[^3], which keeps rounding error from tending either up or down, but does not return the lost precision).

## Complications with Multiplying and Dividing

It may have been noticed that the example class above only includes methods for adding and subtracting, but no multiplicative operations.

This is because if we were to just blindly multiply the underlying integers together, absurdity would ensue:

$$\$5.00 \times \$5.00 = 500¢ \times 500¢ = 250000 = \$2,500.00$$

This is not actually the issue it appears to be, as you never actually multiply currency by other currency.

## Brief Digression Regarding Units

In the above examble, where we multiplied $\$5.00$ by $\$5.00$, we would actually expect a result in square-dollars, or $\$^2$. This notation does not really exist, because it is not actually a concept.

In no cases is money ever multiplied by other money. In most cases, money is multiplied by a rate, for example one of the following:

$$5 \frac{\$}{\text{hr}} \times 5\text{hrs} = \$25.00$$
$$\$4.00 \times 9\% \text{tax} = \$4.0 \times 1.09 = \$5.45$$

Since all we need to do is ensure that the operand of our multiplication or division is a scalar, and not currency, we can implement the missing methods accordingly.

## Implementing Multiplication and Division

```ts
class Currency {
...
    multiply(other: number) {
        const rawResult = this.cents * other;
        this.cents = Math.floor(rawResult);
        return this;
    }

    divide(other: number) {
        const rawResult = this.cents / other;
        this.cents = Math.floor(rawResult);
        return this;
    }
}
```

Note that after each (implicitly floating-point) operation, we must re-coerce our result into the integer we expect for cent values.

# The Dinero Library

The `dinero` library provides a `Dinero` class that more or less works like the example code listing shown here, albiet with much more defensive code and optimizations. This allows us to easily convert into fixed point numbers and use their precision for all of our calculations, without having to worry about floating point error, and without having to implement our own fixed-point number type.

## Problematic Use

### Insufficient Precision

The default precision for `dinero` is cent-level precision.
This means that the result is rounded to the nearest cent upon every mathematical calculation, which, while often sufficient, can start to show odd results after repeated calculations.

### Constant Conversions

Currently, there is a common pattern in our code of the following form:

```ts
const newNumber = convertFromDinero(getDineroUSDValue(someNumber));
```

This is taking `someNumber`, creating a fixed-point `Dinero` object, and then immediately converting from that `Dinero` object back into a `number`. This is essentially just rounding `someNumber` to the nearest cent, which does not provide any additional precision to `newNumber`, _and_ incurs precision loss from the implicit rounding.

This is also often done more complexly as

```ts
function someFunc(someNumber: number): number {
  const newDin = getDineroUSDValue(someNumber);
  const mathResult = newDin.Add(i).Mul(j).Add(k);
  return convertFromDinero(mathResult);
}
function someOtherFunc(someNumber: number): number {
  const newDin = getDineroUSDValue(someNumber);
  const mathResult = newDin.Add(a).Mul(b).Add(c);
  return convertFromDinero(mathResult);
}
// somewhere else
const num1 = someFunc(0.5 /*or something else arbitrary*/);
const num2 = someOtherFunc(num1);
```

Where a number is converted into a `Dinero` object solely for the purpose of performing some calculations, but then it is converted back to a number to be passed between different areas of the code.

If instead the dinero objects are passed through _all_ of the code in place of `number`s, precision loss and rounding erors could be avoided.

## Advice for Usage

### Higher Precision

If the precision on created `Dinero` objects is specified as, for example, 10, calculations are guaranteed a precision down to $$\frac{1}{1000,000,000}¢$$, or one hundred millionth of a cent.

Using the formula above, the largest monetary value representable by this precision is

$$18446744073709551615 \div 10^{10} = 1844674407.3709551615 \approx \$1,844,674,407.37$$

or a little less than 2 billion dollars.

### Minimizing Conversions

In addition to increasing precision, conversions must be minimized, to prevent rounding to the nearest cent between calculations, which would undermine all of that precision gain.

This can be accomplished very simply with the following two rules:

1.  Monetary values are converted to dinero objects immediately when entering the code (from either user input or the DB)
2.  Monetary values are converted to cent-rounded dollars only when about to be sent to a user.

[^1]:
    If you are curious to play around with this number format,
    I highly recommend the following resource:
    http://www.h-schmidt.net/FloatConverter/IEEE754.html

[^2]:
    If the equality $0.999.. = 1$ seems odd to you,
    consult the following guide,
    which is outside the scope of this article:
    https://en.wikipedia.org/wiki/0.999...

[^3]: https://wiki.c2.com/?BankersRounding
