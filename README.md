# Elys Asset Registry
[![Better Stack Badge](https://uptime.betterstack.com/status-badges/v1/monitor/1z653.svg)](https://uptime.betterstack.com/?utm_source=status_badge)

Official Elys Network asset registry providing standardized blockchain and token configurations for all Elys ecosystem projects. This registry serves as the single source of truth for chain metadata, RPC endpoints, token information, and feature capabilities across the entire Elys ecosystem.

## 🎯 Purpose

- **Universal Access**: Consumable by any programming language (Go, Java, JavaScript, C++, Python, Rust)
- **Centralized Configuration**: Single source of truth for all blockchain assets
- **Standardized Format**: Standardized data structure with JSON Schema validation
- **Automatic Updates**: Centralized updates that propagate to all applications
- **Multi-Environment**: Support for mainnet, testnet and devnet

## 🚀 Quick Start

### Basic Usage

```bash
# Get all mainnet chains
curl https://registry.elys.network/v1/chains/mainnet

# Get all currencies across mainnet networks
curl https://registry.elys.network/v1/currencies/mainnet

# Check API health
curl https://registry.elys.network/health
```

## 📡 API Endpoints

### Chain Endpoints

| Method | Endpoint | Description | Status |
|--------|----------|-------------|--------|
| `GET` | `/v1/chains/mainnet` | List all mainnet chains | ✅ Available |
| `GET` | `/v1/chains/testnet` | List all testnet chains | ✅ Available |
| `GET` | `/v1/chains/devnet`  | List all devnet chains | ✅ Available |

### Currency Endpoints

| Method | Endpoint | Description | Status |
|--------|----------|-------------|--------|
| `GET` | `/v1/currencies/mainnet` | List all currencies across mainnet network | ✅ Available |
| `GET` | `/v1/currencies/testnet` | List all currencies across testnet network | ✅ Available  |
| `GET` | `/v1/currencies/devnet` | List all currencies across devnet networks |  ✅ Available  |

### Utility Endpoints

| Method | Endpoint | Description | Status |
|--------|----------|-------------|--------|
| `GET` | `/health` | API health status | ✅ Available |

## 📊 Data Structure

```json
{
  "chains": {
    "elys": {
      "chainId": "elys-1",
      "chainName": "Elys",
      "addressPrefix": "elys",
      "rpcURL": "https://rpc.elys.network:443",
      "restURL": "https://api.elys.network:443",
      "explorerURL": {
        "transaction": "https://mainnet.itrocket.net/elys/tx/{transaction}"
      },
      "channel": {
        "source": "",
        "destination": ""
      },
      "isEnabled": true,
      "priority": 1,
      "currencies": [
        {
          "coinDenom": "ELYS",
          "coinMinimalDenom": "uelys",
          "coinIbcDenom": "",
          "coinDecimals": 6,
          "coinGeckoId": "elys",
          "coinImageUrl": "/tokens/elys.svg",
          "isFeeCurrency": true,
          "isStakeCurrency": false,
          "canSwap": true,
          "canWithdraw": true,
          "canDeposit": true,
          "canUseLiquidityMining": true,
          "canUseLeverageLP": true,
          "canUsePerpetual": false,
          "canUseVaults": true,
          "gasPriceStep": {
            "low": 0.01,
            "average": 0.025,
            "high": 0.03
          }
        }
      ]
    }
  }
}
```

## 🔧 Integration Examples

### JavaScript/Node.js
```javascript

const response = await fetch('https://registry.elys.network/v1/chains/mainnet');
const registry = await response.json();

const elysChain = registry.chains.elys;
console.log(`RPC URL: ${elysChain.rpcURL}`);

const swappableCurrencies = Object.values(registry.chains)
  .flatMap(chain => chain.currencies)
  .filter(currency => currency.canSwap);
```

### Go
```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
)

type AssetRegistry struct {
    Chains map[string]ChainAsset `json:"chains"`
}

type ChainAsset struct {
    ChainID      string     `json:"chainId"`
    ChainName    string     `json:"chainName"`
    RPCURL       string     `json:"rpcURL"`
    RestURL      string     `json:"restURL"`
    Currencies   []Currency `json:"currencies"`
}

type Currency struct {
    CoinDenom        string  `json:"coinDenom"`
    CoinMinimalDenom string  `json:"coinMinimalDenom"`
    CoinDecimals     int     `json:"coinDecimals"`
    CanSwap          bool    `json:"canSwap"`
    CanDeposit       bool    `json:"canDeposit"`
}

func main() {
    resp, err := http.Get("https://registry.elys.network/v1/chains/mainnet")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    var registry AssetRegistry
    json.NewDecoder(resp.Body).Decode(&registry)

    // Usar la chain de Elys
    elysChain := registry.Chains["elys"]
    fmt.Printf("Elys RPC: %s\n", elysChain.RPCURL)
}
```

### Java
```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.URI;

public class ElysRegistryClient {
    private static final String REGISTRY_URL = "https://registry.elys.network/v1/chains/mainnet";
    
    public static class AssetRegistry {
        public Map<String, ChainAsset> chains;
    }
    
    public static class ChainAsset {
        public String chainId;
        public String chainName;
        public String rpcURL;
        public String restURL;
        public List<Currency> currencies;
    }
    
    public static class Currency {
        public String coinDenom;
        public String coinMinimalDenom;
        public int coinDecimals;
        public boolean canSwap;
        public boolean canDeposit;
    }
    
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create(REGISTRY_URL))
            .build();
            
        HttpResponse<String> response = client.send(request, 
            HttpResponse.BodyHandlers.ofString());
            
        ObjectMapper mapper = new ObjectMapper();
        AssetRegistry registry = mapper.readValue(response.body(), AssetRegistry.class);
        
        ChainAsset elys = registry.chains.get("elys");
        System.out.println("Elys RPC: " + elys.rpcURL);
    }
}
```

### Python
```python
import requests
import json
from dataclasses import dataclass
from typing import List, Dict, Optional

@dataclass
class Currency:
    coinDenom: str
    coinMinimalDenom: str
    coinDecimals: int
    canSwap: bool
    canDeposit: bool

@dataclass
class ChainAsset:
    chainId: str
    chainName: str
    rpcURL: str
    restURL: str
    currencies: List[Currency]

@dataclass
class AssetRegistry:
    chains: Dict[str, ChainAsset]

def load_registry() -> AssetRegistry:
    response = requests.get("https://registry.elys.network/mainnet")
    data = response.json()
    
    chains = {}
    for key, chain_data in data["chains"].items():
        currencies = [Currency(**curr) for curr in chain_data["currencies"]]
        chains[key] = ChainAsset(**{**chain_data, "currencies": currencies})
    
    return AssetRegistry(chains=chains)

# Uso
registry = load_registry()
elys_chain = registry.chains["elys"]
print(f"Elys RPC: {elys_chain.rpcURL}")
```

### Rust
```rust
use serde::{Deserialize, Serialize};
use std::collections::HashMap;

#[derive(Serialize, Deserialize, Debug)]
struct Currency {
    #[serde(rename = "coinDenom")]
    coin_denom: String,
    #[serde(rename = "coinMinimalDenom")]
    coin_minimal_denom: String,
    #[serde(rename = "coinDecimals")]
    coin_decimals: u8,
    #[serde(rename = "canSwap")]
    can_swap: bool,
    #[serde(rename = "canDeposit")]
    can_deposit: bool,
}

#[derive(Serialize, Deserialize, Debug)]
struct ChainAsset {
    #[serde(rename = "chainId")]
    chain_id: String,
    #[serde(rename = "chainName")]
    chain_name: String,
    #[serde(rename = "rpcURL")]
    rpc_url: String,
    #[serde(rename = "restURL")]
    rest_url: String,
    currencies: Vec<Currency>,
}

#[derive(Serialize, Deserialize, Debug)]
struct AssetRegistry {
    chains: HashMap# API Endpoints y Consumo Multi-Plataforma

```
## 📁 Repository Structure

```
elys-asset-registry/
├── 📁 data/
│   ├── 📁 mainnet/       
│   ├──── elys.json
│   ├──── cosmos.json
│   ├── 📁 testnet/
│   ├──── elys.json
│   ├──── cosmos.json              
├── 📁 schema/
│   └── asset-registry.schema.json  # JSON Schema
├── 📁 examples/
│   ├── javascript/             # JavaScript examples
│   ├── go/                     # Go examples
│   ├── java/                   # Java examples
│   ├── python/                 # Python examples
│   └── rust/                   # Rust examples
├── 📁 .github/
│   └── workflows/
│       └─ validate-registry.yml        CI for validatio     
├──📄 .version
└──📄 README.md
```

### Validation Rules

- ✅ Valid and well-formatted JSON
- ✅ Complies with defined JSON Schema
- ✅ No duplicate chainIds
- ✅ Valid URLs for RPC/REST endpoints
- ✅ Required properties present
- ✅ Valid date formats

## 🤝 Contributing

### Adding New Assets

1. Fork the repository
2. Add asset to `data/mainnet/chain-key.json` or `data/testnet/chain-key.json` following the schema
3. Create pull request

### Chain Asset Schema

Each chain asset must include:

```json
{
    "chainId": "elys-1",
    "chainName": "Elys",
    "addressPrefix": "elys",
    "rpcURL": "https://rpc.elys.network:443",
    "restURL": "https://api.elys.network:443",
    "explorerURL": {
        "transaction": "https://mainnet.itrocket.net/elys/tx/{transaction}"
    },
    "channel": {
        "source": "",
        "destination": ""
    },
    "currencies": [
        {
            "coinDenom": "ELYS",
            "coinDisplayDenom": "Elys",
            "coinMinimalDenom": "uelys",
            "coinIbcDenom": "",
            "coinDecimals": 6,
            "coinGeckoId": "elys",
            "canSwap": true,
            "isFeeCurrency": true,
            "isStakeCurrency": true,
            "canWithdraw": true,
            "canDeposit": true,
            "canUseLiquidityMining": true,
            "canUseLeverageLP": false,
            "canUsePerpetual": false,
            "canUseVaults": true,
            "gasPriceStep": {
                "low": 0.01,
                "average": 0.025,
                "high": 0.03
            }
        }
    ]
}
```

### Quality Standards

- 🎯 **Accuracy**: Verified and updated information
- 🔒 **Reliability**: Functional and stable endpoints
- 📊 **Completeness**: All required information present
- 🚀 **Performance**: Endpoints with good latency
- 🔄 **Maintenance**: Regularly updated information

## 🔄 Versioning

We follow [Semantic Versioning](https://semver.org/):

- **MAJOR**: Breaking changes to data structure
- **MINOR**: New assets or compatible features
- **PATCH**: Bug fixes and data updates

## 🛠️ Used By

This registry is used by:

- **Frontend Applications**: Elys web and mobile applications
- **Backend Services**: APIs and microservices
- **CLI Tools**: Command line tools
- **Third-party Integrations**: DeFi and wallet integrations
- **Development Tools**: SDKs and frameworks

## 📊 Statistics

- 📦 **Total Assets**: Multiple supported chains
- 🌍 **Networks**: Mainnet, Testnet, Devnet
- 🔄 **Update Frequency**: Updates as needed
