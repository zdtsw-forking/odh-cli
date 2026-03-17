# Alternative Usage Methods

For container-based usage (recommended), see the [README](../README.md).

## Using Go Run (No Installation Required)

If you have Go installed, you can run the CLI directly from GitHub without cloning:

```bash
# Show help
go run github.com/opendatahub-io/odh-cli/cmd@latest --help

# Show version
go run github.com/opendatahub-io/odh-cli/cmd@latest version

# Run lint command
go run github.com/opendatahub-io/odh-cli/cmd@latest lint --target-version 3.3.0
```

> **Note:** Replace `@latest` with `@v1.2.3` to run a specific version, or `@main` for the latest development version.

**Token Authentication:**

```bash
go run github.com/opendatahub-io/odh-cli/cmd@latest \
  lint \
  --target-version 3.3.0 \
  --token=sha256~xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
  --server=https://api.my-cluster.p3.openshiftapps.com:6443
```

**Available commands:**
- `lint` - Validate cluster configuration and assess upgrade readiness
- `must-gather` - Collect diagnostic information from OpenShift AI clusters
- `version` - Display CLI version information

## Must-Gather (Collect Diagnostic Information)

Collect diagnostic information from OpenShift AI clusters for troubleshooting:

**Basic usage (with volume mount for output):**

```bash
# Using Go Run
go run github.com/opendatahub-io/odh-cli/cmd@latest must-gather

# Using container (mount local directory for output)
podman run --rm -ti \
  -v $KUBECONFIG:/kubeconfig \
  -v ./must-gather.local.$(date +%s):/tmp/must-gather \
  quay.io/rhoai/rhoai-upgrade-helpers-rhel9:dev must-gather
```

**For xKS environments (currently OCP, AKS, CKS) - collect LLM-D components only:**

```bash
go run github.com/opendatahub-io/odh-cli/cmd@latest must-gather --llm-d
```

**Collect with time filter:**

```bash
go run github.com/opendatahub-io/odh-cli/cmd@latest must-gather --since 1h
```

## As kubectl Plugin

Install the `kubectl-odh` binary to your PATH:

```bash
# Download from releases
# Place in PATH as kubectl-odh
# Use with kubectl
kubectl odh lint --target-version 3.3.0
kubectl odh version
```
