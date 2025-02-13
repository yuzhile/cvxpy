.. _functions:

Functions
=========

This section of the tutorial describes the functions that can be applied
to CVXPY expressions. CVXPY uses the function information in this
section and the :ref:`DCP rules <dcp>` to mark expressions with a
sign and curvature.

Operators
---------

The infix operators ``+, -, *, /`` are treated as functions. ``+`` and
``-`` are affine functions. ``*`` and ``/`` are affine in
CVXPY because ``expr1*expr2`` is allowed only when one of the
expressions is constant and ``expr1/expr2`` is allowed only when
``expr2`` is a scalar constant.

Indexing and slicing
^^^^^^^^^^^^^^^^^^^^

All non-scalar expressions can be indexed using the syntax
``expr[i, j]``. Indexing is an affine function. The syntax ``expr[i]``
can be used as a shorthand for ``expr[i, 0]`` when ``expr`` is a column
vector. Similarly, ``expr[i]`` is shorthand for ``expr[0, i]`` when
``expr`` is a row vector.

Non-scalar expressions can also be sliced into using the standard Python
slicing syntax. For example, ``expr[i:j:k, r]`` selects every kth
element in column r of ``expr``, starting at row i and ending at row
j-1.

Transpose
^^^^^^^^^

The transpose of any expression can be obtained using the syntax
``expr.T``. Transpose is an affine function.

Power
^^^^^

For any CVXPY expression ``expr``,
the power operator ``expr**p`` is equivalent to
the function ``power(expr, p)``.

Scalar functions
----------------

A scalar function takes one or more scalars, vectors, or matrices as arguments
and returns a scalar.

.. |_| unicode:: 0xA0
   :trim:

