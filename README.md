# Gollama CLI Tool

![gollama-banner-v1](https://github.com/user-attachments/assets/6dec07fd-2adb-4eb8-8e67-5b2d9445c209)

## About Gollama

Gollama is a Go-based command-line tool that simplifies interaction with local language models served by [Ollama](https://github.com/jmorganca/ollama). It allows you to send prompts to models, receive real-time streaming responses directly in your terminal, and configure models using straightforward command-line arguments.

Gollama enhances the basic interaction provided by Ollama by displaying full model responses in the terminal and enabling you to save outputs to a file, offering a seamless experience for working with language models.

## Demo

![btcq_demo](https://github.com/user-attachments/assets/180b8196-d1d2-44b9-95f3-19deeed4d808)

## Features

-   **Streaming Responses**: Gollama handles and prints streaming responses generated by the Ollama CLI as the model processes the input.
-   **Command Execution**: Gollama leverages the Ollama CLI to interact with language models by invoking commands and processing the output.

## Ollama Dependency

Gollama relies on the [Ollama CLI tool](https://github.com/jmorganca/ollama) to interact with local language models. Ollama must be installed and running on your machine or server for Gollama to function properly.

### Install Ollama

Follow the instructions on the [Ollama GitHub repository](https://github.com/jmorganca/ollama) to install and set up Ollama. Ensure that Ollama is available in your system’s `$PATH`.

### Start the Ollama Server

Once Ollama is installed, you need to start the server:

1. **Start the Ollama Server**: This command will run the Ollama server on `localhost:11434` (default url and port).

    ```bash
    ollama serve
    ```

2. **Run the Model**: To run the desired model (e.g., `llama3.1`), execute the following command in a separate terminal window:
    ```bash
    ollama run llama3.1
    ```

> **Note:** The `-model` parameter in Gollama **must match** the model that you run on Ollama. For example, if you start `llama3.1` in Ollama, you must pass `llama3.1` as the `-model` in Gollama. Otherwise, Gollama will not be able to communicate with the correct model.

Ollama should now be running, and Gollama can interact with it by sending prompts.

## Requirements

-   Go 1.23+ installed on your system
-   [Ollama](https://github.com/jmorganca/ollama) installed and running locally or on your server
-   Ensure that the Ollama server is running via `ollama serve`

## Installation

1. Clone this repository:

    ```bash
    git clone https://github.com/lucianoayres/gollama.git
    cd gollama
    ```

2. Build the project:

    ```bash
    make build
    ```

## Usage

After building the project and ensuring that the Ollama server is running, you can run Gollama with the following commands:

```bash
./gollama -model "llama3.1" -prompt "Explain LLMs like I'm five"
```

### Command-line Flags:

-   `-model` : The model to use (default: "llama3.1"). **This must match the model that is currently running on Ollama.**
-   `-prompt` : The prompt to send to the language model (required)
-   `-url` : The host and port where the Ollama server is running (optional) **The default `localhost:11434` will be used if no url is passed.**

If no prompt is provided, a default prompt will be sent.

### Example

```bash
./gollama -model llama3.1 -prompt "What is the capital of France?"
```

### Output

Gollama will print the model's response to the console as it receives streaming output by invoking the Ollama CLI.

#### Saving the Output to a File

You can optionally save the model's output to a file with the following command:

```bash
./gollama -model llama3.1 -prompt "What is the capital of France?" | tee answer.txt
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contribution

Contributions are welcome! Please fork the repository and submit a pull request if you'd like to propose any changes.
