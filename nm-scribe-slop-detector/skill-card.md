## Description:

Detects AI-generated writing patterns in prose.

This skill is ready for commercial/non-commercial use.

## Publisher:

[athola](https://clawhub.ai/user/athola)

### License/Terms of Use:

MIT-0

## Use Case:

Developers and documentation maintainers use this skill to review prose, documentation, README files, and code comments for AI-writing markers, identity leaks, hallucinated references, stubs, and unsupported quality claims before publishing or merging changes.

### Deployment Geography for Use:

Global

## Known Risks and Mitigations:

Risk: Broad cleanup and remediation guidance can lead to file edits, deletions, commits, or pushes beyond simple prose detection.

Mitigation: Run detection in report-only mode unless remediation is explicitly requested, and require approval before edits, deletions, commits, or pushes.

Risk: CI steps, pre-commit hooks, and persistent project-rule files can change repository workflow and developer behavior.

Mitigation: Review and approve CI, hook, and project-rule changes separately before installing them in a branch or shared repository.

Risk: Secret, config, MCP, URL, and dependency checks may inspect sensitive project state or require network access.

Mitigation: Limit scan scope, approve network and security scans in advance, and keep sensitive findings within the project security process.

Risk: Aggressive prose cleanup can remove meaningful comments, historical text, generated files, or low-confidence material that needs human judgment.

Mitigation: Apply the documented anti-goals: preserve safety and contract-bearing comments, avoid generated or vendored files, and never auto-apply low-confidence findings.

## Reference(s):

- [ClawHub skill page](https://clawhub.ai/athola/skills/nm-scribe-slop-detector)
- [OpenClaw homepage metadata](https://github.com/athola/claude-night-market/tree/master/plugins/scribe)

## Skill Output:

**Output Type(s):** [Text, Markdown, JSON, Shell commands, Configuration, Guidance]

**Output Format:** [Markdown reports with structured finding blocks; JSON for CI mode.]

**Output Parameters:** [1D]

**Other Properties Related to Output:** [Findings may include severity, confidence, evidence excerpts, proposed fixes, diffs, density scores, and optional scan history.]

## Skill Version(s):

1.9.19 (source: server release evidence; artifact frontmatter says 1.9.8)

## Ethical Considerations:

Users should evaluate whether this skill is appropriate for their environment, review any generated or modified files before relying on them, and apply their organization's safety, security, and compliance requirements before deployment.
