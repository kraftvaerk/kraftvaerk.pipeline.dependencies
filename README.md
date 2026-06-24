# Kraftvaerk Pipeline Dependencies

Reusable Azure DevOps pipeline templates for security scanning, SBOM generation, and vulnerability tracking via DependencyTrack.

---

## Templates

| Template | Description | Docs | Reference |
|---|---|---|---|
| `audit-template.yml` | Scans for vulnerabilities with Trivy | [docs/audit-template.md](docs/audit-template.md) | `.pipelines/templates/security/audit-template.yml@kv-pipeline-deps` |
| `sbom-template.yml` | Generates a CycloneDX SBOM with Syft | [docs/sbom-template.md](docs/sbom-template.md) | `.pipelines/templates/security/sbom-template.yml@kv-pipeline-deps` |
| `sbom-upload-template.yml` | Uploads an SBOM to DependencyTrack | [docs/sbom-upload-template.md](docs/sbom-upload-template.md) | `.pipelines/templates/security/sbom-upload-template.yml@kv-pipeline-deps` |

---

## Getting Started

Add this repository as a resource in your pipeline, then reference any template using the `@kv-pipeline-deps` alias.

```yaml
resources:
  repositories:
    - repository: kv-pipeline-deps
      type: github
      name: Kraftvaerk/kraftvaerk.pipeline.dependencies
      ref: refs/heads/main

jobs:
  - template: .pipelines/templates/security/sbom-template.yml@kv-pipeline-deps
    parameters:
      jobName: generateSbom
      sourceName: my-service
```

See each template's documentation for a full list of parameters and examples.

---

## What is an SBOM?

A Software Bill of Materials (SBOM) is a machine-readable inventory of every library and dependency included in a software artifact. It enables automated vulnerability scanning — each component can be cross-referenced against known vulnerability databases to identify security risks before they reach production.

- [CycloneDX SBOM standard](https://cyclonedx.org) — the format used by these templates
- [GitHub Advisory Database](https://github.com/advisories) — vulnerability reference used by Trivy
- [NIST National Vulnerability Database (NVD)](https://nvd.nist.gov) — the authoritative CVE database