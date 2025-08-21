# HCV Servers

A server inventory and configuration management repository for the HCV (Host Configuration Versioning) system. This repository maintains the mapping between servers and their corresponding configuration repositories, enabling automated deployment and version management across your infrastructure.

## Overview

The `hcv-servers` repository serves as the central inventory for managing server configurations in the HCV ecosystem. It tracks which configuration repositories are deployed to which servers and maintains version control for these deployments through automated dependency management.

## Project Structure

```
hcv-servers/
├── README.md           # This documentation
├── renovate.json       # Renovate configuration for automated updates
├── server01.yaml       # Server configuration definition
└── serverXX.yaml       # Additional server configurations...
```

## Key Components

### **Server Configuration Files (`serverXX.yaml`)**
Each server has its own YAML configuration file that defines:
- **Configuration Repository**: The source repository containing deployment configs
- **Version**: Specific version/tag of the configuration to deploy
- **Repository URL**: Location of the configuration repository

Example server configuration:
```yaml
---
- name: Config01
  # renovate: depName=https://github.com/truestory1/hcv-config-01
  version: v0.1.1
  repo: https://github.com/truestory1/hcv-config-01
```

### **[renovate.json](renovate.json)**
Automated dependency management configuration that:
- Monitors configuration repository versions
- Creates pull requests for version updates
- Uses custom regex patterns to detect version annotations in YAML files
- Supports Git tags as the default versioning strategy

## HCV Ecosystem Integration

This repository is part of the broader HCV (Host Configuration Versioning) system:

- **🏗️ hcv-config-XX**: Configuration repositories defining file/directory permissions and deployment settings
- **📦 hcv-deploy-files**: Ansible roles for deploying configurations to target servers
- **🖥️ hcv-servers**: *This repository* - Server inventory and configuration version management

## Usage

### Adding a New Server Configuration

1. **Create a new server YAML file:**
   ```bash
   # Example: server02.yaml
   ---
   - name: WebServerConfig
     # renovate: depName=https://github.com/your-org/hcv-web-config
     version: v1.0.0
     repo: https://github.com/your-org/hcv-web-config
   ```

2. **Include Renovate annotation:**
   The `# renovate: depName=` comment enables automated version updates

3. **Commit and push:**
   ```bash
   git add server02.yaml
   git commit -m "Add server02 configuration"
   git push
   ```

### Updating Server Configurations

#### Manual Updates
Edit the `version` field in the appropriate server YAML file:
```yaml
- name: Config01
  # renovate: depName=https://github.com/truestory1/hcv-config-01
  version: v0.2.0  # Updated version
  repo: https://github.com/truestory1/hcv-config-01
```

#### Automated Updates (Recommended)
Renovate automatically:
1. Monitors the referenced configuration repositories
2. Detects new releases/tags
3. Creates pull requests with version updates
4. Updates the `version` field when PRs are merged

### Deployment Workflow

1. **Configuration Update**: Modify configuration in `hcv-config-XX` repository
2. **Release**: Tag new version in configuration repository
3. **Auto-Update**: Renovate detects new version and creates PR in `hcv-servers`
4. **Review & Merge**: Review and merge the version update PR
5. **Deploy**: Use `hcv-deploy-files` to deploy updated configuration to servers

## Configuration Repository Requirements

Configuration repositories referenced in server files should:

- **Use semantic versioning**: `v1.0.0`, `v1.1.0`, etc.
- **Tag releases**: Create Git tags for versions
- **Include `.config.yml`**: Main configuration file defining file/directory settings
- **Follow HCV standards**: Compatible with `hcv-deploy-files` Ansible roles

Example configuration repository structure:
```
hcv-config-example/
├── README.md
├── .config.yml        # Main configuration
├── renovate.json
└── tmp/               # Test files and examples
```

## Renovate Configuration

The `renovate.json` file includes:

- **Custom regex manager**: Parses YAML files for version annotations
- **Git-tags datasource**: Monitors Git repository tags for updates
- **File matching**: Processes all YAML files (`*.yaml`, `*.yml`)
- **Fork processing**: Enabled for security and isolation

### Annotation Format
```yaml
# renovate: depName=<repository-url>
version: <current-version>
```

## Development

### Adding a New Server
1. Fork this repository
2. Create a new `serverXX.yaml` file following the format above
3. Ensure proper Renovate annotation is included
4. Test the YAML syntax is valid
5. Submit a pull request

### Modifying Existing Configurations
1. Update the appropriate server YAML file
2. Verify Renovate annotations are correct
3. Test configuration compatibility
4. Submit pull request with clear description

## Monitoring and Maintenance

- **Renovate PRs**: Review automated dependency update pull requests regularly
- **Version Compatibility**: Ensure configuration repository versions are compatible with deployment tools
- **Documentation**: Keep server configurations documented and up-to-date

## Troubleshooting

### Common Issues

**Renovate not detecting updates:**
- Verify `depName` annotation matches exact repository URL
- Check that the referenced repository has proper Git tags
- Ensure repository is publicly accessible

**Invalid YAML syntax:**
- Validate YAML files using `yamllint` or online validators
- Check indentation and special characters
- Verify all strings are properly quoted when needed

**Version conflicts:**
- Review configuration repository compatibility
- Check deployment logs in `hcv-deploy-files`
- Verify server access and permissions

## Contributing

1. Fork the repository
2. Create a feature branch for your changes
3. Add or modify server configuration files
4. Test YAML syntax and Renovate annotations
5. Submit a pull request with clear description

## Related Repositories

- [hcv-config-01](https://github.com/truestory1/hcv-config-01): Example configuration repository
- [hcv-deploy-files](https://github.com/search?q=org%3Atruestory1+hcv-deploy): Ansible deployment roles (if available)

## License

This project is licensed under copyleft terms.