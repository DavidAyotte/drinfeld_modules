# Drinfeld Modular Forms

This SageMath package provides an implementation for computing with Drinfeld modular forms for `GL_r(A)`.

To install this package, first clone this repository and then run the following command (inside the project folder):

`sage -pip install --upgrade --no-index -v .`

If there is any changes to the current repo, you will then simply need to pull the changes and run the above command again.

Next, to use its functionalities in your SageMath session you just need to type:

`sage: from drinfeld_modular_forms import *` (this will import everything).

To import a single functionality, for example the class `DrinfeldModularFormsRing`, type:

`sage: from drinfeld_modular_forms import DrinfeldModularFormsRing`

## Examples:

One may create the ring of Drinfeld modular forms:

```
    sage: from drinfeld_modular_forms import DrinfeldModularFormsRing
    sage: A = GF(3)['T']; K = Frac(A); T = K.gen()
    sage: M = DrinfeldModularFormsRing(K, 2)
    sage: M.ngens()  # number of generators
    2
```

The elements of this ring are viewed as multivariate polynomials in a choice of generators for the ring. The current implemented generators are the coefficient forms of a universal Drinfeld module over the Drinfeld period domain (see theorem 17.5 in \[1\]). In the computation below, the forms `g0` and `g1` corresponds to the weight `q - 1` Eisenstein series and the Drinfeld modular discriminant respectively.
```
    sage: g0, g1 = M.gens()
    sage: F = (g0 + g1)*g0; F
    g0*g1 + g0^2
```
Note that elements formed with polynomial relations `g0` and `g1` may not be homogeneous in the weight and may not define a Drinfeld modular form. We will call elements of this ring *graded Drinfeld modular forms*.

In the case of rank 2, one can compute the expansion at infinity of any graded form:

```
    sage: g0.expansion()
    1 + ((2*T^3+T)*t^2) + O(t^7)
    sage: g1.exansion()
    t^2 + 2*t^6 + O(t^8)
    sage: ((g0 + g1)*g0).expansion()
    1 + ((T^3+2*T+1)*t^2) + ((T^6+T^4+2*T^3+T^2+T)*t^4) + 2*t^6 + O(t^7)
```
This is achieved via the `A`-expansion theory developed by López-Petrov in \[3\] and \[4\]. We note that the returned expansion is a lazy power series. This means that it will compute on demands any coefficient up to any precision:
```
    sage: g1[600]  # 600-th coefficient
    T^297 + 2*T^279 + T^273 + T^271 + T^261 + 2*T^253 + T^249 + 2*T^243 + 2*T^171 + T^163 + T^153 + 2*T^147 + 2*T^145 + T^139 + T^135 + T^129 + 2*T^123 + 2*T^121 + T^117 + T^115 + T^111 + 2*T^109 + T^105 + 2*T^99 + 2*T^97 + T^93 + T^91 + T^87 + 2*T^85 + T^81 + 2*T^75 + T^69 + T^67 + T^63 + 2*T^61 + 2*T^51 + 2*T^45 + T^43 + T^39 + T^29 + T^27 + 2*T^21 + T^19 + T^13 + 2*T^11 + T^9 + T^7 + 2*T^3 + 2*T
```

In rank 2, it is also possible to compute the normalized Eisenstein series of weight `q^k - 1` (see (6.9) in \[2\]):

```
    sage: from drinfeld_modular_forms import DrinfeldModularFormsRing
    sage: q = 3
    sage: A = GF(q)['T']; K = Frac(A); T = K.gen()
    sage: M = DrinfeldModularFormsRing(K, 2)
    sage: M.eisenstein_series(q^3 - 1)  # weight q^3 - 1
    g0^13 + (-T^9 + T)*g0*g1^3
```

## Note:

Drinfeld modules are currently being implemented in SageMath. See https://github.com/sagemath/sage/pull/350263. As of March 2023, this PR is merged in the current latest development version of SageMath.


This package is still in development and some parts of the code is
based on the initial implementation of Alex Petrov located here:

`petrov/AlexPetrov-original-code-drinfeld-modular-forms.sage`.

## Further Developments:

* Add Hecke operators computations.
* enhance Goss polynomials

## References:

* \[1\] Basson D., Breuer F., Pink R., Drinfeld modular forms of arbitrary rank, Part III: Examples, https://arxiv.org/abs/1805.12339
* \[2\] Gekeler, E.-U., On the coefficients of Drinfelʹd modular forms. Invent. Math. 93 (1988), no. 3, 667–700
* \[3\] López, B. A non-standard Fourier expansion for the Drinfeld discriminant function. Arch. Math. 95, 143–150 (2010). https://doi.org/10.1007/s00013-010-0148-7
* \[4\] Petrov A., A-expansions of Drinfeld modular forms. J. Number Theory 133 (2013), no. 7, 2247–2266
