# GitHub staging manifest

MANIFEST_SCOPE: `CURATED_REPOSITORY_TREE`
TARGET_REPOSITORY: `weiyang02520-ops/dg202611-project-materials`
TARGET_BRANCH: `main`
REPOSITORY_ROLE: `INTERNAL_PROJECT_MATERIALS_REPOSITORY`
FINAL_SUBMISSION_PACKAGE: `NO`
TRACKED_FILES_EXPECTED: `90`

## Included packages

| Package | Selection rule |
|---|---|
| `OFFICIAL/` | Existing official baseline retained |
| `FORMAL_SUBMISSION_REBUILD/FULL_ASSEMBLY_STAGE_C/` | Current Stage C files only; superseded versions excluded |
| `FORMAL_SUBMISSION_REBUILD/DOCUMENT_AUDIT_REFERENCE_BINDING/` | Current reference-binding audit package |
| `FORMAL_SUBMISSION_REBUILD/VISUAL_ASSEMBLY_PREP/` | Current visual assembly preparation package |
| `FORMAL_SUBMISSION_REBUILD/FORMAL_FIGURES_P1/` | Seven formal figures in SVG and PNG plus specifications and checks |
| `PROJECT_GOVERNANCE/` | Core charter, canonical map, manifest and selected gates |
| `EXTERNAL_AUDIT/` | Existing curated audit evidence retained |

## Exclusion policy

Privacy/PII material, credentials, private keys, third-party vendor bundles, uncertain-copyright reference images, archives, videos, large Office files, build/cache outputs, nested repositories and files over 100 MB are excluded. See `LOCAL_EXCLUDED_FILES.md` for the public-safe local exclusion summary.

## Publication checks

The exact file count, byte count and largest-file result are computed immediately before commit and recorded in the corresponding Git commit and release report. The manifest intentionally avoids self-referential file hashes.
