<!-- Banner -->
<p align="center">
  <img src="https://raw.githubusercontent.com/Laminar-Bot/.github/main/assets/banner.png" alt="Laminar Protocol" width="800">
</p>

<h1 align="center">Laminar Protocol</h1>

<p align="center">
  <strong>Open-source tooling for Solana DeFi</strong>
</p>

<p align="center">
  <a href="https://laminar.bot">Website</a> •
  <a href="https://docs.laminar.bot">Docs</a> •
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Solana-black?style=flat&logo=solana&logoColor=14F195" alt="Solana">
  <img src="https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white" alt="Go">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT">
</p>

---

## What We Build

We're building infrastructure for safe, automated trading on Solana. Our open-source libraries power DeFi applications, trading bots, and analytics platforms.

## 📦 Open Source Libraries

### API Clients

Production-ready Go clients for Solana's core DeFi infrastructure:

| Library | Description | Install |
|---------|-------------|---------|
| [**helius-go**](https://github.com/Laminar-Bot/helius-go) | Helius RPC, webhooks, DAS API, priority fees, token holders | `go get github.com/Laminar-Bot/helius-go` |
| [**birdeye-go**](https://github.com/Laminar-Bot/birdeye-go) | Birdeye prices, token security, token overview | `go get github.com/Laminar-Bot/birdeye-go` |

> **Note:** For Jupiter swap execution, we recommend [ilkamo/jupiter-go](https://github.com/ilkamo/jupiter-go) - a well-maintained community library.

### Security Tools

| Library | Description | Install |
|---------|-------------|---------|
| [**solana-token-guard**](https://github.com/Laminar-Bot/solana-token-guard) | Token safety screening - detect rugs, honeypots, concentrated holdings | `go get github.com/Laminar-Bot/solana-token-guard` |

---

## 🚀 Quick Start

### Get Token Price

```go
import birdeye "github.com/Laminar-Bot/birdeye-go"

client, _ := birdeye.NewClient("your-api-key")
price, _ := client.GetPrice(ctx, "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263")
fmt.Printf("BONK: $%s\n", price.Value)
```

### Execute a Swap (using ilkamo/jupiter-go)

```go
import "github.com/ilkamo/jupiter-go"

client, _ := jupiter.NewClient(jupiter.DefaultConfig())

// Get quote: 1 SOL -> BONK
quote, _ := client.GetQuote(ctx, jupiter.GetQuoteParams{
    InputMint:   "So11111111111111111111111111111111111111112",
    OutputMint:  "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263",
    Amount:      1_000_000_000, // 1 SOL in lamports
    SlippageBps: 100,
})

// Build and sign transaction...
```

### Screen Token Safety

```go
import "github.com/Laminar-Bot/solana-token-guard"

guard := tokenguard.New(tokenguard.Config{
    Helius:  heliusClient,
    Birdeye: birdeyeClient,
})

result, _ := guard.Screen(ctx, "TokenMint...", tokenguard.LevelNormal)
if result.Passed {
    fmt.Printf("✅ Safe (score: %d/100)\n", result.Score)
} else {
    fmt.Printf("❌ Failed: %v\n", result.FailedChecks)
}
```

### Subscribe to Wallet Activity

```go
import helius "github.com/Laminar-Bot/helius-go"

client, _ := helius.NewClient("your-api-key")

// Create webhook for wallet monitoring
webhook, _ := client.CreateWebhook(ctx, &helius.CreateWebhookRequest{
    WebhookURL:       "https://your-server.com/webhook",
    AccountAddresses: []string{"WalletToWatch..."},
    TransactionTypes: []string{"SWAP"},
    WebhookType:      helius.WebhookTypeEnhanced,
})
```

---

## 🏗️ Architecture

Our libraries are designed to work together seamlessly:

```
┌─────────────────────────────────────────────────────────────┐
│                     Your Application                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │ helius-go   │  │ birdeye-go  │  │ solana-token-guard  │ │
│  │             │  │             │  │                     │ │
│  │ • RPC       │  │ • Prices    │  │ • Safety checks     │ │
│  │ • Webhooks  │  │ • Security  │  │ • Rug detection     │ │
│  │ • DAS API   │  │ • Overview  │  │ • Risk scoring      │ │
│  │ • Holders   │  │             │  │                     │ │
│  └──────┬──────┘  └──────┬──────┘  └──────────┬──────────┘ │
│         │                │                     │            │
│         └────────────────┴─────────────────────┘            │
│                          │                                  │
│              ┌───────────┴───────────┐                     │
│              │  ilkamo/jupiter-go    │                     │
│              │  (community library)  │                     │
│              │  • Quotes & Swaps     │                     │
│              └───────────────────────┘                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │  Solana Network │
                    └─────────────────┘
```

---

## 📊 Feature Matrix

| Feature | helius-go | birdeye-go | token-guard |
|---------|:---------:|:----------:|:-----------:|
| Token Prices | | ✅ | |
| RPC Methods | ✅ | | |
| Webhooks | ✅ | | |
| Token Metadata | ✅ | ✅ | |
| Holder Analysis | ✅ | | ✅ |
| Token Security | | ✅ | ✅ |
| Priority Fees | ✅ | | |
| DAS API | ✅ | | |

---

## 🔒 Security

All libraries follow security best practices:

- **No credential storage** - API keys passed at initialization only
- **Input validation** - All inputs sanitized
- **Error handling** - No sensitive data in error messages
- **Dependency auditing** - Regular `go mod audit`

Found a security issue? Email security@laminar.bot

---

## 🤝 Contributing

We welcome contributions! Each repository has its own contributing guide:

1. Fork the repository
2. Create a feature branch
3. Write tests for your changes
4. Submit a pull request

Please read our [Code of Conduct](https://github.com/Laminar-Bot/.github/blob/main/CODE_OF_CONDUCT.md) before contributing.

---

## 📄 License

All open-source repositories are released under the [MIT License](https://opensource.org/licenses/MIT).

---

## 💬 Community

- **GitHub Discussions** - Ask questions and share ideas

---

## 🛠️ Built With These Libraries

Using our libraries? Let us know! Open a PR to add your project here.

<!--
- [Project Name](link) - Brief description
-->

---

<p align="center">
  <sub>Built with ☕ for the Solana ecosystem</sub>
</p>
