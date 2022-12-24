# [Image Combiner in Rust](https://www.freecodecamp.org/news/rust-in-replit/#step-9-create-the-combined-image-data)


Within `args.rs`, create a function named `get_nth_arg` which takes a `usize, n` , and returns a `String`. Then, from the `std::env` module, call the `args function`, and chain the nth method to get the nth argument, unwrapping the value.
Define a public struct named Args which consists of three public fields of type String: `image_1, image_2, and output`.
```rs
fn get_nth_arg(n: usize) -> String {
  std::env::args().nth(n).unwrap()
}

pub struct Args {
  pub image_1: String,
  pub image_2: String,
  pub output: String,
}

impl Args {
  pub fn new() -> Self {
    Args {
      image_1: get_nth_arg(1),
      image_2: get_nth_arg(2),
      output: get_nth_arg(3),
    }
  }
}
```

## [Image carte](https://crates.io/crates/image)
**This [crate](https://docs.rs/image/latest/image/) provides basic image processing functions and methods for converting to and from various image formats. All image processing functions provided operate on types that implement the GenericImageView and GenericImage traits and return an ImageBuffer.**
In much the same way other languages have libraries or packages, Rust has crates. In order to encode and decode images, you can use the `image` crate.
Add the image crate with version `0.23.14` to the `Cargo.toml` file.
The image crate comes with an `io` module including a `Reader` struct. This struct implements an `open function` which takes a path to an image file, and returns a Result containing a reader. You can format and decode this reader to yield the image format (for example PNG, JGP, and so on) and the image data.
```rs
fn find_image_from_path(path: String) -> (DynamicImage, ImageFormat) {
  let image_reader: Reader<BufReader<File>> = Reader::open(path).unwrap();
  let image_format: ImageFormat = image_reader.format().unwrap();
  let image: DynamicImage = image_reader.decode().unwrap();
  (image, image_format)
}
```

##  Resize the Images to Match
To make combining the images easier, you resize the largest image to match the smallest image.
First, you can find the smallest image using the `dimensions` method which returns the width and height of the image as a tuple. These tuples can be compared, and the smallest returned. If `image_2` is the smallest image, then `image_1` needs to be resized to match the smallest dimensions. Otherwise, image_2 needs to be resized.
The `resize_exact` method implemented on the `DynamicImage` struct mutably borrows the image, and, using the width, height, and `FilterType` arguments, resizes the image.
```rs
fn get_smallest_dimensions(dim_1: (u32, u32), dim_2: (u32, u32)) -> (u32, u32) {
  let pix_1 = dim_1.0 * dim_1.1;
  let pix_2 = dim_2.0 * dim_2.1;
  return if pix_1 < pix_2 { dim_1 } else { dim_2 };
}

fn standardise_size(image_1: DynamicImage, image_2: DynamicImage) -> (DynamicImage, DynamicImage) {
  let (width, height) = get_smallest_dimensions(image_1.dimensions(), image_2.dimensions());
  if image_2.dimensions() == (width, height) {
    (image_1.resize_exact(width, height, Triangle), image_2)
  } else {
    (image_1, image_2.resize_exact(width, height, Triangle))
  }
}
```

## Create a Floating Image
To handle the output, create a temporary struct to hold the metadata for the output image.
Define a struct named `FloatingImage` to hold the width, height, and data of the image, as well as the name of the output file. As you haven't created the data for the image yet, create a buffer in the form of a `Vec` of u8s with a capacity of `3,655,744 (956 x 956 x 4`). `The <number>_<number>` syntax is Rust's easy-to-read numbering which separates the number into groups or three digits.

```rs
struct FloatingImage {
  width: u32,
  height: u32,
  data: Vec<u8>,
  name: String,
}

impl FloatingImage {
  fn new(width: u32, height: u32, name: String) -> Self {
    let buffer_capacity = 3_655_744;
    let buffer: Vec<u8> = Vec::with_capacity(buffer_capacity);
    FloatingImage {
      width,
      height,
      data: buffer,
      name,
    }
  }
}
```


```
cargo run -- images/2.jpg images/1.jpg images/output.png
```