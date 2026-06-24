# SBOM Upload Template

Downloads an SBOM pipeline artifact and uploads it to a [DependencyTrack](https://dependencytrack.org) instance via its REST API. Projects are auto-created if they do not already exist.

## Parameters

| Parameter | Type | Default | Required | Description |
|---|---|---|---|---|
| `dependencyTrackApiUrl` | string | — | ✅ | Base URL of the DependencyTrack server (no trailing slash) |
| `dependencyTrackApiKey` | string | — | ✅ | API key with BOM upload permission — use a secret variable |
| `projectName` | string | — | ✅ | Project name in DependencyTrack |
| `jobName` | string | `sbomUpload` | | Unique job identifier |
| `displayName` | string | `SBOM Upload` | | Display name in the pipeline UI |
| `dependsOn` | object | `[]` | | Jobs this job depends on |
| `projectVersion` | string | `$(Build.BuildNumber)` | | Version string in DependencyTrack |
| `parentName` | string | `""` | | Parent project name (for component nesting) |
| `sbomArtifactName` | string | `SBOM` | | Pipeline artifact containing the SBOM |
| `sbomFilename` | string | `sbom.cyclonedx.json` | | Filename of the SBOM inside the artifact |
| `latest` | boolean | `true` | | Mark this version as the latest in DependencyTrack |

## Usage

Secret variables must be explicitly declared in the pipeline `variables:` section (via a variable group or pipeline settings) before they can be passed to the template.

```yaml
resources:
  repositories:
    - repository: kv-pipeline-deps
      type: github
      name: Kraftvaerk/kraftvaerk.pipeline.dependencies
      ref: refs/heads/main

variables:
  - group: dependency-track-secrets  # must contain DEPENDENCY_TRACK_URL and DEPENDENCY_TRACK_API_KEY

jobs:
  - template: .pipelines/templates/security/sbom-upload-template.yml@kv-pipeline-deps
    parameters:
      jobName: uploadSbom
      dependsOn: generateSbom
      dependencyTrackApiUrl: $(DEPENDENCY_TRACK_URL)
      dependencyTrackApiKey: $(DEPENDENCY_TRACK_API_KEY)
      projectName: my-service
```

> **Note:** `dependencyTrackApiKey` must be a secret variable — store it in a variable group or pipeline secret, never hard-coded. The template passes it to the task via `env:` so the value is never expanded inline into the script.
