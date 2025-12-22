# Calculus/Tables of Derivatives

## General Rules

$$\frac{d}{dx}(f+g)=\frac{df}{dx}+\frac{dg}{dx}$$

$$\frac{d}{dx}(c\cdot f)=c\cdot\frac{df}{dx}$$

$$\frac{d}{dx}(f\cdot g)=f\cdot\frac{dg}{dx}+g\cdot\frac{df}{dx}$$

$$\frac{d}{dx}\left(\frac{f}{g}\right)=\frac{-f\cdot\frac{dg}{dx}+g\cdot\frac{df}{dx}}{g^{2}}$$

$$\frac{d}{dx}[f(g(x))]=\frac{df}{dg}\cdot\frac{dg}{dx}=f^{\prime}(g(x))\cdot g^{\prime}(x)$$

$$\frac{d^{n}}{dx^{n}}f(x)g(x)=\sum_{i=0}^{n}\binom{n}{i}f^{(n-i)}(x)g^{(i)}(x)$$

$$\frac{d}{dx}\left(\frac{1}{f}\right)=-\frac{f^{\prime}}{f^{2}}$$

## Powers and Polynomials

$$\frac{d}{dx}(c)=0$$

$$\frac{d}{dx}x=1$$

$$\frac{d}{dx}x^{n}=nx^{n-1}$$

$$\frac{d}{dx}\sqrt{x}=\frac{1}{2\sqrt{x}}$$

$$\frac{d}{dx}\frac{1}{x}=-\frac{1}{x^{2}}$$

$$\frac{d}{dx}(c_{n}x^{n}+c_{n-1}x^{n-1}+\dots+c_{1}x+c_{0})=nc_{n}x^{n-1}+(n-1)c_{n-1}x^{n-2}+\dots+c_{1}$$

## Trigonometric Functions

$$\frac{d}{dx}\sin(x)=\cos(x)$$

$$\frac{d}{dx}\cos(x)=-\sin(x)$$

$$\frac{d}{dx}\tan(x)=\sec^{2}(x)$$

$$\frac{d}{dx}\cot(x)=-\csc^{2}(x)$$

$$\frac{d}{dx}\sec(x)=\sec(x)\tan(x)$$

$$\frac{d}{dx}\csc(x)=-\csc(x)\cot(x)$$

## Exponential and Logarithmic Functions

$$\frac{d}{dx}e^{x}=e^{x}$$

$$\frac{d}{dx}a^{x}=a^{x}\ln(a) \text{ if } a>0$$

$$\frac{d}{dx}\ln(x)=\frac{1}{x}$$

$$\frac{d}{dx}\log_{a}(x)=\frac{1}{x \ln(a)} \text{ if } a>0, a\ne1$$

$$\frac{d}{dx}(f^{g})=\frac{d}{dx}(e^{g \ln(f)})=f^{g}\left(f^{\prime}\frac{g}{f}+g^{\prime}\ln(f)\right), f>0$$

$$\frac{d}{dx}(c^{f})=\frac{d}{dx}(e^{f \ln(c)})=c^{f}\ln(c)\cdot f^{\prime}$$

## Inverse Trigonometric Functions

$$\frac{d}{dx}\arcsin(x)=\frac{1}{\sqrt{1-x^{2}}}$$

$$\frac{d}{dx}\arccos(x)=-\frac{1}{\sqrt{1-x^{2}}}$$

$$\frac{d}{dx}\arctan(x)=\frac{1}{x^{2}+1}$$

$$\frac{d}{dx}\text{arccot}(x)=-\frac{1}{x^{2}+1}$$

$$\frac{d}{dx}\text{arcsec}(x)=\frac{1}{|x|\sqrt{x^{2}-1}}$$

$$\frac{d}{dx}\text{arccsc}(x)=-\frac{1}{|x|\sqrt{x^{2}-1}}$$

## Hyperbolic and Inverse Hyperbolic Functions

$$\frac{d}{dx}\sinh(x)=\cosh(x)$$

$$\frac{d}{dx}\cosh(x)=\sinh(x)$$

$$\frac{d}{dx}\tanh(x)=\text{sech}^{2}(x)$$

$$\frac{d}{dx}\text{sech}(x)=-\tanh(x)\text{sech}(x)$$

$$\frac{d}{dx}\coth(x)=-\text{csch}^{2}(x)$$

$$\frac{d}{dx}\text{csch}(x)=-\coth(x)\text{csch}(x)$$

$$\frac{d}{dx}\text{arsinh}(x)=\frac{1}{\sqrt{x^{2}+1}}$$

$$\frac{d}{dx}\text{arcosh}(x)=\frac{1}{\sqrt{x^{2}-1}}, x>1$$

$$\frac{d}{dx}\text{artanh}(x)=\frac{1}{1-x^{2}}, |x|<1$$

$$\frac{d}{dx}\text{arsech}(x)=-\frac{1}{x\sqrt{1-x^{2}}}, 0<x<1$$

$$\frac{d}{dx}\text{arcoth}(x)=\frac{1}{1-x^{2}}, |x|>1$$

$$\frac{d}{dx}\text{arcsch}(x)=-\frac{1}{|x|\sqrt{1+x^{2}}}, x\ne0$$