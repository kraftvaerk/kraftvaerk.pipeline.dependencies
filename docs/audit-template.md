# Audit Template

Scans a directory or build artifact for known vulnerabilities using [Trivy](https://trivy.dev). Results are published as JUnit test results in the pipeline run.

## Parameters

| Parameter | Type | Default | Required | Description |
|---|---|---|---|---|
| `jobName` | string | — | ✅ | Unique job identifier |
| `displayName` | string | `Audit` | | Display name in the pipeline UI |
| `dependsOn` | object | `[]` | | Jobs this job depends on |
| `workingDirectory` | string | `.` | | Path to scan, relative to the sources directory |
| `severity` | string | `CRITICAL,HIGH` | | Comma-separated severity levels to report |
| `failOnVulnerabilities` | boolean | `true` | | Fail the pipeline if vulnerabilities are found |
| `exitCode` | number | `0` | | Trivy exit code on findings (set to `1` to fail the scan step itself) |
| `useBuildArtifacts` | boolean | `false` | | Download a pipeline artifact before scanning |
| `artifactName` | string | `drop` | | Artifact to download when `useBuildArtifacts` is `true` |
| `scanType` | string | `fs` | | `fs` (filesystem) or `config` (IaC/config files) |
| `trivyVersion` | string | `0.71.2` | | Trivy version to install |

## Usage

Declare the repository resource once per pipeline, then reference the template in any stage.

```yaml
resources:
  repositories:
    - repository: kv-pipeline-deps
      type: github
      name: Kraftvaerk/kraftvaerk.pipeline.dependencies
      ref: refs/heads/main

jobs:
  - template: .pipelines/templates/security/audit-template.yml@kv-pipeline-deps
    parameters:
      jobName: auditApp
      workingDirectory: src
      severity: CRITICAL,HIGH,MEDIUM
```
