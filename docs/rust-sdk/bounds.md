# Bounds

## `Bounds<T>`

`Bounds` represents a rectangular region in 2D space. This provides essential methods for positioning, sizing, and geometric operations.

```rust title="turbo::bounds"
pub struct Bounds {
    // Top-left corner coordinates.
    x = i32,
    y = i32,
    // Width and height of the rectangle.
    w = u32,
    h = u32,
}
```

### Using bound to get canvas size

```rust
// Create a new bound with the values of your canvas
let viewport = bounds();
```

### Create a bounds with size and center it

```rust
//A bound now exists that is 48 by 14
let bound = Bounds::with_size(48, 14)
    // centers it horizontally and vertically
    .anchor_center(&viewport)
    // moves it up 16px
    .translate_y(-16);
```

### Making it visible

```rust
rect!(
    //set color of bounds
    color = 0xffffffff,
    // Use the bounds position for x and y
    xy = bound.xy(),
    // Use the bounds size for w and h
    wh = bound.wh(),
    );
```

## Complete Example

Create a row of 3 buttons that use `bounds` to recognize whenever they are hovered or clicked

```rust
turbo::go! {
    clear(0x000000ff);

    let viewport = Bounds::viewport();

    let btn = viewport
        .height(32)
        .inset_left(8)
        .inset_right(8)
        .anchor_center(&viewport)
        .inset_bottom(12)
        .columns_with_gap(3, 12);
    for (i, btn) in btn.into_iter().enumerate() {
        let label = match i {
            0 => "One",
            1 => "Two",
            2 => "Three",
            _ => "",
        };
        let (regular_color, hover_color, pressed_color) = match i {
            0 => (0x8833AAff, 0xAA55CCff, 0x800080FF),
            1 => (0xCC3333ff, 0xFF5555ff, 0xFF0000FF),
            2 => (0x33CCFFff, 0x66DDFFFF, 0x00FFFFFF),
            _ => (0x3333CCff, 0x5555FFff, 0x000000ff),
        };
        rect!(
            color = if btn.just_pressed() {
                pressed_color
            } else if btn.hovered() {
                hover_color
            } else {
                regular_color
            },
            w = btn.w(),
            h = btn.h(),
            x = btn.x(),
            y = btn.y(),
            border_radius = 2,
        );

        let btn_inner = btn.inset_left(4).inset_top(4);
        text!(label, x = btn_inner.x(), y = btn_inner.y(), font = "medium");    
    }
}
```

![Bounds example showing a simple button row](/bound-example.gif)
