# Ravin Cryptosystem

This is crypto challenge #6 of LACTF 2023. Understanding this writeup requires a relatively basic understanding of modulus and python. 

## fastpow
First we address what exactly the fastpow function does. It's name and code suggests that it returns $b^p \pmod{m}$. However, some simple experimentation reveals this is the not the case. For example, $\text{fastpow}(2, 5, 7)$ returns $2$, when it should return $4$. Note that $2^4 \equiv 2 \pmod{4}$, and $\text{fastpow}(2, 4, 7)$ does indeed return $2$. 

This suggests that $\text{fastpow}$ returns $b^{p-1} \pmod {m}$ for odd $p$. It's easy to see why this case by analyzing the $\text{fastpow}$ code. It works off the fact that if $p = (x_{1}x_{2}\cdots x_{n})\_2$, then $p \equiv b^{x_n} \cdot (b^2)^{x_{n-1}}  \cdots  (b^{2^n})^{x_1} \pmod {m}$.

The fastpow code goes through each of the binary digits right to left and adds each corresponding value in the sum, except it always ignores the the rightmost digit. Thus, when $p$ is odd, the return value is $\equiv b^{p-1} \pmod{m}$.

## introducing the problem

First of all, two random large prime numbers $p$ and $q$ are selected and $n$ is set to $p \cdot q$. The code we focus on next is ``c = fastpow(m, e, n)`` (line 19). Since $e = 65537$, an odd number, we see that $c$ gets set to $m ^ {65536} \pmod{n}$. $m$ is of course the cipher text, and we are given $n$, $e$ and $c$, so we essentially need to solve the modular equation $m ^ {65536} \equiv c \pmod{n}$. 

## plug and chug
We would like to plug this equation into Wolfram Alpha to solve it. However, $n$ is too large, so we prime factorize $n$ into $p \cdot q$. I used [this](https://www.dcode.fr/prime-factors-decomposition) website to find that $p = 861346721469213227608792923571$ and $q	 = 1157379696919172022755244871343$. 

Now, we solve $m ^ {65536} \equiv c \pmod{p}$ and $m ^ {65536} \equiv c \pmod{q}$ with Wolfram Alpha and get that $m \equiv x_1, x_2 \pmod{p}$, and $m \equiv y_1, y_2 \pmod{q}$, where $x_1 = 36107698842330390085620749377$, $x_2 = 825239022626882837523172174194$, $y_1 = 365505756060410753955985777171$ and $y_2 =791873940858761268799259094172$. 

 The Chinese remainder theorem guarantees one solution $\pmod{n}$ for each residue pair $\pmod{p}$ and $\pmod{q}$ and there are $2 \cdot 2 = 4$ pairs, so there are $4$ solutions for $m \pmod{n}$. 

Unfortunately, $p$ and $q$ are too large for WolframAlpha to solve the equations, so we can write our own script to solve the equations. This is the script I used during the contest.

```python
# function for extended Euclidean Algorithm
def  gcdExtended(a, b):
	# Base Case
	if a ==  0 :
		return b,0,1
	
	gcd,x1,y1 =  gcdExtended(b%a, a)
	# Update x and y using results of recursive call
	x = y1 - (b//a) * x1
	y = x1
	return gcd,x,y

  
p =  861346721469213227608792923571

q =  1157379696919172022755244871343

y, s, t =  gcdExtended(p, q)

n = p * q

  
# these are the residues that work

r1 =  825239022626882837523172174194
r2 =  365505756060410753955985777171

  
# z is the residue mod n
z = p*s*r2+q*t*r1

z = z % n

print(z)

w = z.to_bytes(length=30, byteorder='big')

print(w)
```

The output was:
```
40549930713646101196507797105878967309586158452159869
b'\x00\x00\x00\x00\x00\x00\x00\x00lactf{g@rbl3d_r6v1ng5}'
```
and just like that we have our flag, **lactf{g@rbl3d_r6v1ng5}**.

