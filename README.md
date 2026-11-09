# website-design

A Claude Code skill that gives one entry point, `/website-design`, for designing and building websites.

It bundles 84 DESIGN.md design systems from [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) and runs a fixed workflow: brief → choose a design language → taste rules → build → verify in a browser → audit. Each step hands off to a specialised skill:

- [taste-skill](https://github.com/leonxlnx/taste-skill)
- [ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
- [impeccable](https://github.com/pbakaus/impeccable)
- [Playwright CLI](https://github.com/microsoft/playwright)
- [img2threejs](https://github.com/img2threejs/img2threejs)

If any of them is missing, the skill installs it before starting.

## Install

In Claude Code:

```text
/plugin marketplace add KW-0220/website-design-skill
/plugin install website-design@website-design
```

Or from a shell:

```bash
claude plugin marketplace add KW-0220/website-design-skill
claude plugin install website-design@website-design --scope user
```

Then start a new session and run `/website-design`.

## License

MIT. The files in `skills/website-design/design-md/` come from VoltAgent/awesome-design-md under its MIT license (`skills/website-design/design-md/LICENSE`). They describe the visual style of public websites as a design language only; don't use them to copy a brand's logo, name, imagery or copy.
