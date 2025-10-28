# Setup Krew Action

A GitHub Action to install [krew](https://krew.sigs.k8s.io/), the kubectl plugin manager, with optional kubectl installation and plugin management.

## Features

- 🔧 Automatically installs krew plugin manager
- 🎯 Detects and installs kubectl if not present
- 📦 Optional installation of krew plugins
- 🌍 Cross-platform support (Linux, macOS)
- 💻 Multi-architecture support (amd64, arm64)
- ⚡ Configurable versions for both krew and kubectl

## Usage

### Basic Usage

```yaml
- name: Setup Krew
  uses: fenio/setup-krew@v1
```

### Install with Plugins

```yaml
- name: Setup Krew
  uses: fenio/setup-krew@v1
  with:
    plugins: 'ctx ns'
```

### Specify Versions

```yaml
- name: Setup Krew
  uses: fenio/setup-krew@v1
  with:
    krew-version: 'v0.4.4'
    kubectl-version: 'v1.28.0'
    plugins: 'ctx ns view-allocations'
```

### Complete Workflow Example

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Krew
        uses: fenio/setup-krew@v1
        with:
          plugins: 'ctx ns'
      
      - name: Use kubectl plugins
        run: |
          kubectl ctx
          kubectl ns
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `krew-version` | Version of krew to install | No | `latest` |
| `kubectl-version` | Version of kubectl to install if not present | No | `stable` |
| `plugins` | Space-separated list of krew plugins to install | No | `''` |

## Outputs

| Output | Description |
|--------|-------------|
| `krew-version` | The installed version of krew |
| `kubectl-installed` | Whether kubectl was installed by this action (`true`/`false`) |
| `plugins-installed` | List of plugins that were installed |

## Examples

### Using Outputs

```yaml
- name: Setup Krew
  id: setup-krew
  uses: fenio/setup-krew@v1
  with:
    plugins: 'ctx ns'

- name: Check outputs
  run: |
    echo "Krew version: ${{ steps.setup-krew.outputs.krew-version }}"
    echo "Kubectl installed: ${{ steps.setup-krew.outputs.kubectl-installed }}"
    echo "Plugins installed: ${{ steps.setup-krew.outputs.plugins-installed }}"
```

### Install Multiple Plugins

```yaml
- name: Setup Krew with multiple plugins
  uses: fenio/setup-krew@v1
  with:
    plugins: 'ctx ns tree view-allocations stern'
```

### Matrix Testing with Different kubectl Versions

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        kubectl-version: ['v1.28.0', 'v1.29.0', 'v1.30.0']
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Krew
        uses: fenio/setup-krew@v1
        with:
          kubectl-version: ${{ matrix.kubectl-version }}
          plugins: 'ctx ns'
```

## Popular Krew Plugins

Here are some popular krew plugins you might want to install:

- `ctx` - Switch between kubectl contexts
- `ns` - Switch between Kubernetes namespaces
- `tree` - Show a tree of Kubernetes resources
- `view-allocations` - View resource allocations
- `stern` - Multi-pod and container log tailing
- `view-secret` - Decode and view secrets
- `ingress-nginx` - Interact with ingress-nginx
- `oidc-login` - Log in to OpenID Connect provider

For a complete list of available plugins, visit the [krew plugin index](https://krew.sigs.k8s.io/plugins/).

## How It Works

1. **Check kubectl**: The action first checks if kubectl is already installed
2. **Install kubectl** (if needed): If kubectl is not found, it downloads and installs the specified version
3. **Install krew**: Downloads and installs the krew plugin manager
4. **Update PATH**: Adds krew to the GitHub Actions PATH
5. **Install plugins** (optional): If plugins are specified, installs them using krew

## Supported Platforms

- Linux (amd64, arm64)
- macOS (amd64, arm64)

## License

MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

If you encounter any issues or have questions, please [open an issue](https://github.com/fenio/setup-krew/issues).
