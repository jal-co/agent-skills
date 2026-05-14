# agents.md — Agent Skills

<context>
This repo holds reusable agent skills as Git submodules. Each submodule is a standalone skill repo following the [agentskills.io](https://agentskills.io/specification) specification. The repo itself is a registry — it groups skills, tracks versions via submodule refs, and documents the catalog in `README.md`.
</context>

---

## 1. Skill Format

<context>

Every skill is a directory containing at minimum a `SKILL.md` file with YAML frontmatter:

```
skill-name/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...
```

### Frontmatter

| Field           | Required | Description                                                        |
|-----------------|----------|--------------------------------------------------------------------|
| `name`          | Yes      | Lowercase, hyphens only, 1–64 chars, must match directory name     |
| `description`   | Yes      | What it does + when to use it, max 1024 chars                      |
| `license`       | No       | License name or reference to bundled file                          |
| `compatibility` | No       | Environment requirements (runtime, packages, network), max 500 chars |
| `metadata`      | No       | Arbitrary key-value pairs (author, version, etc.)                  |
| `allowed-tools` | No       | Space-separated pre-approved tools (experimental)                  |

### Body

Markdown instructions — no format restrictions. Recommended sections:

- Step-by-step instructions
- Input/output examples
- Edge cases and gotchas

Keep `SKILL.md` under 500 lines. Move detailed content to `references/`.

Full spec: https://agentskills.io/specification

</context>

---

## 2. Submodule Layout

<rules>

- Each skill MUST live in its own Git repo
- Each skill repo MUST be added to this repo as a Git submodule
- The submodule directory name MUST match the skill's `name` frontmatter field
- Submodules MUST be added at the repo root (flat structure, no nesting)

</rules>

<context>

```
agent-skills/
├── AGENTS.md
├── README.md              # Skill catalog table (maintained manually)
├── .gitmodules            # Submodule registry
├── my-skill/              # Submodule → github.com/user/my-skill
│   └── SKILL.md
├── another-skill/         # Submodule → github.com/user/another-skill
│   └── SKILL.md
└── ...
```

</context>

---

## 3. Adding a Skill

<workflow>

### Step 1: Create the skill repo

Create a new Git repo for the skill. At minimum it needs a `SKILL.md` with valid frontmatter.

```bash
mkdir my-skill && cd my-skill
git init
# Create SKILL.md with frontmatter + instructions
git add . && git commit -m "feat: initial skill"
# Push to GitHub
```

### Step 2: Add as submodule

From the `agent-skills` repo root:

```bash
git submodule add https://github.com/<owner>/<skill-repo>.git <skill-name>
git add .gitmodules <skill-name>
git commit -m "feat: add <skill-name> skill"
```

The `<skill-name>` directory MUST match the `name` field in the skill's `SKILL.md` frontmatter.

### Step 3: Update the README catalog

Add a row to the skill catalog table in `README.md` (see §5).

### Step 4: Push

```bash
git push
```

</workflow>

---

## 4. Updating & Removing Skills

<workflow>

### Update a skill to latest

```bash
cd <skill-name>
git pull origin main
cd ..
git add <skill-name>
git commit -m "chore: update <skill-name> to latest"
```

### Update all skills

```bash
git submodule update --remote --merge
git add .
git commit -m "chore: update all skills to latest"
```

### Remove a skill

```bash
git submodule deinit -f <skill-name>
git rm -f <skill-name>
rm -rf .git/modules/<skill-name>
git commit -m "chore: remove <skill-name> skill"
```

After removing, MUST also delete the skill's row from the `README.md` catalog table.

</workflow>

---

## 5. README Catalog

<rules>

- `README.md` MUST contain a table cataloging every skill in this repo
- The table MUST be kept in sync whenever a skill is added, updated, or removed
- Each row MUST include the skill name, a short description, and a link to the source repo

</rules>

<guidelines>

Recommended table format:

```markdown
| Skill | Description | Source |
|---|---|---|
| `my-skill` | What it does (from `description` frontmatter) | [repo](https://github.com/owner/repo) |
```

Additional columns MAY be added as needed (e.g., compatibility, version, license).

</guidelines>

---

## 6. Cloning This Repo

<context>

Because skills are submodules, cloning requires `--recurse-submodules`:

```bash
git clone --recurse-submodules https://github.com/jal-co/agent-skills.git
```

If already cloned without submodules:

```bash
git submodule update --init --recursive
```

</context>

---

## 7. Installing Skills Locally

<context>

To use a skill from this repo with a local agent (e.g., pi, Claude Code, Cursor):

1. Clone with submodules (see §6)
2. Symlink the skill directory into the agent's skill path

**Example for pi:**

```bash
ln -s /path/to/agent-skills/my-skill ~/.pi/agent/skills/my-skill
```

**Example for OpenClaw (on server):**

```bash
ln -s /path/to/agent-skills/my-skill /root/.openclaw/workspace/skills/my-skill
```

Alternatively, add each skill repo as a submodule directly in the agent's skill directory.

</context>
