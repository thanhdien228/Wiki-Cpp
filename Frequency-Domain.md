# Spectrum<br>
### What is Spectrum?
The electromagnetic spectrum is the range of all possible frequencies of electromagnetic radiation. It provides a way of categorizing the vast array of electromagnetic waves that occur in the universe, from the lowest frequencies used for radio communication to the highest frequencies that ionize atoms.

Radio waves have have frequencies from 300 GHz to as low as 3 kHz, and corresponding wavelengths from 1 millimeter to 100 kilometers.

### How are time domain & frequency domain performed?
<p align="center"><img src="https://phys.libretexts.org/@api/deki/files/12249/figure-2025-03-01a.jpg?revision=1&size=bestfit&width=651&height=245"/></p>

The electromagnetic spectrum, showing the major categories of electromagnetic waves. The range of frequencies and wavelengths is remarkable. The dividing line between some categories is distinct, whereas other categories overlap. Microwaves encompass the high frequency portion of the radio section of the EM spectrum.

### What is electromagnetic spectrum used for?

The electromagnetic spectrum is the collection of all electromagnetic radiation in the universe, which is a type of energy that pervades the cosmos in the form of electric and magnetic waves, allowing for the transfer of energy and information

# Fourier series

### What is Fourier series?
Definition:
A Fourier series is a way to represent a periodic function as a sum of sine and cosine functions, or equivalently, as a sum of complex exponentials, each with different frequencies and amplitudes. <br>
<p align="center"><img src="https://pysdr.org/_images/summing_sinusoids.svg"/></p>

### How is Fourier series performed?

<p align="center"><img src="https://pysdr.org/_images/fourier_series_triangle.gif"/></p>

The Fourier series of a function f(x) is given by <br>
$f(x) = a_{0}+\sum_{n = 1}^{\infty} a_{n}\cos(nx)+\sum_{n = 1}^{\infty}b_{n}\sin(nx) .$

**where,**

$a_{0} = \frac{1}{2\pi}\int_{-\pi}^{\pi}f(x)dx$ (i)

$a_{n} = \frac{1}{\pi}\int_{-\pi}^{\pi}f(x)\cos(nx) dx$,&nbsp;&nbsp; $n > 0$ (ii)

$b_{n} = \frac{1}{\pi}\int_{-\pi}^{\pi}f(x)\sin(nx) dx$,&nbsp;&nbsp; $n > 0$ (iii)

proof:

$\int_{-\pi}^{\pi}f(x)dx = \int_{-\pi}^{\pi}a_{0}dx + \sum_{n = 1}^{\infty}\int_{-\pi}^{\pi}a_{n}\cos(nx)dx + \sum_{n = 1}^{\infty}\int_{-\pi}^{\pi}b_{n}\sin(nx)dx $ <br>
$\int_{-\pi}^{\pi}f(x)dx = 2\pi a_{0} + 0 + 0 $ (i)<br>

for $a_{n}$:<br>
let $m$ &nbsp; $\epsilon$ &nbsp;  $\Zeta^+$ <br>
$\int_{-\pi}^{\pi}f(x)\cos(mx)dx = \int_{-\pi}^{\pi}a_{0}\cos(mx)dx + \sum_{n = 1}^{\infty}\int_{-\pi}^{\pi}a_{n}\cos(nx)\cos(mx)dx + \sum_{n = 1}^{\infty}\int_{-\pi}^{\pi}b_{n}\sin(nx)\cos(mx)dx $ <br>
if $n = m$: <br>
$\int_{-\pi}^{\pi}\cos(nx)\cos(mx)dx = \pi$<br>
else:<br>
$\int_{-\pi}^{\pi}\cos(nx)\cos(mx)dx = 0 $<br>
$\rightarrow \int_{-\pi}^{\pi}f(x)\cos(mx)dx = \int_{-\pi}^{\pi}a_{n}\cos(nx)\cos(mx)dx$ (ii)<br>

for $b_{n}$:<br>
let $m$ &nbsp; $\epsilon$ &nbsp;  $\Zeta^+$ <br>
$\int_{-\pi}^{\pi}f(x)\sin(mx)dx = \int_{-\pi}^{\pi}a_{0}\sin(mx)dx + \sum_{n = 1}^{\infty}\int_{-\pi}^{\pi}a_{n}\sin(nx)\cos(mx)dx + \sum_{n = 1}^{\infty}\int_{-\pi}^{\pi}b_{n}\sin(nx)\sin(mx)dx $ <br>
if $n = m$: <br>
$\int_{-\pi}^{\pi}\sin(nx)\sin(mx)dx = \pi$<br>
else:<br>
$\int_{-\pi}^{\pi}\sin(nx)\sin(mx)dx = 0 $<br>
$\rightarrow \int_{-\pi}^{\pi}f(x)\sin(mx)dx = \int_{-\pi}^{\pi}a_{n}\sin(nx)\sin(mx)dx$ (iii)<br>

