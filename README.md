# OpenShift Dev Spaces Installation Guide

This guide provides step-by-step instructions for installing OpenShift Dev Spaces in an OpenShift environment.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation Methods](#installation-methods)
- [Post-Installation](#post-installation)
- [Verification](#verification)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)

## Prerequisites

Before installing OpenShift Dev Spaces, ensure you have the following:

### System Requirements

- **OpenShift Container Platform**: Version 4.13-4.17 (4.14-4.17 recommended, 4.13-4.17 for IBM Power architecture)
- **Cluster Access**: Administrative access to your OpenShift cluster
- **CLI Tools**: 
  - `oc` CLI installed and configured
  - Active session with cluster admin permissions
  - `dsc` (Dev Spaces management tool) installed

### Resource Requirements

- Sufficient cluster resources for Dev Spaces components
- Network connectivity to required container registries
- Storage provisioner configured in your cluster

## Installation Methods

### Method 1: Using CLI (Recommended)

The CLI method using `dsc` is the recommended approach for installation.

#### Step 1: Install the DSC Management Tool

1. Download the `dsc` CLI tool from the [Red Hat Developer Portal](https://developers.redhat.com/products/openshift-dev-spaces/download)

2. Extract the archive:
   ```bash
   tar xvzf devspaces-[version]-dsc-[platform].tar.gz
   ```

3. Add the extracted `/dsc/bin` directory to your `$PATH`:
   ```bash
   export PATH=$PATH:/path/to/dsc/bin
   ```

4. Verify installation:
   ```bash
   dsc --version
   ```

#### Step 2: Login to OpenShift Cluster

```bash
oc login --server=<your-openshift-server-url> -u <admin-user>
```

#### Step 3: Deploy OpenShift Dev Spaces

```bash
# Optional: Remove previous instance if exists
dsc server:delete

# Deploy OpenShift Dev Spaces
dsc server:deploy --platform openshift

# Monitor the deployment progress
dsc server:status
```

#### Step 4: Access the Dashboard

```bash
# Open the Dev Spaces dashboard
dsc dashboard:open
```

### Method 2: Using OpenShift Web Console

1. Log in to the OpenShift web console as a cluster administrator

2. Navigate to **Operators** → **OperatorHub**

3. Search for "Red Hat OpenShift Dev Spaces"

4. Click on the **Red Hat OpenShift Dev Spaces** operator

5. Click **Install**

6. Configure the installation:
   - Choose the installation mode (All namespaces or specific namespace)
   - Select the update channel
   - Choose approval strategy (Automatic or Manual)

7. Click **Install** and wait for the operator to be installed

8. After installation, create a `CheCluster` custom resource to deploy Dev Spaces

## Post-Installation

### Verify Installation

Check the status of all Dev Spaces components:

```bash
dsc server:status
```

### Access Dev Spaces

1. Get the Dev Spaces URL:
   ```bash
   dsc dashboard:open
   ```

2. Or manually access via:
   ```bash
   oc get route devspaces -n openshift-devspaces
   ```

### Configure User Access

1. Ensure users have appropriate RBAC permissions
2. Users can access Dev Spaces through the web console or CLI
3. Configure identity providers if needed for authentication

## Verification

### Check Pod Status

```bash
oc get pods -n openshift-devspaces
```

All pods should be in `Running` state.

### Check Operator Status

```bash
oc get csv -n openshift-devspaces
```

### Verify Routes

```bash
oc get routes -n openshift-devspaces
```

## Troubleshooting

### Common Issues

1. **Installation Fails**
   - Verify cluster resources are sufficient
   - Check operator logs: `oc logs -n openshift-devspaces`
   - Ensure all prerequisites are met

2. **Pods Not Starting**
   - Check resource quotas: `oc describe quota -n openshift-devspaces`
   - Verify storage classes are available
   - Review pod events: `oc describe pod <pod-name> -n openshift-devspaces`

3. **Access Issues**
   - Verify routes are created: `oc get routes -n openshift-devspaces`
   - Check network policies if applicable
   - Ensure proper RBAC permissions

### Getting Help

- Check the [official documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/)
- Review operator logs for detailed error messages
- Consult the [GitHub repository](https://github.com/redhat-developer/devspaces-chectl)

## Important Notes

- **Single Instance**: Only one instance of OpenShift Dev Spaces can be deployed per cluster
- **Namespace**: Dev Spaces is typically installed in the `openshift-devspaces` namespace
- **Restricted Environments**: Specialized installation procedures apply for air-gapped or restricted environments
- **Components**: The installation includes:
  - Dev Spaces server components
  - Dev Workspace operator
  - User workspace infrastructure

## Additional Resources

- [Official Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/)
- [Administration Guide](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/latest/html/administration_guide/)
- [User Guide](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/latest/html/user_guide/)
- [Download Page](https://developers.redhat.com/products/openshift-dev-spaces/download)
- [GitHub Repository](https://github.com/redhat-developer/devspaces-chectl)

## License

This guide is provided for informational purposes. Refer to Red Hat's official documentation for the most up-to-date information.
