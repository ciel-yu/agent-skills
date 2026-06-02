# Migration Manifest Schema

The `migration-manifest.json` file records the complete file and section mapping so every surgery can be audited or reversed. Write it alongside the new skill directory (or the first result directory, for splits). Load this file only during the **execute** step if you need the exact field names.

---

## Schema

```json
{
  "operation": "merge | split",
  "timestamp": "ISO-8601",
  "sources": [
    {
      "skill_name": "original-skill-a",
      "path": "skills/original-skill-a",
      "companion_files": ["references/X.md", "references/Y.md"]
    }
  ],
  "results": [
    {
      "skill_name": "merged-skill",
      "path": "skills/merged-skill",
      "file_mapping": [
        {
          "destination": "references/X.md",
          "source_skill": "original-skill-a",
          "source_path": "references/X.md",
          "strategy": "keep"
        },
        {
          "destination": "references/Z.md",
          "source_skill": "original-skill-a",
          "source_path": "references/Z.md",
          "strategy": "merge",
          "additional_sources": [
            {"skill": "original-skill-b", "path": "references/Z.md"}
          ]
        }
      ],
      "sections_mapping": [
        {
          "destination_section": "## Pipeline",
          "source_skill": "original-skill-a",
          "source_section": "## Pipeline"
        }
      ],
      "description_source": "synthesized from original-skill-a + original-skill-b"
    }
  ]
}
```

---

## How to Roll Back

1. Delete all `results[*].path` directories.
2. Original `sources[*].path` directories are untouched (never deleted during execute).
3. For merged references (strategy: `merge` / `keep-stronger` / `demote`), the originals survive in the source directories.

No automatic rollback script — the manifest is a human-readable audit trail. The user can reverse the operation manually or request a reverse surgery.