### What is Fourier series used for?
Fourier series to the non-periodic functions, which helps us to view any functions in terms of the sum of simple sinusoids.

Fourier Series is used
- to solve various functions and find its integral and differential.
- in 3-D Graph Modelling
- to draw graph of various functions.
- in study of Complex function in Statistics, Astronomy, Biology and others, etc.

# Time domain & frequency domain

### What are time domain & frequency domain?

The time domain demonstrates how a signal changes over time. <br>
The frequency domain displays how the signal's energy is distributed over a range of frequencies.<br>
Quick changes in time domain result in many frequencies occurring.<br>
When there is no frequency, the frequency domain is 0 Hz.The frequency domain is not going to be "empty" because that only happens when there is no signal present (i.e., time domain of 0's). We call 0 Hz in the frequency domain "DC", because it's caused by a DC signal in time (a constant signal that doesn't change).

#### Properties

If we do ____ to our time domain signal, then ____ happens to our frequency domain signal.<br><br>

### How are time domain & frequency domain performed?

The Fourier transform converts the function's time-domain representation, shown in red, to the function's frequency-domain representation, shown in blue. The component frequencies, spread across the frequency spectrum, are represented as peaks in the frequency domain.

<p align="center"><img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSJDsvDZMJRzY9jWNGavbmNSJuqCT9ICXlz1w&s"/></p>


# Fourier Transform <br>

### What is Fourier Transform?

Fourier Transform is a mathematical model that helps to transform signals between two different domains, such as transforming signal from frequency domain to time domain.

### How are Fourier Transform performed?

The time domain to the frequency domain and back is called the Fourier Transform. It is defined as follows:

$X(f) = \int x(t)e^{- 2 \pi j f t} dt$

Note the "t" for time, and "f" for frequency. The "j" is simply the imaginary unit. You may have seen it as "i" in high school math class. We use "j" in engineering and computer science because "i" is often referring to current, and in programming it's often used as an iterator.

To return to the time domain from frequency is almost the same, aside from a scaling factor and negative sign:

$x(t) = \frac{1}{2 \pi} \int X(f)e^{2 \pi j f t} df$

w is angular frequency in radians per second, while f is in Hz. All you have to know is that:

$w= 2 \pi f $

Even though it adds a $2 \pi$ term to many equations, it's easier to stick with frequency in Hz. Ultimately you will work with Hz in your SDR application.

The above equation for the Fourier Transform is the continuous form, which you will only see in math problems. The discrete form is much closer to what is implemented in code:

$X_k = \sum_{n = 0}^{N-1} x_n e ^ - \frac{2 \pi j k n}{N} $

Note that the main difference is we replaced the integral with a summation. The index k goes from 0 to N-1.

# Negative Frequencies<br>

### What is Negative Frequencies?

Negative frequency have to do with using complex numbers (imaginary numbers)-there isn't really such thing as a "negative frequency" when it comes to transmitting/receiving RF signals, it's just a representation we use. Here's an intuitive way to think about it.

<p align="center"><img src="https://pysdr.org/_images/negative-frequencies2.svg"/></p>
Now, when the SDR gives us the samples, it will appear like this:<br><br>
<p align="center"><img src="https://pysdr.org/_images/negative-frequencies3.svg"/></p>

Remember that we tuned the SDR to 100 MHz. So the signal that was at about 97.5 MHz shows up at -2.5 MHz when we represent it digitally, which is technically a negative frequency. In reality it's just a frequency lower than the center frequency.

# Time-Frequency Properties

**Linearity Property**<br>
$a x(t) + b y(t) \leftrightarrow a X(f) + b Y(f)$ <br>

If we add two signals in time, then the frequency domain version will also be the two frequency domain signals added together. It also tells us that if we multiply either one by a scaling factor, the frequency domain will also scale by the same amount. <br>

**Frequency Shift Property**<br>
if $x(t)\leftrightarrow X(f) $

then $e^{2 \pi j f_0 t}x(t) \leftrightarrow X(f-f_0)$

Proof:

Fourier transform $[x(t)e^{2 \pi j f_0 t}] = \int_{-\infty}^{\infty}[x(t)e^{-2 \pi j f t}]e^{ 2 \pi j f_0 t}dt = \int_{-\infty}^{\infty} x(t)e^{- 2 \pi j (f-f_0) t}dt = X(f-f_0)$ 

This property tells us that if we take a signal *x(t)* and multiply it by a sine wave, then in the frequency domain we get *X(f)* except shifted by a certain frequency, *$f_0$*.
<p align="center"><img src="https://pysdr.org/_images/freq-shift.svg"/></p>

**Scaling in Time Property**<br>
$x(at) \leftrightarrow \frac{1}{|a|} X\left(\frac{f}{\pm a}\right)$ from ***(i)*** and ***(ii)***

Proof:<br>
Fourier transform $[x(at)]= \int_{-\infty}^{\infty} x(at)e^{-j 2 \pi f t} dt $

Because `a` can be a positive number and negative number so:<br>
***(i)*** For the posetive `a`:<br>
Let:<br>
$ (at = \tau \rightarrow t = \frac{\tau}{a} \rightarrow dt = \frac{1}{a}d \tau) $
this lead to 
$ (t \rightarrow \infty , \tau \rightarrow \infty) $ 
and 
$ (t \rightarrow -\infty , \tau \rightarrow -\infty) $<br>
Fourier transform $[x(at)] = \int_{-\infty}^{\infty} x(at)e^{-j 2 \pi \frac{f}{a} \tau} dt $ <br>
Fourier transform $[x(at)] = \frac{1}{a} \int_{-\infty}^{\infty} x(a\tau)e^{-j 2 \pi \frac{f}{a} \tau} d\tau $ <br>
Fourier transform $[x(at)] = \frac{1}{a} X(\frac{f}{a})$

***(ii)*** For the negative `a`:<br>
Let: <br>
$ (-at = \tau \rightarrow t = -\frac{\tau}{a} \rightarrow dt = -\frac{1}{a}d \tau) $
this lead to 
$ (t \rightarrow \infty , \tau \rightarrow -\infty) $ 
and
$ (t \rightarrow -\infty , \tau \rightarrow \infty) $<br>
Fourier transform $[x(-at)] = \int_{-\infty}^{\infty} x(-at)e^{-j 2 \pi \frac{f}{a} \tau} dt $ <br>
Fourier transform $[x(-at)] = -\frac{1}{a} \int_{\infty}^{-\infty} x(-a\tau)e^{-j 2 \pi (-\frac{f}{a}) \tau} d\tau $ <br>
Fourier transform $[x(-at)] = \frac{1}{a} \int_{-\infty}^{\infty} x(-a\tau)e^{-j 2 \pi (-\frac{f}{a}) \tau} d\tau $ <br>
Fourier transform $[x(-at)] = \frac{1}{a} X(-\frac{f}{a})$

The scaling in the time domain causes inverse scaling in the frequency domain.
<p align="center"><img src="https://pysdr.org/_images/time-scaling.svg"/></p>

**Convolution in Time Property**<br>
$\int x(\tau) y(t-\tau) d\tau \leftrightarrow X(f)Y(f)$<br>
${x(t)*y(t)=\int_{-\infty}^{\infty}x(\tau)y(t-\tau)d\tau}$<br>
${X(f)=F[x(t)*y(t)]=\int_{-\infty}^{\infty}[x(t)*y(t)]e^{-j 2\pi f t}dt}$<br>
${\Rightarrow F[x(t)*y(t)]=\int_{-\infty}^{\infty}[\int_{-\infty}^{\infty}x(\tau)y(t-\tau)d\tau]e^{-j 2 \pi f t}dt }$<br>
By interchanging the order of integration, we get,
${\Rightarrow F[x(t)*y(t)]=\int_{-\infty}^{\infty}x(\tau)[\int_{-\infty}^{\infty}Y(t-\tau)e^{-j 2 \pi f t}dt]d\tau }$

When you convolve two signals, it's like creating a new third signal from them.<br>
x(t) is our received signal. Let Y(f) be the mask we want to apply in the frequency domain.<br>
y(t) is the time domain representation of our mask, and if we convolve it with x(t), we can "filter out" the signal we don't want.

${t = (u + \tau)\: and\: dt = du}$<br>
${\therefore F[x(t)*y(t)]=\int_{-\infty}^{\infty}x(\tau)[\int_{-\infty}^{\infty}Y(u)e^{-j 2 \pi f (u+\tau)}du]d\tau}$<br>
${\Rightarrow F[x(t)*y(t)]=\int_{-\infty}^{\infty}x(\tau)[\int_{-\infty}^{\infty}Y(u)e^{-j 2 \pi f u}du]e^{-jf \tau}d\tau}$<br>
${\Rightarrow F[x(t)*y(t)]=\int_{-\infty}^{\infty}x(\tau)y(f)e^{-jf\tau}d\tau.}$<br>
${\Rightarrow F[x(t)*y(t)]=[\int_{-\infty}^{\infty}X(\tau)e^{-jf \tau}d\tau]Y(f)-X(f).Y(f)}$<br>
${\therefore F[x(t)*y(t)]=x(f).y(f)}$<br>
Or, it can also be represented as,<br>
${x(t)*y(t)\overset{FT}{\leftrightarrow}x(f).y(f)}$

<p align="center"><img src="https://pysdr.org/_images/tikz-649a79a4e38a50b56f209ae74eca2e3aef3f17e4.png"/></p>

# Reference
- [Frequency Domain](https://pysdr.org/content/frequency_domain.html#)
- [Fourier Series Formula](https://www.geeksforgeeks.org/fourier-series/)
