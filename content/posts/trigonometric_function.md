---
title: Trigonometric Function 
description: 三角函数：正弦，余弦，正切，余切，正割，余割
---

**Hyperbolic Function:**

$\sinh x = -i \sin(ix) = \frac{e^x - e^{-x}}{2}$

$\cosh x = \cos(ix) = \frac{e^x + e^{-x}}{2}$

$\tanh x = -i \tan(ix) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$

$\coth x = i \cot(ix) = \frac{e^x + e^{-x}}{e^x - e^{-x}}$

$\operatorname{sech} x = \sec(ix) = \frac{2}{e^x + e^{-x}}$

$\operatorname{csch} x = i \csc(ix) = \frac{2}{e^x - e^{-x}}$

![](hyperbolic.svg)

**Inverse Hyperbolic Function:**

$\operatorname{arsinh} x = \ln(x + \sqrt{x^2 + 1})$

$\operatorname{arcosh} x = \ln(x + \sqrt{x^2 - 1})$

$\operatorname{artanh} x = \frac{1}{2}\ln(\frac{1+x}{1-x})$

$\operatorname{arcoth} x = \frac{1}{2}\ln(\frac{x+1}{x-1})$

$\operatorname{arsech} x = \ln(\frac{1}{x} + \sqrt{\frac{1}{x^2} - 1})$

$\operatorname{arcsch} x = \ln(\frac{1}{x} + \sqrt{\frac{1}{x^2} + 1})$

![](inverse.svg)

---

$(\sin x)^{\prime} = \cos x$

$(\cos x)^{\prime} = -\sin x$

$(\tan x)^{\prime} = \sec^2 x$

$(\cot x)^{\prime} = -\csc^2 x$

$(\sec x)^{\prime} = \sec x \tan x$

$(\csc x)^{\prime} = -\csc x \cot x$

$(\arcsin x)^{\prime} = \frac{1}{\sqrt{1-x^2}}$

$(\arccos x)^{\prime} = -\frac{1}{\sqrt{1-x^2}}$

$(\arctan x)^{\prime} = \frac{1}{1+x^2}$

$(\operatorname{arccot} x)^{\prime} = -\frac{1}{1+x^2}$

$(\operatorname{arcsec} x)^{\prime} = \frac{1}{|x|\sqrt{x^2-1}}$

$(\operatorname{arccsc} x)^{\prime} = -\frac{1}{|x|\sqrt{x^2-1}}$

$(\sinh x)^{\prime} = \cosh x$

$(\cosh x)^{\prime} = \sinh x$

$(\tanh x)^{\prime} = \operatorname{sech}^2 x$

$(\coth x)^{\prime} = -\operatorname{csch}^2 x$

$(\operatorname{sech} x)^{\prime} = -\operatorname{sech} x \tanh x$

$(\operatorname{csch} x)^{\prime} = -\operatorname{csch} x \coth x$

$(\operatorname{arsinh} x)^{\prime} = \frac{1}{\sqrt{x^2+1}}$

$(\operatorname{arcosh} x)^{\prime} = \frac{1}{\sqrt{x^2-1}}$

$(\operatorname{artanh} x)^{\prime} = \frac{1}{1-x^2}$

$(\operatorname{arcoth} x)^{\prime} = \frac{1}{1-x^2}$

$(\operatorname{arsech} x)^{\prime} = -\frac{1}{x\sqrt{1-x^2}}$

$(\operatorname{arcsch} x)^{\prime} = -\frac{1}{|x|\sqrt{1+x^2}}$

$\int \sin x \ dx = -\cos x + C$

$\int \cos x \ dx = \sin x + C$

$\int \tan x \ dx = -\ln |\cos x| + C$

$\int \cot x \ dx = \ln |\sin x| + C$

$\int \sec x \ dx = \ln |\sec x + \tan x| + C$

$\int \csc x \ dx = -\ln |\csc x + \cot x| + C$

$\int \arcsin x \ dx = x \arcsin x + \sqrt{1-x^2} + C$

$\int \arccos x \ dx = x \arccos x - \sqrt{1-x^2} + C$

$\int \arctan x \ dx = x \arctan x - \frac{1}{2} \ln(1+x^2) + C$

$\int \operatorname{arccot} x \ dx = x \operatorname{arccot} x + \frac{1}{2} \ln(1+x^2) + C$

$\int \operatorname{arcsec} x \ dx = x \operatorname{arcsec} x - \operatorname{arcosh} |x| + C$

$\int \operatorname{arccsc} x \ dx = x \operatorname{arccsc} x + \operatorname{arcosh} |x| + C$

$\int \sinh x \ dx = \cosh x + C$

$\int \cosh x \ dx = \sinh x + C$

$\int \tanh x \ dx = \ln(\cosh x) + C$

$\int \coth x \ dx = \ln|\sinh x| + C$

$\int \operatorname{sech} x \ dx = \arctan(\sinh x) + C$

$\int \operatorname{csch} x \ dx = -\ln|\operatorname{csch} x + \coth x| + C$

$\int \operatorname{arsinh} x \ dx = x \operatorname{arsinh} x - \sqrt{x^2+1} + C$

$\int \operatorname{arcosh} x \ dx = x \operatorname{arcosh} x - \sqrt{x^2-1} + C$

$\int \operatorname{artanh} x \ dx = x \operatorname{artanh} x + \frac{1}{2} \ln(1-x^2) + C$

$\int \operatorname{arcoth} x \ dx = x \operatorname{arcoth} x + \frac{1}{2} \ln(x^2-1) + C$

$\int \operatorname{arsech} x \ dx = x \operatorname{arsech} x + \arcsin x + C$

$\int \operatorname{arcsch} x \ dx = x \operatorname{arcsch} x + \operatorname{arsinh} |x| + C$
