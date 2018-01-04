# StatsLib &nbsp; [![Build Status](https://travis-ci.org/kthohr/stats.svg?branch=master)](https://travis-ci.org/kthohr/stats) [![Coverage Status](https://codecov.io/github/kthohr/stats/coverage.svg?branch=master)](https://codecov.io/github/kthohr/stats?branch=master)

StatsLib is a templated C++ library for fast computation of statistical distribution functions.

Features:
* Compile-time (or run-time) evaluation of density functions, cumulative distribution functions, and quantile functions.
* Effective use of C++11 constexpr functions with the [GCE-Math library](https://github.com/kthohr/gcem).
* Simple, R-like syntax.
* Optional vector-matrix functionality is built on the [Armadillo C++ linear algebra library](http://arma.sourceforge.net/).

## Distributons

cdf, pdf, quantile, and random variable generation are available for the following distributions:

* Bernoulli
* Beta
* Binomial
* Cauchy
* Chi-squared
* Exponential
* Gamma
* Inverse-Gamma
* Laplace
* Logistic
* Log-Normal
* Normal (Gaussian)
* Student's t
* Uniform

pdf and random variable generation only:

* inverse-Wishart
* Multivariate Normal
* Wishart

## Compiler Options

For inline functionality only (no constexpr flags) use
```cpp
#define STATS_GO_INLINE
```

To remove any Armadillo-related functionality use
```cpp
#define STATS_NO_ARMA
```

## Examples

Functions are called using a clean R-like syntax.

```cpp
// evaluate the normal PDF at x = 1, mu = 0, sigma = 1
double dval_1 = stats::dnorm(1.0,0.0,1.0)
 
// evaluate the normal PDF at x = 1, mu = 0, sigma = 1, and return the log value
double dval_2 = stats::dnorm(1.0,0.0,1.0,true)
 
// evaluate the normal CDF at x = 1, mu = 0, sigma = 1
double pval_1 = stats::pnorm(1.0,0.0,1.0)
 
// evaluate the Laplacian quantile at p = 0.1, mu = 0, sigma = 1
double qval_1 = stats::qlaplace(0.1,0.0,1.0)

// matrix output
arma::mat beta_rvs = stats::rbeta(100,100,3.0,2.0);
// matrix input
arma::mat beta_cdf_vals = stats::pbeta(beta_rvs,3.0,2.0);
```

## Compile-time computation

Compile-time features are enable using the ```constexpr``` specifier:
```cpp
#include "stats.hpp"

int main()
{
    
    constexpr double dens_1 = stats::dlaplace(1.0,1.0,2.0);  // answer = 0.25
    constexpr double prob_1 = stats::plaplace(1.0,1.0,2.0);  // answer = 0.5
    constexpr double quant_1 = stats::qlaplace(0.1,1.0,2.0); // answer = -2.218875...

    return 0;
}
```

```assembly
LCPI0_0:
	.quad	-4611193153885729483    ## double -2.2188758248682015
LCPI0_1:
	.quad	4602678819172646912     ## double 0.5
LCPI0_2:
	.quad	4598175219545276417     ## double 0.25000000000000006
	.section	__TEXT,__text,regular,pure_instructions
	.globl	_main
	.p2align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## BB#0:
	push	rbp
Lcfi0:
	.cfi_def_cfa_offset 16
Lcfi1:
	.cfi_offset rbp, -16
	mov	rbp, rsp
Lcfi2:
	.cfi_def_cfa_register rbp
	xor	eax, eax
	movsd	xmm0, qword ptr [rip + LCPI0_0] ## xmm0 = mem[0],zero
	movsd	xmm1, qword ptr [rip + LCPI0_1] ## xmm1 = mem[0],zero
	movsd	xmm2, qword ptr [rip + LCPI0_2] ## xmm2 = mem[0],zero
	mov	dword ptr [rbp - 4], 0
	movsd	qword ptr [rbp - 16], xmm2
	movsd	qword ptr [rbp - 24], xmm1
	movsd	qword ptr [rbp - 32], xmm0
	pop	rbp
	ret
	.cfi_endproc
```

## Author

Keith O'Hara

## License

GPL (>= 2)
