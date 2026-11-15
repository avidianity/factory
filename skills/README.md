# Skills

A skill is a folder with a `SKILL.md` that tells an agent how to do one kind of task.
The Kit has two kinds: its own skills, which Bootstrap copies into the Project, and pinned third-party skills, which the Project installs and never copies.

## The Kit's own skills

| Skill | What it does | How it starts |
| ----- | ------------ | ------------- |
| [bootstrap](./bootstrap/SKILL.md) | Runs [BOOTSTRAP.md](../BOOTSTRAP.md) in a Project | The Owner asks for it |
| [upgrade](./upgrade/SKILL.md) | Runs [UPGRADE.md](../UPGRADE.md) in a Project | The Owner asks for it |
| [correction](./correction/SKILL.md) | Turns a repeated mistake into a check | The Owner or an agent sees a mistake repeat |
| [one-go](./one-go/SKILL.md) | Lands one task: implement, review, commit, pull request, merge | The Owner asks for it |
| [one-go-bg](./one-go-bg/SKILL.md) | The same landing, run by a background agent | The Owner asks for it |
| [one-go-parallel](./one-go-parallel/SKILL.md) | Lands several tasks with parallel agents | The Owner asks for it |

Bootstrap copies `correction`, `one-go`, `one-go-bg`, and `one-go-parallel` into the Project's skills folder.
`bootstrap` and `upgrade` run from the Kit and are not copied.

Skills marked "The Owner asks for it" set `disable-model-invocation: true`, so an agent never starts them on its own.
Starting one of them is the Owner's permission for the work it describes, and for nothing more.

## Pinned third-party skills

These are Matt Pocock's skills from `mattpocock/skills`.
The Project installs them with the skills installer, which records each one in `skills-lock.json` with a content hash.

| Skill | Path in the source | Content hash, checked 2026-11-16 |
| ----- | ------------------ | --------------------------- |
| `grill-me` | `skills/productivity/grill-me` | `ab1acb9fbd9eaee03da254abb8a8de228d1e7a07475fe959da4a5eefa34ea4b9` |
| `grill-with-docs` | `skills/engineering/grill-with-docs` | `02e258f41d210f9b2e7cce9faf62b3c59e8fcebe5d2736369c3cd07d471324f8` |
| `grilling` | `skills/productivity/grilling` | `485451958784497cd4c4dbea13cb5bbb579da58fac0d0e45f3bcb310fab68ac7` |
| `domain-modeling` | `skills/engineering/domain-modeling` | `b78689833404148c769077355e12dbe5ad996a158a1e21e58ddf169e439d9d8c` |
| `wait-what` | `skills/productivity/wait-what` | `70c9bf9f5a84b6849760f9492f569d5aa12307a0310323fde5f583144730c26c` |

`grill-with-docs` loads `grilling` and `domain-modeling`, so install all three together.

Install all five together, and restore them on a new machine or in CI, with the skills installer commands in [adapters/tools.md](../adapters/tools.md#skills-installer).

The lock file entry has this shape:

```json
{
  "version": 1,
  "skills": {
    "grill-me": {
      "source": "mattpocock/skills",
      "sourceType": "github",
      "skillPath": "skills/productivity/grill-me/SKILL.md",
      "computedHash": "ab1acb9fbd9eaee03da254abb8a8de228d1e7a07475fe959da4a5eefa34ea4b9"
    }
  }
}
```

When the installed hash differs from the table above, the upstream skill changed after this Kit release.
Read the change before you accept it.

## Writing a Project skill

- Put it in the Project's skills folder as `<name>/SKILL.md`.
- Start with frontmatter: `name`, `description`, and `disable-model-invocation: true` when only the Owner may start it.
- Name the documents it reads; do not copy their rules into it.
- When it calls an agent's own tool, name the tool for each agent the Project uses, from [adapters/agents.md](../adapters/agents.md).
- Do not add a skill that works in one agent only.
