# Rust
###### tags: `Rust` `Programming Language`

write!(f, "{:?}", x) uses `Debug`
write!(f, "{}", x) uses `Display`


// `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in `1..=100` {

// `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in `1..101` {


A `Vec` can be mutable. On the other hand, `slices` are read-only objects. To get a slice, use `&`. In Rust, it's more common to pass slices as arguments rather than vectors when you just want to provide read access.

---


## 1. Understanding Ownership
### 1.2 References and Borrowing

These ampersands represent references, and they allow you to refer to some value without taking ownership of it.

## 4. Lexical structure
### 4.1 Keywords

## 5. Primitive Types
**Scalar Types**
* Signed integers: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
* Unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128` and `usize` (pointer size)
* Floating point: `f32`, `f64`
* char Unicode scalar values like ``'a'``, ``'α'`` and ``'∞'`` (4 bytes each)
* `bool` either `true` or `false`
* The unit type ``()``, whose only possible value is an empty tuple: ``()``

Despite the value of a unit type being a tuple, it is not considered a compound type because it does not contain multiple values.


**Compound Types**
* Arrays like ``[1, 2, 3]``
* Tuples like ``(1, true)``

## 6. Std library types
### 6.17. Strings
Because strings are valid UTF-8, they do not support indexing.

`String` is a heap allocated string, while `"it  is string"` creates `&'static str` which is essentially a pointer to data segment. `String::from("it is string")` and `"it is string".to_string()` create heap allocated string.

Rust strings are stored as a sequence of bytes representing characters in UTF-8 encoding. 

Although this section is largely about `String`, both types are used heavily in Rust’s standard library, and both `String` and string slices are UTF-8 encoded.

## TODO:
`method`, `trait`, `function`, `associated function`, `attributes`, `closure`, `life time`

## Reference
[The Rust Programming Language](https://doc.rust-lang.org/book/title-page.html)
[Rust By Practice](https://practice.course.rs/)
[rust-lang_v1.25](https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/first-edition/primitive-types.html)
[Rust standard library APIs](https://doc.rust-lang.org/std/index.html)