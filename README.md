# Babylon.rs

<a href="https://docs.rs/babylon"><img src="https://img.shields.io/badge/docs-latest-blue.svg?style=flat-square" alt="docs.rs docs" /></a>

A WebAssembly wrapper for [Babylon.js](https://www.babylonjs.com/) in Rust.

This project is pre-alpha and the api is in active exploration. Current priorities:

* get a basic GLTF up
* get a camera
* get some sort of interaction

This project uses [`js_ffi`](https://github.com/richardanaya/js_ffi) for javascript binding and [`lazy_static`](https://github.com/rust-lang-nursery/lazy-static.rs) for global static singletons fairly extensively.

# Idioms
* Scenes hold 3D objects
* Materials determine how a 3D object looks
* When an 3D object drops it's removed from the scene

# HelloWorld

<p align="center">
  <img src="https://richardanaya.github.io/babylon.rs/images/demo_0.png">
</p>

```rust
use babylon::prelude::*;
#[macro_use]
extern crate lazy_static;
use std::sync::Mutex;

lazy_static! {
    static ref GAME: Mutex<Game> = Mutex::new(Game::new());
}

struct Game {
    scene: Scene,
    shape: Vec<Sphere>,
}

impl Game {
    fn new() -> Self {
        Game {
            scene: Scene::create_from_basic_engine("#renderCanvas"),
            shape: vec![],
        }
    }
}

#[no_mangle]
pub fn main() {
    babylon::js::log("Starting demo...");
    let mut game = GAME.lock().unwrap();
    for _ in 0..10 {
        let mut sphere = Sphere::new(&game.scene, babylon::js::random());
        sphere.set_position(
            babylon::js::random() - 0.5,
            babylon::js::random() - 0.5,
            babylon::js::random() - 0.5,
        );
        game.shape.push(sphere);
    }
}
```

See this demo [here](https://richardanaya.github.io/babylon.rs/examples/helloworld/index.html) ( be sure to play with mouse and arrow keys :arrow_left: :arrow_up: :arrow_down: :arrow_right:!)

# Materials

<p align="center">
  <img src="https://richardanaya.github.io/babylon.rs/images/demo_1.png">
</p>

```rust
let mut game = GAME.lock().unwrap();
for _ in 0..10 {
    let mut cube = Cube::new(
        &game.scene,
        babylon::js::random(),
        babylon::js::random(),
        babylon::js::random(),
    );
    let mut mat = StandardMaterial::new(&game.scene);
    mat.set_diffuse_color(Color::new(babylon::js::random(),babylon::js::random(),babylon::js::random()));
    mat.set_alpha(babylon::js::random());
    cube.set_material(&mat);
    cube.set_position(
        babylon::js::random() - 0.5,
        babylon::js::random() - 0.5,
        babylon::js::random() - 0.5,
    );
    game.shape.push(cube);
}
```

See this demo [here](https://richardanaya.github.io/babylon.rs/examples/materials/index.html) ( be sure to play with mouse and arrow keys :arrow_left: :arrow_up: :arrow_down: :arrow_right:!)

# Other Demos

* [Timer](https://richardanaya.github.io/babylon.rs/examples/timer/index.html)
* [Keyboard](https://richardanaya.github.io/babylon.rs/examples/keyboard/index.html)

* [Pong](https://richardanaya.github.io/babylon.rs/examples/pong/index.html)

# License

This project is licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or
   http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in `babylon.rs` by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
