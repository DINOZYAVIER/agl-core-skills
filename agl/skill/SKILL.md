---
name: skill
description: Create or update agentLIBRE runtime skills. Use when editing the core skills repository or trusted skill sources, designing required_hooks and allowed_tools, checking SKILL.md manifests, or converting generic YAML-frontmatter skills into the stricter agentLIBRE runtime format.
version: 1
source: core
pack: agl
required_hooks:
  - repo_path.validate
  - skill_manifest.validate
  - secret_scan.validate
allowed_tools:
  - fs.read
  - fs.list
  - fs.search
  - fs.edit
context_budget_tokens: 2048
references:
  include: []
guarantees:
  - runtime skill manifests must use the agentLIBRE runtime fields
  - runtime skill references must stay under references/
  - embedded core skills must not include executable scripts
  - skill authoring artifacts must not expose obvious secret material
---

Use this skill for agentLIBRE runtime skills.

Keep `SKILL.md` concise. Put only the workflow and essential constraints in
the body. Add references only when the skill needs reusable domain detail.
Do not add `scripts/` under embedded core skills; builtin asset embedding
rejects executable skill scripts.

Use the agentLIBRE manifest fields:

- `name`
- `description`
- `version`
- `source`
- `pack`
- `required_hooks`
- `allowed_tools`
- `requestable_tools`
- `denied_tools`
- `permission_request_templates`
- `context_budget_tokens`
- `references.include`
- `guarantees`
- `artifacts` or `folders`

Choose hooks that can actually run in the current host. Choose tools that the
skill may need, but keep write tools out of read-only skills.

Use `artifacts`/`folders` when a skill needs workspace paths under `.agl`.
Declare `path`, `kind`, `access`, and any `create.when` situations explicitly.
`create.when: artifact_write` prepares or validates the declared folder before
an AgentLIBRE-owned `.agl` write; it is not a separate authorization grant.
