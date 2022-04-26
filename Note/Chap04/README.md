# Chapter04 Processing and Summarizing Data
- In statistics nomenclature, the act of summarizing data is known as ***descriptive statistics***. In data science nomenclature such activities take the names of ***analytics*** and ***dash-boarding***, while the process of manipulating and pre-processing data is sometimes called ***data cleansing***, or ***data cleaning***.

## Mutability, References, Shallow Copies and Deep Copies in Julia
- Two different mechanisms for passing variables to functions.
  - ***Call by value*** `f()` gets a copy of the variable `x`. As `f()` executes, even if the code appears to modify x, it is actually modifying a local copy.
  - ***Call by reference*** `f()` obtains a ***memory reference*** (***pointer***) to `x`. In such a case, as `f()` executes, if it modifies `x`, then it actually modifies values in the original memory location of `x`.
- In Julia, both mechanisms exist under a unified umbrella called ***pass by sharing***. This means that variables are not copied when passed to funcitons. However, if a value is about to a changed within a function then depending on the mutability attribute of its types.
  - If the variable type is ***immutable*** then a local copy is made and the behavior follows the "call by value" type.
  - If the variable type is ***mutable*** then the called function does not create a local copy. Instead, it can modify the original variable according to "call by reference" mechanisms.
- In Julia, primitive type such as `Int64` or `Float32` are immutable. But arrays are mutable.

```julia
f(z::Int) = begin z=0 end
f(z::Array{Int}) = begin z[1]=0 end

x = 1
@show typeof(x)
@show isimmutable(x)
println("Before call by value: ", x)
f(x)
println("After call by value: ", x, '\n')

x = [1]
@show typeof(x)
@show isimmutable(x)
println("Before call by reference: ", x)
f(x)
println("After call by reference: ", x)
```
- `copy()` creats a "shallow copy" of the vairables and copies all entries but does not do it recursively.
- `deepcopy()` recursively produces a copy until a completely independent copy of the variable is created.

```julia
println("Immutable: ")
a = 10
b = a
b = 20
@show a

println("\nNo copy:")
a = [10]
b = a
b[1] = 20
@show a

println("\nCopy: ")
a = [10]
b = copy(a)
b[1] = 20
@show a

println("\nShallow copy: ")
a = [[10]]
b = copy(a)
b[1][1] = 20
@show a

println("\nDeep copy: ")
a = [[10]]
b = deepcopy(a)
b[1][1] = 20
@show a;
```