.. list-table::
   :header-rows: 1

   * - Function
     - Meaning
     - Domain
     - Sign
     - Curvature |_|
     - Monotonicity

   * - entr(X)
     - :math:`\sum_{ij}-X_{ij} \log (X_{ij})`
     - :math:`X_{ij} > 0`
     - |unknown| unknown
     - |concave| concave
     - None

   * - :ref:`geo_mean(x) <geo_mean>`

       :ref:`geo_mean(x, p) <geo_mean>`

       :math:`p \in \mathbf{R}^n_{+}`

       :math:`p \neq 0`
     - :math:`x_1^{1/n} \cdots x_n^{1/n}`

       :math:`\left(x_1^{p_1} \cdots x_n^{p_n}\right)^{\frac{1}{\mathbf{1}^T p}}`
     - :math:`x \in \mathbf{R}^n_{+}`
     - |positive| positive
     - |concave| concave
     - |incr| incr.

   * - harmonic_mean(x)
     - :math:`\frac{n}{\frac{1}{x_1} + \cdots + \frac{1}{x_n}}`
     - :math:`x \in \mathbf{R}^n_{+}`
     - |positive| positive
     - |concave| concave
     - |incr| incr.

   * - kl_div(X, Y)
     - :math:`\sum_{ij} X_{ij} \log(X_{ij}/Y_{ij}) -X_{ij}+Y_{ij}`
     - :math:`X_{ij} > 0`

       :math:`Y_{ij} > 0`
     - |positive| positive
     - |convex| convex
     - None

   * - lambda_max(X)
     - :math:`\lambda_{\max}(X)`
     - :math:`X \in \mathbf{S}^n`
     - |unknown| unknown
     - |convex| convex
     - None

   * - lambda_min(X)
     - :math:`\lambda_{\min}(X)`
     - :math:`X \in \mathbf{S}^n`
     - |unknown| unknown
     - |concave| concave
     - None

   * - lambda_sum_largest(X, |_| k)

       :math:`k = 1,\ldots, n`
     - :math:`\text{sum of $k$ largest}\\ \text{eigenvalues of $X$}`
     - :math:`X \in\mathbf{S}^{n}`
     - |unknown| unknown
     - |convex| convex
     - None

   * - lambda_sum_smallest(X, |_| k)

       :math:`k = 1,\ldots, n`
     - :math:`\text{sum of $k$ smallest}\\ \text{eigenvalues of $X$}`
     - :math:`X \in\mathbf{S}^{n}`
     - |unknown| unknown
     - |concave| concave
     - None

   * - log_det(X)
     - :math:`\log \left(\det (X)\right)`
     - :math:`X \in \mathbf{S}^n_+`
     - |unknown| unknown
     - |concave| concave
     - None

   * - log_sum_exp(X)
     - :math:`\log \left(\sum_{ij}e^{X_{ij}}\right)`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |unknown| unknown
     - |convex| convex
     - |incr| incr.

   * - matrix_frac(x, P)
     - :math:`x^T P^{-1} x`
     - :math:`x \in \mathbf{R}^n`

       :math:`P \in\mathbf{S}^n_{++}`
     - |positive| positive
     - |convex| convex
     - None

   * - max_entries(X)
     - :math:`\max_{ij}\left\{ X_{ij}\right\}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - same as X
     - |convex| convex
     - |incr| incr.

   * - min_entries(X)
     - :math:`\min_{ij}\left\{ X_{ij}\right\}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - same as X
     - |concave| concave
     - |incr| incr.

   * - mixed_norm(X, p, q)
     - :math:`\left(\sum_k\left(\sum_l\lvert x_{k,l}\rvert^p\right)^{q/p}\right)^{1/q}`
     - :math:`X \in\mathbf{R}^{n \times n}`
     - |positive| positive
     - |convex| convex
     - None

   * - norm(x)

       norm(x, 2)
     - :math:`\sqrt{\sum_{i}x_{i}^2 }`
     - :math:`X \in\mathbf{R}^{n}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x_{i} \geq 0`

       |decr| for :math:`x_{i} \leq 0`

   * - norm(X, "fro")
     - :math:`\sqrt{\sum_{ij}X_{ij}^2 }`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`X_{ij} \geq 0`

       |decr| for :math:`X_{ij} \leq 0`

   * - norm(X, 1)
     - :math:`\sum_{ij}\lvert X_{ij} \rvert`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`X_{ij} \geq 0`

       |decr| for :math:`X_{ij} \leq 0`

   * - norm(X, "inf")
     - :math:`\max_{ij} \{\lvert X_{ij} \rvert\}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`X_{ij} \geq 0`

       |decr| for :math:`X_{ij} \leq 0`

   * - norm(X, "nuc")
     - :math:`\mathrm{tr}\left(\left(X^T X\right)^{1/2}\right)`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - None

   * - norm(X)

       norm(X, 2)
     - :math:`\sqrt{\lambda_{\max}\left(X^T X\right)}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - None

   * - :ref:`pnorm(X, p) <pnorm>`

       :math:`p \geq 1`

       or ``p = 'inf'``
     - :math:`\|X\|_p = \left(\sum_{ij} |X_{ij}|^p \right)^{1/p}`
     - :math:`X \in \mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`X_{ij} \geq 0`

       |decr| for :math:`X_{ij} \leq 0`

   * - :ref:`pnorm(X, p) <pnorm>`

       :math:`p < 1`, :math:`p \neq 0`
     - :math:`\|X\|_p = \left(\sum_{ij} X_{ij}^p \right)^{1/p}`
     - :math:`X \in \mathbf{R}^{m \times n}_+`
     - |positive| positive
     - |concave| concave
     - |incr| incr.


   * - quad_form(x, P)

       constant :math:`P \in \mathbf{S}^n_+`
     - :math:`x^T P x`
     - :math:`x \in \mathbf{R}^n`


     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x_i \geq 0`

       |decr| for :math:`x_i \leq 0`

   * - quad_form(x, P)

       constant :math:`P \in \mathbf{S}^n_-`
     - :math:`x^T P x`
     - :math:`x \in \mathbf{R}^n`
     - |negative| negative
     - |concave| concave
     - |decr| for :math:`x_i \geq 0`

       |incr| for :math:`x_i \leq 0`

   * - quad_form(c, X)

       constant :math:`c \in \mathbf{R}^n`
     - :math:`c^T X c`
     - :math:`X \in\mathbf{R}^{n \times n}`
     - depends |_| on |_| c, |_| X
     - |affine| affine
     - depends |_| on |_| c

   * - quad_over_lin(X, y)
     - :math:`\left(\sum_{ij}X_{ij}^2\right)/y`
     - :math:`x \in \mathbf{R}^n`

       :math:`y > 0`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`X_{ij} \geq 0`

       |decr| for :math:`X_{ij} \leq 0`

       |decr| decr. in :math:`y`

   * - sum_entries(X)
     - :math:`\sum_{ij}X_{ij}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - same as X
     - |affine| affine
     - |incr| incr.

   * - sum_largest(X, k)

       :math:`k = 1,2,\ldots`
     - :math:`\text{sum of } k\text{ largest }X_{ij}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - same as X
     - |convex| convex
     - |incr| incr.

   * - sum_smallest(X, k)

       :math:`k = 1,2,\ldots`
     - :math:`\text{sum of } k\text{ smallest }X_{ij}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - same as X
     - |concave| concave
     - |incr| incr.

   * - sum_squares(X)
     - :math:`\sum_{ij}X_{ij}^2`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`X_{ij} \geq 0`

       |decr| for :math:`X_{ij} \leq 0`

   * - trace(X)
     - :math:`\mathrm{tr}\left(X \right)`
     - :math:`X \in\mathbf{R}^{n \times n}`
     - same as X
     - |affine| affine
     - |incr| incr.

Clarifications
^^^^^^^^^^^^^^

The domain :math:`\mathbf{S}^n` refers to the set of symmetric matrices. The domains :math:`\mathbf{S}^n_+` and :math:`\mathbf{S}^n_-` refer to the set of positive semi-definite and negative semi-definite matrices, respectively. Similarly, :math:`\mathbf{S}^n_{++}` and :math:`\mathbf{S}^n_{--}` refer to the set of positive definite and negative definite matrices, respectively.

For a vector expression ``x``, ``norm(x)`` and ``norm(x, 2)`` give the Euclidean norm. For a matrix expression ``X``, however, ``norm(X)`` and ``norm(X, 2)`` give the spectral norm.

The function ``norm(X, "fro")`` is called the `Frobenius norm <http://en.wikipedia.org/wiki/Matrix_norm#Frobenius_norm>`__
and ``norm(X, "nuc")`` the `nuclear norm <http://en.wikipedia.org/wiki/Matrix_norm#Schatten_norms>`__. The nuclear norm can also be defined as the sum of ``X``'s singular values.

