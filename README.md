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

在命令行中与 ChatGPT 交互。

## 安装及配置

```bash
pip install askchat --upgrade
```

配置环境变量：

```bash
export OPENAI_API_KEY="your-api-key"
export OPENAI_API_BASE_URL="https://api.openai.com"
export OPENAI_API_BASE="https://api.openai.com/v1"
export OPENAI_MODEL="gpt-3.5-turbo"
```

注：`OPENAI_API_BASE` 变量优先于 `OPENAI_API_BASE_URL` 变量，二者选一即可。

## 使用方法

使用默认的环境变量简单地运行：

```bash
ask hello
```

## AskChat

通过 `askchat` 更灵活地使用参数和管理对话，工作目录为 `~/.askchat/`，支持选项如下：

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

### 默认参数

`askchat` 命令行工具的默认参数，这些参数用于直接与 ChatGPT 交互或者配置 API 的连接信息。

| 参数            | 示例            | 解释                               |
|-----------------|-----------------|-----------------------------------|
| `<message>`     | `askchat hello` | 最简单的对话          |
| `-m / --model`  | `-m gpt-3.5-turbo` | 指定使用的模型名称                  |
| `-b / --base-url` | `-b https://api.example.com` | 设置 Base URL (不包含 `/v1`) |
| `--api-base`    | `--api-base https://api.example.com/v1` | 设置 Base URL (包含 `/v1`)  |
| `-a / --api-key` | `-a sk-xxxxxxx` | 提供 OpenAI API 密钥                |
| `-u / --use-env` | `-u prod` | 使用指定配置文件加载环境变量，详见 `chatenv`     |

注：一些模型 API，比如智谱，使用 `/v4` 作为 API 的基础路径，这时可以使用 `--api-base` 参数。

### 对话管理

对话管理参数允许用户保存、加载、删除和列出对话历史记录，以及继续之前的对话。

| 参数                | 示例             | 解释                                       |
|---------------------|------------------|--------------------------------------------|
| `-c`                | `askchat -c`     | 继续上一次的对话                             |
| `-r / --regenerate` | `askchat -r`     | 重新生成上一次对话的最后回复                   |
| `-l / --load`       | `askchat -l conversation1` | 从文件加载对话历史，继续对话                  |
| `-p / --print`      | `askchat -p [name]`     | 打印上次或指定的对话历史                       |
| `-s / --save`       | `askchat -s conversation1` | 将当前对话历史保存到文件                      |
| `-d / --delete`     | `askchat -d conversation1` | 删除指定的对话历史文件                        |
| `--list`            | `askchat --list` | 列出所有保存的对话历史文件                     |

所有对话保存在 `~/.askchat/`，使用 `askchat` 的最近一次对话保存在 `~/.askchat/_last_chat.json` 文件。

### 其他选项

这些选项提供了一些辅助功能，如生成配置文件、调试日志、打印模型列表和显示版本信息。

| 参数                      | 示例                 | 解释                                       |
|---------------------------|----------------------|--------------------------------------------|
| `--generate-config`       | `askchat --generate-config` | 生成配置文件，保存在 `~/.askchat/.env` 中  |
| `--debug`                 | `askchat --debug`    | 打印调试日志                                |
| `--valid-models`          | `askchat --valid-models` | 打印包含 "gpt" 名称的有效模型列表            |
| `--all-valid-models`      | `askchat --all-valid-models` | 打印所有有效的模型列表                     |
| `-v / --version`          | `askchat -v`         | 打印 `askchat` 的版本信息                    |

注：`--all-valid-models` 会打印所有可用模型，包括 Embedding, dalle-3, tts 等，使用 `--valid-models` 可以过滤掉这些。

## ChatEnv

`chatenv` 用于管理不同的环境配置，支持创建、激活、删除等操作，便于在不同的环境之间切换，管理 API 密钥、模型名称和 API 的基础 URL 等配置信息。

1. 创建一个新的环境配置，使用 `create` 命令。

    ```bash
    chatenv create <name> [-a API_KEY] [-b BASE_URL] [--api-base API_BASE] [-m MODEL]
    ```

2. 激活某个环境，将其设置为当前使用的配置。

    ```bash
    chatenv use <name>
    ```

3. 删除指定的环境配置文件。

    ```bash
    chatenv delete <name>
    chatenv delete --default
    ```

4. 列出当前所有可用环境。

    ```bash
    chatenv list
    ```

5. 显示指定环境的配置信息，如果没有指定环境名称，则显示默认环境的配置。

    ```bash
    chatenv show [name]
    ```

6. 将当前激活的环境配置保存为指定名称的配置文件。

    ```bash
    chatenv save <name>
    ```

7. 更新指定或默认环境配置的一项或多项设置。

    ```bash
    chatenv config [name] [-a API_KEY] [-b BASE_URL] [--api-base API_BASE] [-m MODEL]
    ```