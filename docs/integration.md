# 🔧 Integration Examples

Code examples for consuming the registry from various programming languages.

## 🎯 Purpose

- **Universal Access**: Consumable by any programming language (Go, Java, JavaScript, C++, Python, Rust)
- **Centralized Configuration**: Single source of truth for all blockchain assets
- **Standardized Format**: Standardized data structure with JSON Schema validation
- **Automatic Updates**: Centralized updates that propagate to all applications
- **Multi-Environment**: Support for mainnet, testnet and devnet

## Languages

- [JavaScript / Node.js](#javascript--nodejs)
- [Go](#go)
- [Python](#python)
- [Java](#java)
- [Rust](#rust)

### JavaScript/Node.js

```javascript
const response = await fetch("https://registry.elys.network/v1/chains/mainnet");
const registry = await response.json();

const elysChain = registry.chains.elys;
console.log(`RPC URL: ${elysChain.rpcURL}`);

const swappableCurrencies = Object.values(registry.chains)
  .flatMap((chain) => chain.currencies)
  .filter((currency) => currency.canSwap);
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