The functions ``max_entries`` and ``min_entries`` give the largest and smallest entry, respectively, in a single expression. These functions should not be confused with ``max_elemwise`` and ``min_elemwise`` (see :ref:`elementwise`). Use ``max_elemwise`` and ``min_elemwise`` to find the max or min of a list of scalar expressions.

The function ``sum_entries`` sums all the entries in a single expression. The built-in Python ``sum`` should be used to add together a list of expressions. For example, the following code sums the columns of a matrix variable:

.. code:: python

    X = Variable(100, 100)
    col_sum = sum([X[:, i] for i in range(X.size[1])])

.. _elementwise:

Elementwise functions
---------------------

These functions operate on each element of their arguments. For example, if ``X`` is a 5 by 4 matrix variable,
then ``abs(X)`` is a 5 by 4 matrix expression. ``abs(X)[1, 2]`` is equivalent to ``abs(X[1, 2])``.

Elementwise functions that take multiple arguments, such as ``max_elemwise`` and ``mul_elemwise``, operate on the corresponding elements of each argument.
For example, if ``X`` and ``Y`` are both 3 by 3 matrix variables, then ``max_elemwise(X, Y)`` is a 3 by 3 matrix expression.
``max_elemwise(X, Y)[2, 0]`` is equivalent to ``max_elemwise(X[2, 0], Y[2, 0])``. This means all arguments must have the same dimensions or be
scalars, which are promoted.

