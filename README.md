<!-- Banner -->
<p align="center">
  <!-- <img src="https://raw.githubusercontent.com/Laminar-Bot/.github/main/assets/banner.png" alt="Laminar Protocol" width="800"> -->
</p>

<h1 align="center">Laminar Protocol</h1>

<p align="center">
  <strong>Open-source tooling for Solana DeFi</strong>
</p>

<p align="center">
  <a href="https://laminar.bot">Website</a> •
  <a href="https://docs.laminar.bot">Docs</a> •
  <a href="https://twitter.com/laminarbot">Twitter</a> •
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
| [**helius-go**](https://github.com/Laminar-Bot/helius-go) | Helius RPC, webhooks, DAS API, enhanced transactions | `go get github.com/Laminar-Bot/helius-go` |
| [**jupiter-go**](https://github.com/Laminar-Bot/jupiter-go) | Jupiter swap quotes, transactions, price API | `go get github.com/Laminar-Bot/jupiter-go` |
| [**birdeye-go**](https://github.com/Laminar-Bot/birdeye-go) | Birdeye prices, analytics, wallet tracking, OHLCV | `go get github.com/Laminar-Bot/birdeye-go` |

### Security Tools

| Library | Description | Install |
|---------|-------------|---------|
| [**solana-token-guard**](https://github.com/Laminar-Bot/solana-token-guard) | Token safety screening - detect rugs, honeypots, concentrated holdings | `go get github.com/Laminar-Bot/solana-token-guard` |

---

## 🚀 Quick Start

### Get Token Price

```go
import "github.com/Laminar-Bot/jupiter-go"

client := jupiter.NewClient(jupiter.DefaultConfig())
price, _ := client.GetPrice(ctx, "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263")
fmt.Printf("BONK: $%s\n", price.Price)
```

### Execute a Swap

```go
import "github.com/Laminar-Bot/jupiter-go"

client := jupiter.NewClient(jupiter.DefaultConfig())

// Get quote: 1 SOL -> BONK
quote, _ := client.GetQuote(ctx, &jupiter.QuoteRequest{
    InputMint:   jupiter.NativeSOL,
    OutputMint:  "DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263",
    Amount:      jupiter.SOLToLamports(decimal.NewFromInt(1)),
    SlippageBps: 100,
})

// Build transaction
swap, _ := client.GetSwapTransaction(ctx, &jupiter.SwapRequest{
    QuoteResponse: quote,
    UserPublicKey: "YourWallet...",
})
// Sign and send swap.SwapTransaction
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
import "github.com/Laminar-Bot/helius-go"

client := helius.NewClient(helius.Config{APIKey: "..."})

// Create webhook for wallet monitoring
webhook, _ := client.CreateWebhook(ctx, &helius.CreateWebhookRequest{
    WebhookURL:       "https://your-server.com/webhook",
    AccountAddresses: []string{"WalletToWatch..."},
    TransactionTypes: []string{"SWAP"},
    WebhookType:      "enhanced",
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
│  │ jupiter-go  │  │ helius-go   │  │ solana-token-guard  │ │
│  │             │  │             │  │                     │ │
│  │ • Quotes    │  │ • RPC       │  │ • Safety checks     │ │
│  │ • Swaps     │  │ • Webhooks  │  │ • Rug detection     │ │
│  │ • Prices    │  │ • DAS API   │  │ • Risk scoring      │ │
│  └──────┬──────┘  └──────┬──────┘  └──────────┬──────────┘ │
│         │                │                     │            │
│         └────────────────┼─────────────────────┘            │
│                          │                                  │
│                   ┌──────┴──────┐                          │
│                   │ birdeye-go  │                          │
│                   │             │                          │
│                   │ • Prices    │                          │
│                   │ • Analytics │                          │
│                   │ • Wallets   │                          │
│                   └─────────────┘                          │
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

| Feature | helius-go | jupiter-go | birdeye-go | token-guard |
|---------|:---------:|:----------:|:----------:|:-----------:|
| Token Prices | | ✅ | ✅ | |
| Swap Execution | | ✅ | | |
| RPC Methods | ✅ | | | |
| Webhooks | ✅ | | | |
| Token Metadata | ✅ | | ✅ | |
| Wallet Tracking | | | ✅ | |
| Holder Analysis | ✅ | | ✅ | ✅ |
| Liquidity Data | | | ✅ | ✅ |
| Security Checks | | | | ✅ |
| OHLCV Charts | | | ✅ | |

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

- **Discord** - [discord.gg/laminar](https://discord.gg/laminar) - Chat with the team and community
- **Twitter** - [@laminarbot](https://twitter.com/laminarbot) - Updates and announcements
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
