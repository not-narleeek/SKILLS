# SKILLS

Personal collection of AI agent skills (Claude-style `SKILL.md` packages), synced from `~/.agents/skills`.

Each top-level directory is one skill. The entrypoint is always `SKILL.md` with YAML frontmatter (`name`, `description`, optionally `allowed-tools`); supporting files live alongside it.

## AI setup instructions

If you are an AI agent setting up a new machine from this repo:

1. Clone this repo to `~/.agents/skills` (if that dir already exists with skills, clone elsewhere and merge/symlink).
   ```bash
   git clone git@github.com:not-narleeek/SKILLS.git ~/.agents/skills
   ```
2. If your agent harness reads skills from a different path, symlink it:
   ```bash
   ln -s ~/.agents/skills <harness-skills-dir>
   ```
   Known harness paths: `~/.config/omp/skills`, `~/.claude/skills`.
3. Validate every skill dir has a `SKILL.md` with valid frontmatter (`name` must match or sensibly alias the dir name). Skip/report any that don't.
4. `ctf-*` skills assume the tooling from `scripts/install_ctf_tools.sh` — review it, then run it if you want CTF tooling installed.
5. `scripts/generate_catalog.py` regenerates the skill catalog; `scripts/skill_security_auditor.py` audits skill contents. Run the auditor after adding skills from untrusted sources.
6. Restart the agent session so the harness picks up the skills.

## Adding a new skill

1. `mkdir ~/.agents/skills/<skill-name>` with a `SKILL.md` (frontmatter: `name`, `description`; include `allowed-tools` if the harness supports it).
2. From inside `~/.agents/skills`:
   ```bash
   git add <skill-name> && git commit -m "add skill: <skill-name>" && git push
   ```
3. On other machines: `git pull` in `~/.agents/skills`.