.. list-table::
   :header-rows: 1

   * - Function
     - Meaning
     - Domain
     - Sign
     - Curvature |_|
     - Monotonicity

   * - abs(x)
     - :math:`\lvert x \rvert`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x \geq 0`

       |decr| for :math:`x \leq 0`

   * - exp(x)
     - :math:`e^x`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| incr.

   * - huber(x, M=1)

       :math:`M \geq 0`
     - :math:`\begin{cases}x^2 &|x| \leq M  \\2M|x| - M^2&|x| >M\end{cases}`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x \geq 0`

       |decr| for :math:`x \leq 0`

   * - inv_pos(x)
     - :math:`1/x`
     - :math:`x > 0`
     - |positive| positive
     - |convex| convex
     - |decr| decr.

   * - log(x)
     - :math:`\log(x)`
     - :math:`x > 0`
     - |unknown| unknown
     - |concave| concave
     - |incr| incr.

   * - log1p(x)
     - :math:`\log(x+1)`
     - :math:`x > -1`
     - sign(x)
     - |concave| concave
     - |incr| incr.

   * - max_elemwise(x1, |_| ..., |_| xk)
     - :math:`\max \left\{x_1, \ldots , x_k\right\}`
     - :math:`x_i \in \mathbf{R}`
     - max(sign(xi))
     - |convex| convex
     - |incr| incr.

   * - min_elemwise(x1, |_| ..., |_| xk)
     - :math:`\min \left\{x_1, \ldots , x_k\right\}`
     - :math:`x_i \in \mathbf{R}`
     - min(sign(xi))
     - |concave| concave
     - |incr| incr.

   * - mul_elemwise(c, |_| x)

       :math:`c \in \mathbf{R}`
     - c*x
     - :math:`x \in\mathbf{R}`
     - sign(c*x)
     - |affine| affine
     - depends |_| on |_| c

   * - neg(x)
     - :math:`\max \left\{-x, 0 \right\}`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |decr| decr.

   * - pos(x)
     - :math:`\max \left\{x, 0 \right\}`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| incr.

   * - :ref:`power(x, 0) <power>`
     - :math:`1`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - constant
     - |_|

   * - :ref:`power(x, 1) <power>`
     - :math:`x`
     - :math:`x \in \mathbf{R}`
     - same as x
     - |affine| affine
     - |incr| incr.

   * - :ref:`power(x, p) <power>`

       :math:`p = 2, 4, 8, \ldots`
     - :math:`x^p`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x \geq 0`

       |decr| for :math:`x \leq 0`

   * - :ref:`power(x, p) <power>`

       :math:`p < 0`
     - :math:`x^p`
     - :math:`x > 0`
     - |positive| positive
     - |convex| convex
     - |decr| decr.

   * - :ref:`power(x, p) <power>`

       :math:`0 < p < 1`
     - :math:`x^p`
     - :math:`x \geq 0`
     - |positive| positive
     - |concave| concave
     - |incr| incr.

   * - :ref:`power(x, p) <power>`

       :math:`p > 1,\ p \neq 2, 4, 8, \ldots`

     - :math:`x^p`
     - :math:`x \geq 0`
     - |positive| positive
     - |convex| convex
     - |incr| incr.

   * - scalene(x, alpha, beta)

       :math:`\text{alpha} \geq 0`

       :math:`\text{beta} \geq 0`
     - :math:`\alpha\mathrm{pos}(x)+ \beta\mathrm{neg}(x)`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x \geq 0`

       |decr| for :math:`x \leq 0`

   * - sqrt(x)
     - :math:`\sqrt x`
     - :math:`x \geq 0`
     - |positive| positive
     - |concave| concave
     - |incr| incr.

   * - square(x)
     - :math:`x^2`
     - :math:`x \in \mathbf{R}`
     - |positive| positive
     - |convex| convex
     - |incr| for :math:`x \geq 0`

       |decr| for :math:`x \leq 0`


