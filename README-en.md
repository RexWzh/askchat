# AskChat

<div align="center">
    <a href="https://pypi.python.org/pypi/askchat">
        <img src="https://img.shields.io/pypi/v/askchat.svg" alt="PyPI version" />
    </a>
    <a href="https://github.com/cubenlp/askchat/actions/workflows/test.yml">
        <img src="https://github.com/cubenlp/askchat/actions/workflows/test.yml/badge.svg" alt="Tests" />
    </a>
    <a href="https://cubenlp.github.io/askchat/">
        <img src="https://img.shields.io/badge/docs-github_pages-blue.svg" alt="Documentation Status" />
    </a>
    <a href="https://codecov.io/gh/cubenlp/askchat">
        <img src="https://codecov.io/gh/cubenlp/askchat/branch/main/graph/badge.svg" alt="Coverage" />
    </a>
</div>

<div align="center">
<img src="https://qiniu.wzhecnu.cn/PicBed6/picgo/askchat.jpeg" alt="Ask Chat" width="256">

[English](README-en.md) | [简体中文](README.md)
</div>

Invoke ChatGPT from the command line.

## Installation and Configuration

```bash
pip install askchat --upgrade
```

Configure environment variables:

```bash
export OPENAI_API_KEY="your-api-key"
export OPENAI_API_BASE_URL="https://api.openai.com"
export OPENAI_API_BASE="https://api.openai.com/v1"
export OPENAI_MODEL="gpt-3.5-turbo"
```

Note: The `OPENAI_API_BASE` variable takes precedence over `OPENAI_API_BASE_URL`. Use one of them as needed.

## Usage

To run with the default environment variables:

```bash
ask hello
```

## AskChat

Use `askchat` for more flexibility with parameters and to manage conversations, with related files saved under `~/.askchat/`. Supported options include:

```bash
❯ askchat --help
Usage: askchat [OPTIONS] [MESSAGE]...

  Interact with ChatGPT in terminal via chattool

Options:
  -m, --model TEXT     Model name
  -b, --base-url TEXT  Base URL of the API (without suffix `/v1`)
  --api-base TEXT      Base URL of the API (with suffix `/v1`)
  -a, --api-key TEXT   OpenAI API key
  -u, --use-env TEXT   Use environment variables from the ENV_PATH
  -c                   Continue the last conversation
  -r, --regenerate     Regenerate the last conversation
  -l, --load TEXT      Load the conversation from a file
  -p, --print          Print the last conversation or a specific conversation
  -s, --save TEXT      Save the conversation to a file
  -d, --delete TEXT    Delete the conversation from a file
  --list               List all the conversation files
  --generate-config    Generate a configuration file by environment table
  --debug              Print debug log
  --valid-models       Print valid models that contain "gpt" in their names
  --all-valid-models   Print all valid models
  -v, --version        Print the version
  --help               Show this message and exit.
```

### Default Parameters

Default parameters for the `askchat` command-line tool, used for direct interaction with ChatGPT or configuring API connection information.

| Parameter          | Example                   | Description                        |
|--------------------|---------------------------|------------------------------------|
| `<message>`        | `askchat hello`           | The simplest form of dialogue      |
| `-m / --model`     | `-m gpt-3.5-turbo`        | Specify the model name             |
| `-b / --base-url`  | `-b https://api.example.com` | Set the Base URL (excluding `/v1`) |
| `--api-base`       | `--api-base https://api.example.com/v1` | Set the Base URL (including `/v1`) |
| `-a / --api-key`   | `-a sk-xxxxxxx`           | Provide the OpenAI API key         |
| `-u / --use-env`   | `-u prod`                 | Load environment variables from the specified config file, see `chatenv` |

Note: For some model APIs, like ChatGPT, `/v4` is used as the base path of the API. In such cases, use the `--api-base` parameter.

### Conversation Management

Conversation management parameters allow users to save, load, delete, and list conversation histories, as well as continue a previous conversation.

| Parameter              | Example                         | Description                                    |
|------------------------|---------------------------------|------------------------------------------------|
| `-c`                   | `askchat -c`                    | Continue the last conversation                 |
| `-r / --regenerate`    | `askchat -r`                    | Regenerate the last response of the conversation |
| `-l / --load`          | `askchat -l conversation1`      | Load conversation history from a file and continue |
| `-p / --print`         | `askchat -p [name]`             | Print

 the last or a specified conversation history |
| `-s / --save`          | `askchat -s conversation1`      | Save the current conversation history to a file   |
| `-d / --delete`        | `askchat -d conversation1`      | Delete a specified conversation history file     |
| `--list`               | `askchat --list`                | List all saved conversation history files        |

All conversations are saved in `~/.askchat/`, with the most recent conversation saved in `~/.askchat/_last_chat.json`.

### Other Options

These options provide auxiliary functions, such as generating configuration files, printing debug logs, listing models, and showing version information.

| Parameter                | Example                       | Description                                |
|--------------------------|-------------------------------|--------------------------------------------|
| `--generate-config`      | `askchat --generate-config`   | Generate a config file, saved in `~/.askchat/.env` |
| `--debug`                | `askchat --debug`             | Print debug logs                           |
| `--valid-models`         | `askchat --valid-models`      | Print valid models containing "gpt" in their names |
| `--all-valid-models`     | `askchat --all-valid-models`  | Print all valid models                     |
| `-v / --version`         | `askchat -v`                  | Print the version of `askchat`             |

Note: `--all-valid-models` prints all available models, including Embedding, dalle-3, tts, etc. Use `--valid-models` to filter these out.

## ChatEnv

`chatenv` is a command-line tool designed for managing different environment configurations for `askchat`, supporting operations such as create, activate, delete, etc. It facilitates switching between different environments, managing API keys, model names, and API base URLs.

1. Create a new environment configuration using the `create` command.

    ```bash
    chatenv create <name> [-a API_KEY] [-b BASE_URL] [--api-base API_BASE] [-m MODEL]
    ```

2. Activate an environment, setting it as the current configuration.

    ```bash
    chatenv use <name>
    ```

3. Delete a specified environment configuration file.

    ```bash
    chatenv delete <name>
    chatenv delete --default
    ```

4. List all available environments.

    ```bash
    chatenv list
    ```

5. Display the configuration information of a specified environment, or the default environment if no name is provided.

    ```bash
    chatenv show [name]
    ```

6. Save the currently active environment configuration to a file with a specified name.

    ```bash
    chatenv save <name>
    ```

7. Update one or more settings of a specified or default environment configuration.

    ```bash
    chatenv config [name] [-a API_KEY] [-b BASE_URL] [--api-base API_BASE] [-m MODEL]
    ```