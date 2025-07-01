# Elys Asset Registry

[![Better Stack Badge](https://uptime.betterstack.com/status-badges/v1/monitor/1z653.svg)](https://uptime.betterstack.com/?utm_source=status_badge)

The official Elys Network Asset Registry is a standardized source of blockchain and token configurations used across the Elys ecosystem. This registry powers frontend applications, backend services, wallets, SDKs, and third-party integrations by serving as the single source of truth for network metadata, token definitions, RPC endpoints, and capabilities.

📝 Official Listing Process:

This repository also functions as the official listing process for adding new assets and liquidity pools to the Elys Network. Projects that wish to be supported by the Elys ecosystem must submit a structured pull request here, following the defined schema and validation rules. Once merged, these assets become available across all Elys-enabled applications and services.

## 📚 Documentation

- [Listing New Assets and Pools](docs/listing-assets.md)
- [API Reference](docs/api.md)
- [Chain Schema](docs/SCHEMA.md)
- [Integration Examples](docs/integration.md)
- [Versioning](docs/versioning.md)

## 🚀 Quick Start

### Basic API Usage

```bash
# Get all mainnet chains
curl https://registry.elys.network/v1/chains/mainnet

# Get all currencies across mainnet networks
curl https://registry.elys.network/v1/currencies/mainnet

# Check API health
curl https://registry.elys.network/health
```

## 🤝 Contributing

To add assets or make changes, see [Listing New Assets](docs/listing-assets.md).

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
