# Interface Audit Skills

Two portable [Agent Skills](https://agentskills.io/specification) for auditing and repairing user interfaces. Use them independently or pass an audit report from one agent to another.

| Skill | Use it to |
| --- | --- |
| [interface-audit](skills/interface-audit/SKILL.md) | Audit a screen, component, journey, or whole accessible app. Apply the [UX and UI checklist](skills/interface-audit/references/checklist.md) and save a report with evidence, findings, and coverage gaps. |
| [interface-fix](skills/interface-fix/SKILL.md) | Fix one finding, selected findings, or the full actionable backlog. Resume from saved progress and verify each repair against its acceptance checks. |

The checklist covers navigation, visual implementation, accessibility, forms, search, payments, performance, platform controls, AI features, and user research. The [visual clarity and information design module](skills/interface-audit/references/visual-content.md) covers what a reader sees first, how to structure copy, and when to use lists, cards, tables, charts, or disclosures. It works with read-only and interactive pages. Both skills use the same [report format](skills/interface-audit/references/report-format.md).

## Install

Copy the complete skill folder or folders into your agent's project skill directory. Include the references inside each folder.

| Agent | Project directory |
| --- | --- |
| [Codex](https://developers.openai.com/codex/skills) | `.agents/skills/` |
| [Claude Code](https://code.claude.com/docs/en/skills) | `.claude/skills/` |
| [Cursor](https://cursor.com/docs/skills) | `.cursor/skills/` or `.agents/skills/` |

From your target project's root, with this repository cloned elsewhere, you can copy both folders as follows. Set `skills_root` to the directory for your agent and check for existing folders with the same names before copying.

```sh
skills_root=.agents/skills
mkdir -p "$skills_root"
cp -R /path/to/interface-audit-skill/skills/interface-audit "$skills_root/"
cp -R /path/to/interface-audit-skill/skills/interface-fix "$skills_root/"
```

For another Agent Skills runtime, use its skill directory. In a runtime without skill discovery, give the agent the relevant `SKILL.md` and its `references/` folder. Neither skill requires a particular browser, model, language, or framework. A full audit needs access to the running interface and the relevant test tools. A repair needs authorized edit access.

## Use

1. Ask for a focused or whole-app audit. Provide a URL, workspace app, build, prototype, or attached design. Add scenarios, devices, roles, and any test environment details that matter.
2. Review the report. The default is `.interface-audit/<audit-id>/report.json` in the audited project's workspace. If the agent cannot write a file, pass the returned report to the repair agent.
3. Ask for the next finding, selected IDs, all actionable findings, or verification only.

| Goal | Example request |
| --- | --- |
| Focused audit | Use `interface-audit` to audit checkout on mobile and with a keyboard at https://staging.example.com. |
| Read-only page audit | Use `interface-audit` to review this pricing page. A first-time visitor should see whether annual billing is required and compare exact prices and seats. |
| Whole-app audit | Use `interface-audit` to audit the whole app in this workspace as a signed-in customer. |
| One fix | Use `interface-fix` to fix the next finding in this report. |
| Selected fixes | Use `interface-fix` to fix F-002 and F-005 in this report. |
| Fix all or resume | Use `interface-fix` to fix all actionable findings in this report and save progress after each. |
| Verify only | Use `interface-fix` to verify F-002 on this build. |

See [an example report](examples/report.json) for the handoff shape. Keep project reports and evidence in an authorized workspace. Share them only with the intended audience.
