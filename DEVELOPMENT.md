# Working on CMU Web Projects

Set up your computer once for any CMU Web project. Each project's own
DEVELOPMENT.md covers its commands; this page covers the tools they share.

## What You Need

- **Git.**
- **The project's language.** Each project names its version in its own
  files:
  - **Node** for the Web Kilt, PlaceMellon, QRnegie, FencePost and the Kilt
    MCP server. Use the version the project names (its `.node-version`, or
    the Node image in its Dockerfile).
  - **Ruby 3.2 or later, with Bundler,** for a project's gem (the Web Kilt,
    PlaceMellon, QRnegie and Jekyll & Plaid).
  - **Python 3** for projects built with Python scripts, such as Hunt &
    Peck. They use the standard library only.
- **Docker or Podman,** only to run a project's browser tests or its local
  stack (Forbes & Moreword runs WordPress this way). Either works; the
  commands are the same.

## Each Project's Tools

| Project               | Language                    | Everyday check                                                    | Browser tests                                                    | Run it locally                                                 |
| --------------------- | --------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------- | -------------------------------------------------------------- |
| Web Kilt (both lines) | Node, plus Ruby for the gem | `npm run check` (needs the network: it reads CMU's live template) | `npm run test:browser` on line 0 (needs the network)             | `npm run docs`                                                 |
| PlaceMellon, QRnegie  | Node, plus Ruby for the gem | `npm run check`                                                   | None                                                             | `npm run dev`                                                  |
| FencePost             | Node                        | `npm run check`                                                   | None locally; the Test Action workflow runs the action in CI     | `npm run dev`                                                  |
| Kilt MCP server       | Node                        | `npm run check`                                                   | None                                                             | `npm run dev`                                                  |
| Jekyll & Plaid        | Ruby                        | `bundle exec rake test` in `packages/jekyll`                      | None                                                             | `bundle exec jekyll serve` in `docs/` (see its DEVELOPMENT.md) |
| Forbes & Moreword     | PHP (WordPress), in Docker  | `dev/run.sh check` (with `dev/run.sh up` running)                 | `npm run test:browser`, or `dev/run.sh browser` with Docker only | `dev/run.sh up`                                                |
| Scaife & Sound        | Python                      | `python3 tools/build.py`                                          | `python3 test/run.py` (Docker or Podman)                         | `python3 test/run.py` starts Vaultwarden with the theme        |
| The Cut & Paste       | Python                      | `python3 tools/pages.py` (builds the pages; no checks yet)        | None                                                             | `python3 -m http.server 4200 --directory docs`                 |

## Checks

Every project has an everyday check, one command that needs nothing beyond
the project's language (e.g., `npm run check`). Run it before opening a pull
request. Continuous integration runs it again, with everything else.

Browser tests are separate commands (e.g., `npm run test:browser`) that use
[Playwright](https://playwright.dev/). Run them when you change how a page
looks or behaves. The first time, install Playwright's browser:

```bash
npx playwright install chromium            # Node projects
python3 -m playwright install chromium     # Python projects
```

## Formatting

Every project formats its files the same way, so a diff shows only real
changes:

- **[Prettier](https://prettier.io/)** formats Markdown, YAML, JSON, CSS,
  SCSS, JavaScript, TypeScript and plain HTML, with the same settings in
  every project (`.prettierrc`: 100-character lines, double quotes, trailing
  commas).
- **[Ruff](https://docs.astral.sh/ruff/)** formats and lints Python.
- **Left alone:** files a formatter would break, which each project lists
  in its `.prettierignore`: Liquid templates, WordPress block markup, PHP
  (which follows WordPress's own style) and LaTeX.

A project's hooks run its formatters, so you don't need Node to format a
Python or Ruby project. To format by hand, run `prek run --all-files`.

## Commit Hooks (Optional)

Projects with a `.pre-commit-config.yaml` run their fast checks, such as
formatting and linting, before each commit with
[prek](https://github.com/j178/prek). Install prek once, by whichever means
you prefer:

```bash
brew install prek          # macOS or Linux with Homebrew
uv tool install prek       # or: pipx install prek
npm install -g @j178/prek  # or with Node
```

Then, in each project you work on:

```bash
prek install
```

The hooks only save you a round trip; continuous integration checks
everything either way.

## Branches and Commits

- Branch from the project's main branch: `main`, or the template line's
  branch (`0.x` or `1.x`) where a project has one.
- Write commit subjects as
  [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
  (e.g., `fix: keep the menu open on phones`).
- Versions use semantic versioning as plain numbers (e.g., `1.4.0`).

## Writing

Docs, labels and examples follow the Web Kilt's
[writing conventions](https://webkilt.andrew.cmu.edu/0/conventions.html).
Use made-up content in examples, such as Andrew Carnegie (ac00) for a
person.

## Listing a Project

The CMU Web home page and each project's More From CMU Web section list the
projects from the repositories themselves. There's no list to edit by hand.

- **Name:** the README's first heading.
- **Description:** the repository's description on GitHub, one sentence,
  shown on every card.
- **Address:** the repository's website on GitHub. The README's Site line
  matches it.
- **Picture:** the README's image (`docs/images/preview.jpg` for a tool,
  `in-use.jpg` for a theme).
- **Tool or theme:** the repository's topic: `cmu-web-tool`,
  `cmu-web-theme-site` or `cmu-web-theme-app`. Only these exact topics list
  a project. `cmu-web-tool-addon` marks a part of another project, such as
  an MCP server, which is reached from that project instead.

The home page rebuilds every hour and reads the current values, so a changed
description shows there within the hour. Each project's About page loads
the same list from the home page when it's viewed, so it changes at the
same time, without a release. Without JavaScript, the section is a link to
the home page.

To change how a project is listed, edit its repository's description,
website or topics on GitHub, or its README; never the cards.
