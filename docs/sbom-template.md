# SBOM Template

Generates a [CycloneDX](https://cyclonedx.org) Software Bill of Materials (SBOM) for a project using [Syft](https://github.com/anchore/syft). The SBOM is published as a pipeline artifact for later use (e.g. upload to DependencyTrack).

## Parameters

| Parameter | Type | Default | Required | Description |
|---|---|---|---|---|
| `jobName` | string | — | ✅ | Unique job identifier |
| `sourceName` | string | — | ✅ | Name of the project/component in the SBOM |
| `displayName` | string | `SBOM Generation` | | Display name in the pipeline UI |
| `dependsOn` | object | `[]` | | Jobs this job depends on |
| `workingDirectory` | string | `.` | | Path to scan, relative to the sources directory |
| `outputFormat` | string | `cyclonedx-json` | | Syft output format |
| `outputFilename` | string | `sbom.cyclonedx.json` | | Output filename for the SBOM |
| `sourceVersion` | string | `$(Build.BuildNumber)` | | Version to embed in the SBOM |
| `useBuildArtifacts` | boolean | `true` | | Download and extract a build artifact before scanning |
| `artifactName` | string | `function-app-build` | | Artifact to download when `useBuildArtifacts` is `true` |
| `sbomArtifactName` | string | `SBOM` | | Name for the published SBOM pipeline artifact |

## Usage

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
      artifactName: my-service-build
      dependsOn: build
```
