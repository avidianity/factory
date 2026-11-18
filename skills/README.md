# Skills

A skill is a folder with a `SKILL.md` that tells an agent how to do one kind of task.
The Kit has two kinds: its own skills, which Bootstrap copies into the Project, and pinned third-party skills, which the Project installs and never copies.

## The Kit's own skills

| Skill | What it does | How it starts |
| ----- | ------------ | ------------- |
| [bootstrap](./bootstrap/SKILL.md) | Runs [BOOTSTRAP.md](../BOOTSTRAP.md) in a Project | The Owner asks for it |
| [upgrade](./upgrade/SKILL.md) | Runs [UPGRADE.md](../UPGRADE.md) in a Project | The Owner asks for it |
| [correction](./correction/SKILL.md) | Turns a repeated mistake into a check | The Owner or an agent sees a mistake repeat |

Bootstrap copies `correction` into the Project's skills folder.
`bootstrap` and `upgrade` run from the Kit and are not copied.

Skills marked "The Owner asks for it" set `disable-model-invocation: true`, so an agent never starts them on its own.
Starting one of them is the Owner's permission for the work it describes, and for nothing more.

## Pinned third-party skills

The Project installs these skills with the skills installer, which records each one in `skills-lock.json` with a content hash.
[core/agents/planning.md](../core/agents/planning.md) decides when each one runs.
Two sources fit together:

- Matt Pocock's skills plan what to build: they settle Decisions, write the Spec, and split it into Tickets with Blocking edges.
- pstack, Poteto's skills from Cursor, shape and prove the change: they sketch the design in code and accept only findings that a run proves.

### From `mattpocock/skills`

| Skill | Path in the source | Content hash, checked 2026-11-19 |
| ----- | ------------------ | -------------------------------- |
| `grill-me` | `skills/productivity/grill-me` | `ab1acb9fbd9eaee03da254abb8a8de228d1e7a07475fe959da4a5eefa34ea4b9` |
| `grilling` | `skills/productivity/grilling` | `485451958784497cd4c4dbea13cb5bbb579da58fac0d0e45f3bcb310fab68ac7` |
| `wait-what` | `skills/productivity/wait-what` | `70c9bf9f5a84b6849760f9492f569d5aa12307a0310323fde5f583144730c26c` |
| `grill-with-docs` | `skills/engineering/grill-with-docs` | `02e258f41d210f9b2e7cce9faf62b3c59e8fcebe5d2736369c3cd07d471324f8` |
| `domain-modeling` | `skills/engineering/domain-modeling` | `b78689833404148c769077355e12dbe5ad996a158a1e21e58ddf169e439d9d8c` |
| `wayfinder` | `skills/engineering/wayfinder` | `bd82983c105881f6cb0c15e5d029c0d5fca756939b69eebe94e49003069a276f` |
| `research` | `skills/engineering/research` | `7662666af51f8b1672ed7d88360d4b82656da21bb7b11c49e956eeb3036b85d5` |
| `prototype` | `skills/engineering/prototype` | `1e3ba2202a5b77b8aa50bc4447432331d6e42590e53ec0935689662cfed199ca` |
| `triage` | `skills/engineering/triage` | `4342a07c9ebe0696713ec93a883c46e56503a587a34d824fabab5ca61e3bdfbb` |
| `to-spec` | `skills/engineering/to-spec` | `eb934b5eda9523c7853a2741544bd4721ee1bea471f5a04f8397827f67bdbdda` |
| `to-tickets` | `skills/engineering/to-tickets` | `17496514c92954f68eacdf2b1221e2ddb39fa231b83f03c70bc7d4422f704b2d` |
| `implement` | `skills/engineering/implement` | `560465496ad0669329993bfdd0604e5c93b5c2446309a76f486040010b063908` |
| `implement-spec` | `skills/engineering/implement-spec` | `c3474ef320861d793e8b30d41fbdd2cb0ceaa1081bb5c81b7f26c455eb776573` |
| `tdd` | `skills/engineering/tdd` | `e3e0f78ff1c2d52ffefa0378baa1e80e129d359288cf9a56f4e59afcd2aac86c` |
| `codebase-design` | `skills/engineering/codebase-design` | `d40f025dc1b0f2020663260f38f2bcb8d6ed3523afb11e5fda624b01238677e8` |
| `code-review` | `skills/engineering/code-review` | `ce86b40a3d1648a0018b712f0c70704e6c2316b2a9b9e915ab116916a31273dc` |

### From `backnotprop/pstack`

`backnotprop/pstack` is a mirror of Cursor's pstack that works in every agent the Kit supports.
Install from the mirror, not from Cursor's own plugin, because the plugin works in one agent only.

