# json-api-builder-validation

[![npm version](https://img.shields.io/npm/v/json-api-builder-validation?color=cb3837&style=flat-square)](https://www.npmjs.com/package/json-api-builder-validation)
[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)
[![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen.svg?style=flat-square)](package.json)
[![GitHub Repository](https://img.shields.io/badge/GitHub-keshavsoft%2Fjson-api-builder-validation-181717.svg?style=flat-square&logo=github)](https://github.com/keshavsoft/json-api-builder-validation)
[![Documentation](https://img.shields.io/badge/docs-live_portal-amber.svg?style=flat-square)](https://keshavsoft.github.io/json-api-builder-validation/)

> **Stage 7 of 8**: Stage 7: Pre-Flight Tree Linting & Post-Build Artifact Verification

Part of the modular **[JSON API Builder](https://github.com/keshavsoft/json-api-builder-orchestrator)** ecosystem by [KeshavSoft](https://github.com/keshavsoft).

---

## The Story

Guards the pipeline at both ends: pre-flight schema linting for tree.json and templates, followed by post-generation verification to ensure complete artifact integrity.

### The Problem It Solves
Silent build failures or typos in JSON specification trees result in missing routes, broken imports, or runtime crashes that are difficult to debug.

### The Solution
Runs deep pre-flight validation on the input tree and templates before execution, and post-build verification to assert that every expected file, router, and export exists and is valid.

---

## Architecture & Pipeline Position

```
[1. Folders] ──▶ [2. Files] ──▶ [3. Content] ──▶ [4. Hook Routes]
                                                         │
[8. Orchestrator] ◀── [7. Validate] ◀── [6. Endpoints] ◀── [5. App]
```

**This repository implements Stage 7 in isolation.** It can be used independently in custom build workflows or composed automatically via `json-api-builder`.

---

## Installation

```bash
npm install json-api-builder-validation
```

---

## Quick Start

```js
import { validateConfiguration, verifyGeneratedApp } from "json-api-builder-validation";

// Pre-flight check
validateConfiguration({ clientRoot: "./my-project" });

// Post-generation check
const report = verifyGeneratedApp({ outputRoot: "./my-project/output" });
console.log("Validation passed:", report.valid);
```

---

## Input Specification

Specification tree and generated output directory.

```json
{
  "api": { "routes.js": "routes.estree.json" },
  "endpoints": { "fs": { "routes.js": "routes.estree.json" } }
}
```

---

## Output Transformation

Structured verification report validating overall build integrity.

```text
{
  "valid": true,
  "verifiedFiles": 7,
  "missingFiles": []
}
```

---

## API Reference

### `validateConfiguration`, `verifyGeneratedApp`

Accepts an options object with the following configuration:

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `clientRoot` | `string` | `process.cwd()` | Root path of the client project. |
| `outputRoot` | `string` | `path.join(clientRoot, 'output')` | Directory where artifacts were generated. |

---

## Sibling Pipeline Packages

| Stage | Package | Description |
| :---: | :--- | :--- |
| **01** | [json-api-builder-create-folders](https://github.com/keshavsoft/json-api-builder-create-folders) | Directory hierarchy scaffolding |
| **02** | [json-api-builder-create-files](https://github.com/keshavsoft/json-api-builder-create-files) | Non-destructive placeholder file creation |
| **03** | [json-api-builder-insert-content](https://github.com/keshavsoft/json-api-builder-insert-content) | ESTree template to JavaScript insertion |
| **04** | [json-api-builder-alter-content](https://github.com/keshavsoft/json-api-builder-alter-content) | Hierarchical route hooking & AST imports |
| **05** | [json-api-builder-create-app](https://github.com/keshavsoft/json-api-builder-create-app) | Root app.js assembly & server runtime |
| **06** | [json-api-builder-create-endpoints](https://github.com/keshavsoft/json-api-builder-create-endpoints) | CRUD endpoints & local JSON file sync |
| **07** | [json-api-builder-validation](https://github.com/keshavsoft/json-api-builder-validation) | Specification linting & artifact checks |
| **08** | [json-api-builder](https://github.com/keshavsoft/json-api-builder-orchestrator) | Master orchestrator coordinating all stages |

---

## Development & Testing

```bash
# Install dependencies
npm install

# Run automated tests
npm test

# Run syntax check
npm run check

# Prepublish verification
npm run prepublishOnly
```

---

## License

[MIT](LICENSE) © 2026 [KeshavSoft](https://github.com/keshavsoft)