Vector/Matrix functions
-----------------------

A vector/matrix function takes one or more scalars, vectors, or matrices as arguments
and returns a vector or matrix.

.. list-table::
   :header-rows: 1

   * - Function
     - Meaning
     - Domain
     - Sign
     - Curvature |_|
     - Monotonicity

   * - conv(c, x)

       :math:`c\in\mathbf{R}^m`
     - :math:`c*x`
     - :math:`x\in \mathbf{R}^n`
     - depends |_| on |_| c, |_| x
     - |affine| affine
     - depends |_| on |_| c

   * - diag(x)
     - :math:`\left[\begin{matrix}x_1  & &  \\& \ddots & \\& & x_n\end{matrix}\right]`
     - :math:`x \in\mathbf{R}^{n}`
     - same as x
     - |affine| affine
     - |incr| incr.

   * - diag(X)
     - :math:`\left[\begin{matrix}X_{11}  \\\vdots \\X_{nn}\end{matrix}\right]`
     - :math:`X \in\mathbf{R}^{n \times n}`
     - same as X
     - |affine| affine
     - |incr| incr.

   * - hstack(X1, |_| ..., |_| Xk)
     - :math:`\left[\begin{matrix}X_1  \cdots    X_k\end{matrix}\right]`
     - :math:`X_i \in\mathbf{R}^{m \times n_i}`
     - sign(sum([x1, |_| ..., |_| xk]))
     - |affine| affine
     - |incr| incr.

   * - reshape(X, |_| n', |_| m')
     - :math:`X' \in\mathbf{R}^{m' \times n'}`
     - :math:`X \in\mathbf{R}^{m \times n}`

       :math:`m'n' = mn`
     - same as X
     - |affine| affine
     - |incr| incr.

   * - vec(X)
     - :math:`x' \in\mathbf{R}^{mn}`
     - :math:`X \in\mathbf{R}^{m \times n}`
     - same as X
     - |affine| affine
     - |incr| incr.

   * - vstack(X1, |_| ..., |_| Xk)
     - :math:`\left[\begin{matrix}X_1  \\ \vdots  \\X_k\end{matrix}\right]`
     - :math:`X_i \in\mathbf{R}^{m_i \times n}`
     - sign(sum([x1, |_| ..., |_| xk]))
     - |affine| affine
     - |incr| incr.


Clarifications
^^^^^^^^^^^^^^
The output :math:`y` of ``conv(c, x)`` has size :math:`n+m-1` and is defined as
:math:`y[k]=\sum_{j=0}^k c[j]x[k-j]`.

The output :math:`x'` of ``vec(X)`` is the matrix :math:`X` flattened in column-major order into a vector.
Formally, :math:`x'_i = X_{i \bmod{m}, \left \lfloor{i/m}\right \rfloor }`.

The output :math:`X'` of ``reshape(X, m', n')`` is the matrix :math:`X` cast into an :math:`m' \times n'` matrix.
The entries are taken from :math:`X` in column-major order and stored in :math:`X'` in column-major order.
Formally, :math:`X'_{ij} = \mathbf{vec}(X)_{m'j + i}`.

.. |positive| image:: functions_files/positive.svg
              :width: 15px
              :height: 15px

.. |negative| image:: functions_files/negative.svg
              :width: 15px
              :height: 15px

.. |unknown| image:: functions_files/unknown.svg
              :width: 15px
              :height: 15px

.. |convex| image:: functions_files/convex.svg
              :width: 15px
              :height: 15px

.. |concave| image:: functions_files/concave.svg
              :width: 15px
              :height: 15px

.. |affine| image:: functions_files/affine.svg
              :width: 15px
              :height: 15px

.. |incr| image:: functions_files/increasing.svg
              :width: 15px
              :height: 15px

.. |decr| image:: functions_files/decreasing.svg
              :width: 15px
              :height: 15px
