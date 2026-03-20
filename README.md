# CleanStart Container for Python

Official Python programming language runtime container, optimized and security-hardened for enterprise deployments. Includes the complete Python interpreter and standard library, pip package manager, and essential build tools. Features multi-stage builds to minimize image size, integrated security scanning, and enterprise-ready configurations. Supports both production deployments and development workflows with separate tagged versions.

**📌 CleanStart Foundation:** Security-hardened, minimal base OS designed for enterprise containerized environments.

---

## Key Features

- Complete Python runtime environment with standard library
- Integrated pip package manager and build tools
- Multi-stage builds for optimized image size
- Enterprise security features and compliance scanning
- Security-hardened base OS
- Minimal attack surface

---

## Common Use Cases

Typical scenarios where this container excels:

- Web application development and deployment
- Data science and machine learning workflows
- API development and microservices
- Automation and scripting in enterprise environments
- Cloud-native application development

---

## Quick Start

### Pull Commands

Download the runtime container images:
```bash
docker pull ghcr.io/cleanstart-containers/python:latest
docker pull ghcr.io/cleanstart-containers/python:latest-dev
```

### Python Hello World Application

A minimal Python Hello World application using the CleanStart image.

Run the following command:
```bash
docker run --rm ghcr.io/cleanstart-containers/python:latest-dev -c "print('Hello World')"
```

Expected output:
```bash
Hello World
```

### Interactive Development

Start interactive session for development:
```bash
docker run -it --name test-python \
  --entrypoint /bin/sh \
  -v $(pwd):/workspace \
  -w /workspace \
  ghcr.io/cleanstart-containers/python:latest-dev
```

### Run Hello World

Execute a simple Hello World program:
```bash
docker run --rm ghcr.io/cleanstart-containers/python:latest-dev -c 'print("Hello, World!")'
```

### Mount Workspace

Run container with local workspace mounted:
```bash
docker run --rm -v $(pwd):/app -w /app --user $(id -u):$(id -g) ghcr.io/cleanstart-containers/python:latest-dev -c 'import os; print(os.listdir("."))'
```

### Application Server

Run application with port forwarding:
```bash
docker run -d --name python-app \
  -p 8000:8000 \
  -v $(pwd):/app \
  -w /app \
  ghcr.io/cleanstart-containers/python:latest
```

---

## Configuration

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PATH` | `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` | System PATH configuration |
| `PYTHONPATH` | `/usr/local/lib/python3.9/site-packages` | Python packages search path |

---

## Security & Best Practices

### Recommended Security Context
```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  runAsGroup: 1000
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ['ALL']
```

### Best Practices

- Use specific image tags for production (avoid latest)
- Configure resource limits: memory and CPU constraints
- Enable read-only root filesystem when possible
- Run containers with non-root user (--user 1000:1000)
- Use --security-opt=no-new-privileges flag
- Regularly update container images for security patches
- Implement proper network segmentation
- Monitor container metrics for anomalies

---

## Architecture Support

### Multi-Platform Images
```bash
docker pull --platform linux/amd64 ghcr.io/cleanstart-containers/python:latest
docker pull --platform linux/arm64 ghcr.io/cleanstart-containers/python:latest
```

---

## Resources

- **Official Documentation:** https://www.python.org/doc/
- **Provenance / SBOM / Signature:** https://images.cleanstart.com/images/python
- **Docker Hub:** https://hub.docker.com/r/cleanstart/python
- **CleanStart All Images:** https://images.cleanstart.com
- **CleanStart Community Images:** https://hub.docker.com/u/cleanstart

---

## Vulnerability Disclaimer

CleanStart offers Docker images that include third-party open-source libraries and packages maintained by independent contributors. While CleanStart maintains these images and applies industry-standard security practices, it cannot guarantee the security or integrity of upstream components beyond its control.

Users acknowledge and agree that open-source software may contain undiscovered vulnerabilities or introduce new risks through updates. CleanStart shall not be liable for security issues originating from third-party libraries, including but not limited to zero-day exploits, supply chain attacks, or contributor-introduced risks.

**Security remains a shared responsibility:** CleanStart provides updated images and guidance where possible, while users are responsible for evaluating deployments and implementing appropriate controls.
