
# WebAssembly Calculator

This project demonstrates how to create a simple calculator in C, compile it to WebAssembly, and interact with it through a web interface.

## WebAssembly Setup and Compilation

### Prerequisites

- **Emscripten**: Make sure Emscripten is installed and set up in your environment. If it’s not already installed, follow these steps:
  1. Clone the Emscripten SDK repository:

     ```bash
     git clone https://github.com/emscripten-core/emsdk.git ~/dev_tools/emsdk
     cd ~/dev_tools/emsdk
     ```

  2. Install and activate the latest version:

     ```bash
     ./emsdk install latest
     ./emsdk activate latest
     source ./emsdk_env.sh
     ```

### Compiling the C Code to WebAssembly

To compile the C code (`calculator.c`) to WebAssembly, use the following command:

```bash
emcc calculator.c -s WASM=1 -o calculator.js -s NO_EXIT_RUNTIME=1 -s "EXPORTED_FUNCTIONS=['_add','_subtract','_multiply','_divide']"
```

#### Explanation of the Compilation Command

- **`emcc calculator.c`**: This compiles the C file to WebAssembly.
- **`-s WASM=1`**: Ensures that the output includes a WebAssembly module (`.wasm`).
- **`-o calculator.js`**: Specifies the output JavaScript file (`calculator.js`) that serves as the interface between the browser and the WebAssembly module.
- **`-s NO_EXIT_RUNTIME=1`**: Prevents the runtime from exiting, which is necessary for keeping the WebAssembly environment alive in a browser.
- **`-s "EXPORTED_FUNCTIONS=['_add','_subtract','_multiply','_divide']"`**: Explicitly exports the C functions so they can be called from JavaScript. The functions are referenced with a leading underscore (`_`) in JavaScript (e.g., `_add`).

### How It All Ties Together

#### 1. **The `Module` Object**

- When you compile the C code, Emscripten generates a JavaScript file (e.g., `calculator.js`), which contains a `Module` object.
- **`Module`**: This object provides the interface between JavaScript and the WebAssembly module. It includes methods for loading the WebAssembly binary and accessing the exported C functions.

#### 2. **Exported Functions**

- The C functions specified in the `EXPORTED_FUNCTIONS` list during compilation (e.g., `add`, `subtract`, `multiply`, `divide`) are made available in JavaScript through `Module`.
- **Calling Functions**: In JavaScript, you call these functions using the `Module._functionName` syntax (e.g., `Module._add(num1, num2)`).

#### 3. **Web Interface Interaction**

- The web interface (defined in `index.html`) provides input fields for the user to enter numbers and select an operation.
- When the user clicks "Calculate," JavaScript retrieves the inputs, calls the corresponding WebAssembly function via `Module`, and displays the result.

### Notes for Future Development

- **Adding New Functions**: If you add more functions to `calculator.c`, remember to update the `EXPORTED_FUNCTIONS` list during compilation.
- **Updating the Web Interface**: Any changes to `index.html` are immediately reflected once you refresh the browser. The JavaScript in this file handles all interactions between the UI and the WebAssembly module.

### Running the Web App

1. **Start a local server**:

   ```bash
   python3 -m http.server
   ```

2. **Open the app**:

   Navigate to `http://localhost:8000` in your browser to interact with the calculator.

## Summary

- **Compilation**: The Emscripten command compiles your C code into WebAssembly and JavaScript, which are then used in a web page.
- **Interaction**: The web interface interacts with the WebAssembly functions via the `Module` object generated by Emscripten.

This `README.md` should serve as a concise guide to setting up, compiling, and understanding how the WebAssembly pieces fit together in your project.