| Skill | Path in the source | Content hash, checked 2026-11-19 |
| ----- | ------------------ | -------------------------------- |
| `architect` | `skills/architect` | `b2c440fa359b72e30115258c62df7c96713dcac1fee0abb9e18033901d8a9982` |
| `arena` | `skills/arena` | `b4a7802d3f9ad9cc5a9aeec23e53541be5224ff38d93ed7dbbf5a176f0ecff5e` |
| `sharpen` | `skills/sharpen` | `27a8957e6dbd9c1f50228592b1aa1ef5598aef14d4c2047213a740ecdbfc217f` |
| `blast-radius` | `skills/blast-radius` | `f9ec7353939c4449f2edb1cd195049f477b9ba6c0a869de75c7b440e27295f01` |
| `interrogate` | `skills/interrogate` | `20ae3427594fc8e8848558f4c093d619b547e8c166833ac9cb2cba58dfaa14e4` |
| `in-your-own-words` | `skills/in-your-own-words` | `eaacabc9a2c7a1a674334a4e01e5900a6f7a764d405237ce17d4e4200cf528be` |
| `how` | `skills/how` | `397faf7706279220499dd649a110e0055f1665ecab63528f321d231632af17de` |
| `why` | `skills/why` | `2050d10fff1d0fa6e19c0a96552a641af6a24f012614955a6dcf3763bf54ac3c` |
| `unslop` | `skills/unslop` | `4bbcb6719a8f0f6f7fb46752e1c360791d7758a837708e300a06fc7d6907938c` |
| `show-me-your-work` | `skills/show-me-your-work` | `ec213956e5cb2f5f6f5d814c943d83e4fd8d865417d44acfba9355a2e2e1a192` |
| `principle-separate-before-serializing-shared-state` | `skills/principle-separate-before-serializing-shared-state` | `7be23154bac7c6858b373fb5525e847028c06487a91f502ad8a9ac96b9d5b408` |
| `principle-redesign-from-first-principles` | `skills/principle-redesign-from-first-principles` | `c6b2348402c9c1a4d3b43ab695ad8dbb6a92b19e02e08f6395146ab9b1b61ce9` |
| `principle-prove-it-works` | `skills/principle-prove-it-works` | `e1903c637f60b63a8ded3ca9ec60d132190f0473085581be7fcf1fb8cb18bb49` |
| `principle-laziness-protocol` | `skills/principle-laziness-protocol` | `23fb5314b8b2bfb6c2b783892e791aca6c59c69313b714eecc11704e4a2d38cb` |
| `principle-encode-lessons-in-structure` | `skills/principle-encode-lessons-in-structure` | `164c25e039539bc5cc04f109f3596d723e40c68f9745cd35129b34d4fe3e706e` |

`arena` and `interrogate` pick a model for each agent they start from the Owner's pstack settings file.
The Owner writes that file once with pstack's `setup-pstack` skill, in their own agent setup.
The Project never pins `setup-pstack` and never commits the settings file.
Without it, the skills use their built-in defaults.
`arena` and `show-me-your-work` also point to the Subagents section of pstack's `poteto-mode` skill for the reasoning effort of each agent.
Without `poteto-mode`, each agent runs at the default effort.

### Install all of them together

Each skill below loads the skills next to it, so a partial install breaks it.

| Skill | Loads |
| ----- | ----- |
| `grill-me` | `grilling` |
| `grill-with-docs`, `triage` | `grilling`, `domain-modeling` |
| `wayfinder` | `grilling`, `domain-modeling`, `research`, `prototype` |
| `implement`, `implement-spec` | `tdd`, `code-review` |
| `tdd` | `codebase-design` |
| `architect` | `arena` |
| `arena` | `principle-separate-before-serializing-shared-state`, `principle-redesign-from-first-principles`, `principle-prove-it-works`, `principle-laziness-protocol` |
| `interrogate` | `arena`, `in-your-own-words` |
| `blast-radius` | `arena`, `how`, `why`, `unslop` |
| `show-me-your-work` | `unslop`, `principle-encode-lessons-in-structure` |

Install them, and restore them on a new machine or in CI, with the skills installer commands in [adapters/tools.md](../adapters/tools.md#skills-installer).

### Skills the Kit leaves out on purpose

Each fact has one home, so the Kit does not pin a skill that does the job of a Kit page.

| Skill | Source | Why it is left out |
| ----- | ------ | ------------------ |
| `setup-matt-pocock-skills` | Matt Pocock | Bootstrap writes the pages it would write: `docs/agents/issue-tracker.md`, `docs/agents/domain.md`, and the Agent skills block in `.rules` |
| `pr` | Matt Pocock | The Evidence bundle template owns the pull request body |
| `correct` | pstack | The Kit's `correction` skill owns this job |
| `no-comments` | pstack | The [no-comments](../checks/no-comments.md) check owns this rule |
| `create-verification-skill` | pstack | [core/agents/verification.md](../core/agents/verification.md) owns the Verification contract |
| `tdd`, `teach` | pstack | Matt Pocock's skills of the same name are pinned; two skills cannot share a name |
| `poteto-mode` | pstack | It sets one person's working style; it belongs in the Owner's own agent setup |
| Other `principle-*` skills | pstack | No pinned skill loads them |
| `typescript-best-practices` | pstack | The Kit names no tech stack |

### The lock file

The lock file entry has this shape:

```json
{
  "version": 2,
  "skills": {
    "grill-me": {
      "source": "mattpocock/skills",
      "sourceType": "github",
      "skillPath": "skills/productivity/grill-me/SKILL.md",
      "commit": "5863806dd4e15c63174bee4e7515a4256792ecfa",
      "computedHash": "ab1acb9fbd9eaee03da254abb8a8de228d1e7a07475fe959da4a5eefa34ea4b9"
    }
  }
}
```

When the installed hash differs from the tables above, the upstream skill changed after this Kit release.
Read the change before you accept it.

## Writing a Project skill

- Put it in the Project's skills folder as `<name>/SKILL.md`.
- Start with frontmatter: `name`, `description`, and `disable-model-invocation: true` when only the Owner may start it.
- Name the documents it reads; do not copy their rules into it.
- When it calls an agent's own tool, name the tool for each agent the Project uses, from [adapters/agents.md](../adapters/agents.md).
- Do not add a skill that works in one agent only.
- Do not give it the name of a pinned skill.
