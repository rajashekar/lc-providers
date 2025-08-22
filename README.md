# LC Providers Repository

This repository contains provider configurations for the [lc (LLM Client)](https://github.com/rajashekar/lc) tool. It serves as the central registry for installable LLM provider configurations.

## Structure

```
lc-providers/
├── registry.json          # Provider registry with metadata
├── providers/            # Provider configuration files
│   ├── openai.toml
│   ├── anthropic.toml
│   ├── gemini.toml
│   └── ...
└── README.md
```

## How It Works

1. **Registry File (`registry.json`)**: Contains metadata about all available providers including:
   - Provider name and description
   - Configuration file name
   - Version information
   - Authentication type required
   - Documentation URLs
   - Tags for categorization

2. **Provider Configurations (`providers/*.toml`)**: Individual TOML files containing:
   - API endpoints
   - Model paths
   - Request/response templates
   - Custom headers
   - Provider-specific settings

## Using Providers with LC

### Installing a Provider

```bash
# Install a provider (e.g., OpenAI)
lc providers install openai
# or shorthand
lc p i openai
```

### Listing Available Providers

```bash
# Show all available providers from registry
lc providers available
```

### Listing Installed Providers

```bash
# Show installed providers with auth status
lc providers list
```

### Updating Providers

```bash
# Update a specific provider
lc providers upgrade openai

# Update all installed providers
lc providers upgrade --all
```

### Removing a Provider

```bash
lc providers remove openai
```

## Adding a New Provider

To add a new provider to this repository:

1. **Create the provider configuration file** in `providers/` directory:
   ```toml
   # providers/your-provider.toml
   endpoint = "https://api.your-provider.com/v1"
   models_path = "/models"
   chat_path = "/chat/completions"
   
   [headers]
   # Any required headers
   
   [chat_templates."model-pattern"]
   request = """
   {
     "model": "{{ model }}",
     "messages": {{ messages | json }}
   }
   """
   ```

2. **Add the provider to `registry.json`**:
   ```json
   "your-provider": {
     "name": "Your Provider Name",
     "description": "Description of your provider",
     "config_file": "your-provider.toml",
     "version": "1.0.0",
     "auth_type": "api_key",
     "tags": ["chat"],
     "official": false,
     "docs_url": "https://docs.your-provider.com"
   }
   ```

3. **Submit a Pull Request** with your changes

## Authentication Types

Providers can use different authentication methods:

- `api_key`: Standard API key authentication
- `service_account`: Service account JSON (e.g., Google Cloud)
- `oauth`: OAuth 2.0 flow
- `token`: Bearer token authentication
- `headers`: Custom authentication headers
- `none`: No authentication required

## Environment Variables

- `LC_PROVIDER_REGISTRY`: Override the default registry URL
  ```bash
  export LC_PROVIDER_REGISTRY="https://your-custom-registry.com"
  ```

## Contributing

Contributions are welcome! Please ensure:

1. Provider configurations are valid TOML
2. All required fields are present in registry.json
3. Version numbers follow semantic versioning
4. Documentation URLs are valid and accessible

## License

This repository is licensed under the MIT License. See [LICENSE](LICENSE) file for details.