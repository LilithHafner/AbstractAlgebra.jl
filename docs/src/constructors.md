# Constructing mathematical objects in AbstractAlgebra.jl

## Constructing objects in Julia

In Julia, one constructs objects of a given type by calling a type constructor. This is
simply a function with the same name as the type itself. For example, to construct a
`BigInt` object from an `Int` in Julia, we simply call the `BigInt` constructor:

```julia
n = BigInt(123)
```

Note that a number literal too big to fit in an `Int` or `Int128` automatically creates
a `BigInt`:

```julia
julia> typeof(12345678765456787654567890987654567898765678909876567890)
BigInt
```

## How we construct objects in AbstractAlgebra.jl

As we explain in [Elements and parents](https://nemocas.github.io/AbstractAlgebra.jl/dev/extending_abstractalgebra/#Elements-and-parents), Julia types
don't contain enough information to properly model groups, rings, fields, etc.
Instead of using types to construct objects, we use special objects that we
refer to as parent objects. They behave a lot like Julia types.

Consider the following simple example, to create a multiprecision integer:

```julia
n = ZZ(12345678765456787654567890987654567898765678909876567890)
```

Here `ZZ` is not a Julia type, but a callable object. However, for most purposes
one can think of such a parent object as though it were a type.

## Constructing parent objects

For more complicated groups, rings, fields, etc., one first needs to construct the
parent object before one can use it to construct element objects.

AbstractAlgebra.jl provides a set of functions for constructing such parent objects.
For example, to create a parent object for univariate polynomials over the integers,
we use the `polynomial_ring` parent object constructor.

```julia
R, x = polynomial_ring(ZZ, "x")
f = x^3 + 3x + 1
g = R(12)
```

In this example, `R` is the parent object and we use it to convert the `Int` value
$12$ to an element of the polynomial ring $\mathbb{Z}[x]$.

## List of parent object constructors

For convenience, we provide a list of all the parent object constructors in
AbstractAlgebra.jl and explain what mathematical domains they represent.

| Mathematics                      | AbstractAlgebra.jl constructor              |
|----------------------------------|---------------------------------------------|
| $R = \mathbb{Z}$                 | `R = ZZ`                                    |
| $R = \mathbb{Q}$                 | `R = QQ`                                    |
| $R = \mathbb{F}_{p}$             | `R = GF(p)`                                 |
| $R = \mathbb{Z}/n\mathbb{Z}$     | `R = residue_ring(ZZ, n)`                   |
| $S = R[x]$                       | `S, x = polynomial_ring(R, "x")`            |
| $S = R[x, y]$                    | `S, (x, y) = polynomial_ring(R, ["x", "y"])`|
| $S = R[[x]]$ (to precision $n$)  | `S, x = power_series_ring(R, n, "x")`       |
| $S = R((x))$ (to precision $n$)  | `S, x = laurent_series_ring(R, n, "x")`     |
| $S = K((x))$ (to precision $n$)  | `S, x = laurent_series_field(K, n, "x")`    |
| $S = \mathrm{Frac}_R$            | `S = fraction_field(R)`                     |
| $S = R/(f)$                      | `S = residue_ring(R, f)`                    |
| $S = R/(f)$ (with $(f)$ maximal) | `S = residue_field(R, f)`                   |
| $S = \mathrm{Mat}_{m\times n}(R)$| `S = matrix_space(R, m, n)`                 |
| $S = \mathbb{Q}[x]/(f)$          | `S, a = number_field(f, "a")`               |
