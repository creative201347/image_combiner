

**The `..=` syntax is Rust's range syntax which allows the range to be inclusive of the end value. The `*` symbol before a variable is Rust's dereferencing operator, which allows the value of the variable to be accessed.**


``` rs
// Struct Definition
struct Person {
    name: String
}

// Impl block defining method
impl Person {
  fn say_name(&self){
    println!("{}", self.name);
  }
}

```
**`impl Struct ...` adds some methods to Struct. These methods aren't available to other types or traits.**
The `impl` keyword is primarily used to define implementations on types. Inherent implementations are standalone, while trait implementations are used to implement traits for types, or other traits.

**Some functions are connected to a particular type. These come in two forms: `associated functions`, and `methods`. Associated functions are functions that are defined on a type generally, while methods are associated functions that are called on a particular instance of a type.**
``` rs
struct Point {
    x: f64,
    y: f64,
}

// Implementation block, all `Point` associated functions & methods go in here
impl Point {
    // This is an "associated function" because this function is associated with
    // a particular type, that is, Point.
    //
    // Associated functions don't need to be called with an instance.
    // These functions are generally used like constructors.
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    }

    // Another associated function, taking two arguments:
    fn new(x: f64, y: f64) -> Point {
        Point { x: x, y: y }
    }
}
```
