# island-cli

Command-line interface for the [Archipelag.io](https://archipelag.io) distributed compute network.

**[Full documentation](https://docs.archipelag.io/sdks/cli/)** · **[Quickstart](https://docs.archipelag.io/getting-started/quickstart/cli/)** · **[API Specs](https://app.archipelag.io/api/v1/docs)**

The binary is called `island`.

## Install

### From source

```bash
cargo install --git https://github.com/archipelag-io/island-cli
```

### From crates.io

```bash
cargo install island-cli
```

## Quick Start

```bash
# Authenticate with your API key
island auth login

# Chat with an AI model (streaming)
island chat "Explain quantum computing in one paragraph"

# Check your account balance
island account

# List available Cargos
island workloads list

# Submit a custom job
island jobs submit --workload llm-chat --input '{"prompt": "Hello!"}' --stream

# View Islands on the network
island hosts list

# Check market rates
island market rates
```

## Configuration

Config is stored at `~/.island/config.toml`:

```toml
api_key = "ak_your_key_here"
```

You can also use environment variables:

```bash
export ARCHIPELAG_API_KEY="ak_your_key_here"
export ARCHIPELAG_API_URL="https://api.archipelag.io"    # default
export ARCHIPELAG_NATS_URL="nats://sail.archipelag.io:4222"  # default
```

Precedence: CLI flag > environment variable > config file.

## Commands

| Command | Description |
|---------|-------------|
| `island auth login` | Save API key |
| `island auth status` | Show authentication status |
| `island auth logout` | Remove saved credentials |
| `island account` | Show account info and credits |
| `island chat <prompt>` | Chat with an AI model (streaming) |
| `island jobs list` | List your compute jobs |
| `island jobs submit` | Submit a new job |
| `island jobs get <id>` | Get job details |
| `island jobs stream <id>` | Stream job output |
| `island jobs cancel <id>` | Cancel a running job |
| `island workloads list` | List available Cargos |
| `island workloads get <slug>` | Get Cargo details |
| `island hosts list` | List online Islands |
| `island hosts get <id>` | Get Island details |
| `island api-keys list` | List API keys |
| `island api-keys create <name>` | Create a new API key |
| `island api-keys delete <id>` | Delete an API key |
| `island market rates` | Show current market rates |
| `island market history <slug>` | Price history for a Cargo |
| `island sail subscribe <subject>` | Subscribe to NATS messages |
| `island completion <shell>` | Generate shell completions |

## Output Formats

All commands support `--format json` for machine-readable output:

```bash
island jobs list --format json
island account --format json | jq '.credits'
```

## Chat Options

```bash
# With system prompt
island chat "Write a haiku" --system "You are a poet"

# Control generation
island chat "Hello" --max-tokens 100 --temperature 0.5

# Non-streaming (wait for complete response)
island chat "Hello" --no-stream

# Use a specific Cargo
island chat "Hello" --workload llm-chat
```

## Sail (NATS)

Subscribe to live messages on the Archipelag.io message fabric:

```bash
# Watch all heartbeats
island sail subscribe "host.*.heartbeat"

# Watch job status updates for a specific host
island sail subscribe "host.abc123.status"

# Limit to 10 messages
island sail subscribe "host.*.heartbeat" --max 10
```

## Shell Completions

```bash
# Bash
island completion bash >> ~/.bashrc

# Zsh
island completion zsh >> ~/.zshrc

# Fish
island completion fish > ~/.config/fish/completions/island.fish
```

## License

MIT
