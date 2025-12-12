# Spell Converter - TFS

Desktop tool developed with Tauri + Rust for converting and managing TFS files.

## 📋 Prerequisites

Before starting, ensure you have the following installed on your machine:

1.  **Rust** (Programming Language)
    - Download and install: [https://rustup.rs/](https://rustup.rs/)
    - During installation, select the default option.

2.  **Visual Studio Build Tools** (For compiling C++ on Windows)
    - Required for compiling Rust dependencies.
    - Download "Visual Studio Build Tools" and install the "Desktop development with C++" workload.

3.  **WebView2** (Already installed on updated Windows 10/11)
    - Tauri uses Edge's WebView2 to render the interface.

4.  **Tauri CLI** (Command Line Tool)
    - After installing Rust, run in your terminal:
      ```powershell
      cargo install tauri-cli
      ```

---

## 🚀 How to Run the Project

This project includes a `build.bat` script to facilitate all operations.

### 1. Clone and Prepare
If you just downloaded the project, make sure Rust is installed and updated.

### 2. Run in Development Mode
To test the application while editing the code (with hot-reload):

1.  Run the `build.bat` file.
2.  Choose option **[3] 🏃 Run in DEV**.

This will compile the Rust backend and open the application window.

---

## 📦 How to Build (.exe)

To generate the final executable for distribution:

1.  Run the `build.bat` file.
2.  Choose one of the build options:
    - **[1] ⚡ Fast Build:** Uses existing cache (faster).
    - **[2] 🧹 Clean & Build:** Cleans everything and builds from scratch (recommended for final builds or if strange errors occur).

The executable will be generated at:
`src-tauri/target/release/spell-converter.exe`

---

## 🛠️ Project Structure

- **src/**: Contains Frontend code (HTML, CSS, JS).
  - `index.html`: Main interface.
  - `styles.css`: Application styles.
  - `main.js`: Frontend logic and backend communication.
- **src-tauri/**: Contains Backend code (Rust).
  - `src/main.rs`: Main Rust logic, Tauri commands, and file manipulation.
  - `tauri.conf.json`: Window configurations, permissions, and build settings.
- **build.bat**: Utility script to facilitate building and running.

## 📝 Manual Commands (Optional)

If you prefer not to use `build.bat`, you can run commands directly in the terminal within the `src-tauri` folder:

- **Dev:** `cargo tauri dev`
- **Build:** `cargo tauri build`
- **Clean:** `cargo clean`

---

## ⚠️ Common Troubleshooting

- **Error "tauri command not found":**
  Ensure you have installed the CLI: `cargo install tauri-cli`.

- **Linker Error (link.exe):**
  Check if Visual Studio Build Tools with C++ is correctly installed.

- **White screen or connection error:**
  Check if another process is using the same port or if your antivirus is blocking the executable.
