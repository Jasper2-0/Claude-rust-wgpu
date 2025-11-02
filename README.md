# Claude Rust wgpu

A clean, well-abstracted wgpu project scaffolding that hides the verbose boilerplate and provides a simple foundation for graphics programming.

## Features

- **Clean Abstractions**: All wgpu initialization and rendering code is encapsulated in a `State` struct
- **Easy to Extend**: Simple methods for update, render, input handling, and resize events
- **Interactive Demo**: Move your cursor to see the background color change dynamically
- **Modern Rust**: Uses latest winit 0.29 and wgpu 0.19

## Project Structure

```
src/
├── lib.rs   # State abstraction and wgpu resource management
└── main.rs  # Clean event loop
```

### State Abstraction

The `State` struct in `lib.rs` provides:

- `new(window)` - Async initialization of all wgpu resources
- `resize(size)` - Handles window resize events
- `input(event)` - Processes input events
- `update()` - Frame update logic
- `render()` - Rendering logic

This design keeps the main event loop clean and focused while hiding wgpu's verbose setup code.

## Building and Running

### Prerequisites

- Rust 1.70 or later
- A GPU with Vulkan, Metal, or DirectX 12 support

### Build

```bash
cargo build --release
```

### Run

```bash
cargo run --release
```

### Development Mode

```bash
# With logging enabled
RUST_LOG=info cargo run
```

## Controls

- **ESC** - Exit application
- **Mouse Move** - Changes background color based on cursor position

## Extending the Project

### Adding Custom Rendering

Modify the `render()` method in `src/lib.rs`:

```rust
pub fn render(&mut self) -> Result<(), wgpu::SurfaceError> {
    // Your custom rendering code here
}
```

### Adding Shaders

1. Create a `shaders/` directory
2. Add your WGSL shader files
3. Load them in `State::new()` using `device.create_shader_module()`

### Adding Input Handling

Extend the `input()` method in `src/lib.rs`:

```rust
pub fn input(&mut self, event: &WindowEvent) -> bool {
    match event {
        WindowEvent::KeyboardInput { .. } => {
            // Handle keyboard input
            true
        }
        _ => false,
    }
}
```

## Dependencies

- **wgpu** - Modern, safe graphics API abstraction
- **winit** - Cross-platform window creation
- **pollster** - Simple executor for async wgpu initialization
- **env_logger** - Logging support
- **bytemuck** - Safe casting for vertex data

## Resources

- [wgpu Documentation](https://wgpu.rs/)
- [Learn wgpu Tutorial](https://sotrh.github.io/learn-wgpu/)
- [winit Documentation](https://docs.rs/winit/)

## License

This project template is free to use for any purpose.
