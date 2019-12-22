# Native Tutorial

In this tutorial, we're going to write a program that resizes an image and applies a filter to it. 
You'll get a feel for how to use Photon, and will be able to build upon this to use Photon in your own projects. 

## Getting Started 
Ensuring you have Rust installed, create a new Rust project:

```bash
cargo new photon-demo
cd photon-demo
```

Once you've moved into the new directory, take a look at the source files generated.

## Add Photon as A Dependency

Add Photon as a dependency to your project. 

Your Cargo.toml should look like this:

#### Cargo.toml
    #!toml hl_lines="8"
    [dependencies]
    photon=1.2

## Writing The Program
Next up, open your `bin.rs` file. You'll find a sample function in there, remove that since we won't be using it. 

#### Open An Image

To open an image:

##### bin.rs
    #!rust hl_lines="4"
    extern crate photon;
    use photon::native::{open_image};
    fn main() {
        let mut img = open_image("image.jpg");
    }

#### Apply a Filter Effect

To apply a filter effect to the opened image, we need to pass in our image and a filter name. 

    #!rust
    photon::filters::filter(&mut img, "twenties");

Notice that we're passing a mutable reference to the image. This allows the function to modify the image, rather than return a new image.
There are a variety of filter effects we can pass. Once you get the program compiled, try passing in "radio" instead of the filter above.
For a full list, see the documentation. 

#### Save To The Filesystem
Then, to write the image to the filesystem:

    #!rust
    save_image(img, "new_image.jpg");

Notice here we're saving it as a JPG image, but we could also save it as a PNG or a different output format, by including a different file extension.

#### Get An Image
Next up, you'll need an image to work with. You can use an image from your own collection, or try out the images available at [Unsplash](https://unsplash.com/), 
which are also available in the Public Domain.

Name it `image.jpg`, and save it in the same directory as your rust project.

### Final Program 
The final code looks like this:

##### bin.rs 
    #!rust
    extern crate photon;
    use photon::Rgb;
    use photon::native::{open_image, save_image};

    fn main() {
        // Open the image (a PhotonImage is returned)
        let mut img = open_image("image.jpg");

        // Apply a filter to the pixels
        photon::filters::filter(&mut img, "twenties");
        save_image(img, "new_image.jpg");    

    }

##### Run The Code
To run the program in release mode, run: 

```bash
cargo run --release 
```

!!! warning
    Make sure you run in **release** mode for optimum performance. Otherwise, performance will be greatly affected.8 
    Don't forget to add the release flag, as shown above.


#### Bonus: Add Timing 
If you'd like to find out how long it takes to process your image, you can add some code to capture this:

    #!rust hl_lines="11"
    extern crate photon;
    use photon::Rgb;
    use photon::native::{open_image, save_image};
    use time::{PreciseTime};

    fn main() {
        // Open the image (a PhotonImage is returned)
        let mut img = open_image("image.jpg");

        // Apply a filter to the pixels
        let start = PreciseTime::now();
        photon::channels::alter_channel(&mut img, 1, -20);
        let end = PreciseTime::now();
        println!("Took {} seconds to process image.", start.to(end));
        save_image(img, "raw_image.png");    

    }

#### Examples

To view more examples for native-use, check out the [`/examples`](https://github.com/silvia-odwyer/photon/tree/master/crate/examples) folder in Photon's repository.
You'll find full instructions on how to run these in the README.

#### Working with the Web
If you'd like to get started with Photon for the web, see the [accompanying tutorial](web-tutorial.md).