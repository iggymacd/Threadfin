# Building From Source

This guide explains how to build Threadfin from its source code. This is useful if you want to run a development version, contribute to the project, or if pre-compiled binaries are not available for your platform.

## Prerequisites

*   **Go:** Version 1.18 or newer. You can download and install Go from [https://golang.org/dl/](https://golang.org/dl/). Follow the official installation instructions for your operating system.
*   **Git:** To clone the Threadfin repository. Install Git from [https://git-scm.com/downloads](https://git-scm.com/downloads).
*   **Make (Optional but Recommended):** Some projects use Makefiles for build automation. Check the Threadfin repository for a `Makefile`.
*   **Node.js and npm/yarn (If building frontend assets):** Threadfin's frontend uses TypeScript. To compile the TypeScript files into JavaScript for the web UI, you'll need Node.js and a package manager like npm (comes with Node.js) or Yarn.

## Build Steps

### 1. Clone the Repository

Open your terminal or command prompt and clone the Threadfin GitHub repository:

```bash
git clone https://github.com/Threadfin/Threadfin.git
cd Threadfin
```

### 2. Build the Go Backend

This is the primary step to compile the main Threadfin application.

*   **Tidy Dependencies:**
    This command ensures your `go.mod` and `go.sum` files are consistent and downloads necessary dependencies.
    ```bash
    go mod tidy
    ```
*   **Vendor Dependencies (Optional but Good Practice):**
    This command stores a copy of the dependencies in a `vendor` directory within your project. This can make builds more reproducible.
    ```bash
    go mod vendor
    ```
*   **Build the Executable:**
    This command compiles the Go source code and creates the `threadfin` executable (or `threadfin.exe` on Windows).
    ```bash
    go build threadfin.go
    ```
    After this step, you should find the executable in the root of the `Threadfin` directory. You can run it directly:
    ```bash
    ./threadfin [options]
    ```

### 3. Build Frontend Assets (TypeScript to JavaScript)

Threadfin's web UI is built with TypeScript (`.ts` files in the `ts/` directory), which needs to be compiled into JavaScript (`.js` files, typically placed in `html/js/`) for browsers to understand.

*   **Navigate to the TypeScript Directory (if applicable):**
    The `README.md` for Threadfin mentions: "If TypeScript files were changed, run: `tsc -p ./ts/tsconfig.json`". This implies you might need to run this from the root of the project or within the `ts/` directory.
    ```bash
    # Assuming you are in the root of the Threadfin project
    # The tsconfig.json file dictates how TypeScript is compiled.
    ```
*   **Install TypeScript Compiler (if not already installed globally):**
    If you don't have `tsc` (TypeScript Compiler) available, you might need to install it. This is often done as a dev dependency within a project using `package.json`, or globally.
    If there's a `package.json` in the root or `ts/` directory:
    ```bash
    # npm install # To install dependencies from package.json
    # or
    # yarn install
    ```
    To install TypeScript globally (less common for project-specific builds but might be assumed):
    ```bash
    # npm install -g typescript
    ```
*   **Compile TypeScript:**
    As per the Threadfin `README.md`:
    ```bash
    tsc -p ./ts/tsconfig.json
    ```
    This command should compile the `.ts` files and output the resulting `.js` files to the directory specified in `tsconfig.json` (likely `html/js/`).

### 4. Embed Updated JavaScript Files (If Necessary)

The Threadfin `README.md` also states: "Then, to embed updated JavaScript files into the source code (src/webUI.go), run it in development mode at least once: `go build threadfin.go; threadfin -dev`".

This suggests that Threadfin might embed the compiled JavaScript assets directly into the Go binary for serving.

*   **Re-build and Run in Dev Mode:**
    After compiling your TypeScript (Step 3), if you made changes to the `.ts` files:
    1.  Re-build the Go application:
        ```bash
        go build threadfin.go
        ```
    2.  Run Threadfin in development mode:
        ```bash
        ./threadfin -dev
        ```
        This `-dev` mode likely triggers a process where Threadfin reads the newly compiled JavaScript files from `html/js/` and updates an internal Go source file (e.g., `src/webUI.go`) that embeds these assets. This ensures the Go binary serves the latest frontend code.
    3.  After running with `-dev` once and it completes its task (it might exit or indicate completion), you may need to **re-build the Go application one final time** to include these embedded assets in the production binary:
        ```bash
        go build threadfin.go
        ```

### Summary of Full Build Including Frontend Changes:

1.  Make changes to Go backend (`.go` files) or TypeScript frontend (`.ts` files).
2.  If `.ts` files changed: `tsc -p ./ts/tsconfig.json`
3.  `go build threadfin.go`
4.  If `.ts` files changed (and thus JS assets need re-embedding): `./threadfin -dev` (run once, let it do its thing)
5.  `go build threadfin.go` (final build to include embedded assets)

The resulting `threadfin` executable should now contain all your backend and frontend changes.

## Cross-Compilation (Optional)

Go makes cross-compilation straightforward. If you want to build Threadfin for a different OS or architecture than your current system:

Set `GOOS` (target operating system) and `GOARCH` (target architecture) environment variables before running `go build`.

**Example: Build for Linux ARM64 from a Windows/macOS Intel machine:**

*   **Linux/macOS Terminal:**
    ```bash
    GOOS=linux GOARCH=arm64 go build threadfin.go
    ```
*   **Windows PowerShell:**
    ```powershell
    $env:GOOS = "linux"
    $env:GOARCH = "arm64"
    go build threadfin.go
    Remove-Item Env:GOOS # Clean up
    Remove-Item Env:GOARCH
    ```

Common `GOOS` values: `linux`, `windows`, `darwin` (for macOS).
Common `GOARCH` values: `amd64` (for 64-bit Intel/AMD), `arm64` (for 64-bit ARM), `arm` (for 32-bit ARM).

Refer to Go documentation for all supported `GOOS` and `GOARCH` combinations.
