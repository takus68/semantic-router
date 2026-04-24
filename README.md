# Semantic Router

A Go implementation of semantic routing for LLM applications — Fork of [vllm-project/semantic-router](https://github.com/vllm-project/semantic-router).

Semantic Router is a superfast decision-making layer for your LLMs and agents. Rather than waiting for slow LLM generations to make tool-use decisions, we use the magic of semantic vector space to make those decisions — routing requests to the most appropriate handler based on meaning, not just keywords.

## Features

- **Semantic Routing**: Route requests based on semantic similarity using vector embeddings
- **Multiple Encoders**: Support for various embedding providers (OpenAI, local models, etc.)
- **Fast Inference**: Decision-making without LLM round-trips
- **Kubernetes Native**: CRD-based configuration for cloud-native deployments
- **Extensible**: Plugin architecture for custom encoders and route handlers

## Getting Started

### Prerequisites

- Go 1.21+
- Kubernetes 1.26+ (for CRD-based deployments)

### Installation

```bash
go get github.com/vllm-project/semantic-router
```

### Quick Start

```go
package main

import (
    "fmt"
    "log"

    router "github.com/vllm-project/semantic-router"
)

func main() {
    // Define routes
    routes := []router.Route{
        {
            Name: "politics",
            Utterances: []string{
                "isn't politics the best thing ever",
                "why don't you tell me about your political opinions",
                "don't you just love the president",
            },
        },
        {
            Name: "chitchat",
            Utterances: []string{
                "how's the weather today?",
                "how are things going?",
                "lovely day isn't it",
            },
        },
    }

    // Initialize router with default encoder
    sr, err := router.New(routes)
    if err != nil {
        log.Fatal(err)
    }

    // Route a query
    result, err := sr.Route("don't you love politics?")
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("Routed to: %s\n", result.Name)
}
```

## Architecture

```
┌─────────────────────────────────────────┐
│              Semantic Router            │
├─────────────┬───────────────────────────┤
│   Encoder   │      Route Index          │
│  (Embedder) │  (Vector Similarity)      │
├─────────────┴───────────────────────────┤
│              Route Handlers             │
└─────────────────────────────────────────┘
```

## Configuration

Semantic Router can be configured via:

- **Go API**: Programmatic configuration
- **YAML/JSON**: File-based configuration
- **Kubernetes CRDs**: Cloud-native configuration (see `.crd-ref-docs.yaml`)

## Development

```bash
# Clone the repository
git clone https://github.com/vllm-project/semantic-router
cd semantic-router

# Install dependencies
go mod download

# Run tests
go test ./...

# Build
go build ./...
```

## Contributing

Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.
