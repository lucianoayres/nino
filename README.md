![nino-banner-v1](https://github.com/user-attachments/assets/6dec07fd-2adb-4eb8-8e67-5b2d9445c209)

[About](#about-nino) · [Demo](#demo) · [Features](#features) · [Ollama Dependency](#ollama-dependency) · [Requirements](#requirements) · [Installation](#installation) · [Usage Examples](#usage-examples) · [Using Env Vars](#using-environment-variables) · [Command-line Flags](#command-line-flags) · [Makefile Usage](#makefile-usage) · [Acknowledgements](#acknowledgements) · [License](#license) · [Contribution](#contribution)

## About Nino

Nino is a Go-based command-line tool that simplifies interaction with local language models served by [Ollama](https://github.com/jmorganca/ollama). It allows you to send prompts to models, receive real-time streaming responses directly in your terminal, and configure models using straightforward command-line arguments.

Nino enhances the basic interaction provided by Ollama by displaying full model responses in the terminal and enabling you to save outputs to a file, offering a seamless experience for working with language models.

## Demo

![btcq_demo](https://github.com/user-attachments/assets/180b8196-d1d2-44b9-95f3-19deeed4d808)

## Features

-   **Streaming Responses**: Nino handles and prints streaming responses generated by the Ollama CLI as the model processes the input.
-   **Flexible Prompt Input**: Nino supports prompts provided directly via the `-prompt` argument or from a text file using the `-prompt-file` option.
-   **Export File**: Nino allows users to optionally save model output to a file, while still displaying real-time responses in the terminal.

## Ollama Dependency

Nino relies on the [Ollama CLI tool](https://github.com/jmorganca/ollama) to interact with local language models. Ollama must be installed and running on your machine or server for nino to function properly.

### Install Ollama

Follow the instructions on the [Ollama GitHub repository](https://github.com/jmorganca/ollama) to install and set up Ollama. Ensure that Ollama is available in your system’s `$PATH`.

### Start the Ollama Server

Once Ollama is installed, you need to start the server:

1. **Start the Ollama Server**: This command will run the Ollama server on `http://localhost:11434/api/generate` (default URL and port).

    ```bash
    ollama serve
    ```

2. **Run the Model**: To run the desired model (e.g., `llama3.1`), execute the following command in a separate terminal window:

    ```bash
    ollama run llama3.1
    ```

> **Note:** The `-model` parameter in nino **must match** the model that you run on Ollama. For example, if you start `llama3.1` in Ollama, you must pass `llama3.1` as the `-model` in nino. Otherwise, nino will not be able to communicate with the correct model.

Ollama should now be running, and nino can interact with it by sending prompts.

## Requirements

-   Go 1.23+ installed on your system
-   [Ollama](https://github.com/jmorganca/ollama) installed and running locally or on your server
-   Ensure that the Ollama server is running via `ollama serve`

## Installation

1. Clone this repository:

    ```bash
    git clone https://github.com/lucianoayres/nino.git
    cd nino
    ```

2. Build the project:

    ```bash
    make build
    ```

## Usage Examples

After building the project and ensuring that the Ollama server is running, you can run nino with the following commands:

### Using Default Model and URL:

You can use nino with just a prompt as the only argument. By default, it will use the `llama3.1` model and connect to the default URL and port for the local Ollama server:

```bash
./nino "Who said the quote, 'A person who never made a mistake never tried anything new'?"
```

To prevent unintended line breaks or splitting of arguments in the shell, it's recommended to enclose the prompt in double quotes.

```bash
./nino "What's the typical temperature range for a CPU while gaming?"
```

### Using `-model` and `-prompt` Arguments:

```bash
./nino -model llama3.1 -prompt "Which country has the most time zones?"
```

### Using `-prompt-file` Argument:

You can pass a text file containing the prompt using the `-prompt-file` flag:

```bash
./nino -model llama3.1 -prompt-file ./prompts/question.txt
```

This will read the contents of `question.txt` and send it as the prompt to the language model.

### Using an Alternative Model

This example uses all parameters with the `mistral` model. Ensure Ollama is running with `mistral`:

```bash
./nino -model mistral -prompt "What is the capital of Australia?" -url http://localhost:55555/api/generate -output result.txt
```

### Using an Output File

You can optionally save the model's output to a file while still printing it to the console with the following command:

```bash
./nino -model llama3.1 -prompt "What's the Japanese word for 'Thank you'?" -output answer.txt
```

### Using Command Substitution

You can dynamically generate input for nino by using shell command substitution with the $(...) syntax. This allows the output of a shell command to be used as a prompt input:

```bash
./nino "Analyze my project directory and suggest maintenance improvements: $(ls -la)"
```

Additionally, file content can be passed as input using shell redirection:

```bash
./nino "Review the following project details and provide feedback:" < project.txt
```

## Using Environment Variables

You can set environment variables to use as defaults for the `-model` and `-url` parameters if they are not passed on the command line.

### Setting `NINO_MODEL` and `nino_URL`

-   Set a default model:

    ```bash
    export NINO_MODEL="llama3.1"
    ```

-   Set a default URL:

    ```bash
    export NINO_URL="http://localhost:11434/api/generate"
    ```

When the environment variables are set, nino will use them as default values. You can still override them by passing `-model` and `-url` flags at runtime.

### Clearing Environment Variables

To clear an environment variable, use:

```bash
unset NINO_MODEL
unset NINO_URL
```

## Command-line Flags:

-   `-model` : The model to use (default: "llama3.1").
    -   **This must match the model that is currently running on Ollama.**
-   `-prompt` : The prompt to send to the language model (required unless `-prompt-file` is used).
-   `-prompt-file` : The path to a text file containing the prompt (optional).
    -   If both `-prompt` and `-prompt-file` are provided, `-prompt` takes precedence.
-   `-url` : The host and port where the Ollama server is running (optional).
    -   **The default `http://localhost:11434/api/generate` will be used if no URL is passed.**
-   `-output` : Specifies the filename where the model output will be saved (optional).

## Makefile Usage

The `Makefile` in the nino project automates several key tasks like building, testing, and cleaning the project. Below are the available commands and their usage:

### `make all`

The default target, `all`, runs both the `test` and `build` commands to ensure that tests pass before building the project.

```bash
make all
```

-   **This command will:**
    1. Run all unit tests (see `make test` for details).
    2. Build the nino binary (see `make build` for details).

### `make build`

This target compiles the nino binary. The build process is performed in the `src/cmd/nino` directory, and the output binary is placed at the root of the project directory.

```bash
make build
```

-   **This command will:**
    1. Navigate to the `src/cmd/nino` directory.
    2. Compile the Go project and generate the `nino` binary in the root directory.

### `make update-local-bin`

This command copies the freshly built `nino` binary to `/usr/local/bin`, updating the system-wide version of the binary.

```bash
make update-local-bin
```

-   **Note:** This command requires `sudo` privileges, as it modifies the `/usr/local/bin` directory.

### `make test`

Runs all unit tests located in the `src` directory. This ensures that the project is functioning correctly.

```bash
make test
```

-   **This command will:**
    1. Run the tests using Go's built-in test framework.
    2. Output the results of each test case.

### `make coverage`

This target runs all tests and generates a code coverage report. The report is saved as `coverage.html` in the `src` directory, which can be viewed in a browser.

```bash
make coverage
```

-   **This command will:**
    1. Run the tests and generate a coverage profile.
    2. Create an HTML report of the test coverage at `src/coverage.html`.

### `make clean`

Cleans up the project by removing the built binary and any generated coverage reports.

```bash
make clean
```

-   **This command will:**
    1. Delete the `nino` binary from the root directory.
    2. Remove the `coverage.out` and `coverage.html` files from the `src` directory.

### `make clean-coverage`

This target removes only the coverage report files, leaving the binary intact.

```bash
make clean-coverage
```

-   **This command will:**
    1. Delete the `coverage.out` and `coverage.html` files from the `src` directory.

### `make setup-git-hooks`

Configures Git to use a custom hooks directory (`git-hooks`) for pre-commit or other Git hook scripts.

```bash
make setup-git-hooks
```

-   **This command will:**
    1. Set the Git configuration to use the `git-hooks` directory.
    2. Ensure that all hooks are executable by running `chmod +x` on the files inside `git-hooks`.

## Acknowledgements

I would like to thank the developers of [Ollama](https://github.com/jmorganca/ollama) for providing the core tools that nino relies on. Additionally, a big thanks to the open-source community for creating the resources that made this project possible.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contribution

Contributions are welcome! Please fork the repository and submit a pull request if you'd like to propose any changes.
