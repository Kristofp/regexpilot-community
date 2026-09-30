<div align="center">

# RegexPilot

**A visual regex builder for macOS that runs your pattern on the real engine.**

[Website](https://regexpilot.com) · [Download](https://regexpilot.com/download) · [Mac App Store](https://apps.apple.com/us/app/regexpilot/id6764146125?platform=mac) · [VS Code extension](https://marketplace.visualstudio.com/items?itemName=regexpilot.regexpilot-vscode)

</div>

<img src="https://regexpilot.com/demo-preview.png" alt="RegexPilot editing a regular expression as a railroad diagram, with live match results" width="100%">

---

## What is this repository?

RegexPilot's application source is **proprietary and closed-source** — it
isn't in this repo. This repository is the app's **public face**: the issue
tracker, and the two things that genuinely are open — bundled color
**themes** and **translations** — published as plain data under an MIT
license (see [License](#license) below).

If you're looking for the app itself: [download it](https://regexpilot.com/download),
get it from the [Mac App Store](https://apps.apple.com/us/app/regexpilot/id6764146125?platform=mac),
or `brew install --cask kristofp/regexpilot/regexpilot`.

## What RegexPilot does

RegexPilot turns a regular expression into a **railroad diagram you can edit
directly** — drag blocks from a palette, connect them on a track, and read a
plain-English explanation of every part as you work.

### Why it's different

Most regex tools test your pattern with JavaScript and hope it behaves the
same in the language you actually ship. RegexPilot bundles **native engines
for all 21 supported flavors** and runs the pattern through the real
interpreter for your target language, rather than an approximation:

| Flavor                                            | Engine                                                                                                                                                                                      |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| JavaScript / TypeScript                           | JavaScriptCore (the engine Safari runs), with `regress` (a pure-Rust ES2018 engine) as an automatic fallback if the system framework is unreachable                                         |
| Python                                            | CPython (`re` and `regex`)                                                                                                                                                                  |
| Java, Kotlin, Scala, Groovy, Clojure (JVM family) | `java.util.regex` via GraalVM native-image                                                                                                                                                  |
| Ruby                                              | Onigmo, statically linked                                                                                                                                                                   |
| C#                                                | .NET                                                                                                                                                                                        |
| PHP                                               | PCRE2 via static-php-cli                                                                                                                                                                    |
| Go                                                | Go's stdlib `regexp` (RE2, no backtracking)                                                                                                                                                 |
| Rust                                              | the `regex` crate (RE2-family, default) or `fancy-regex` (backtracking, selectable — for lookaround/backreferences)                                                                         |
| Swift                                             | `NSRegularExpression` (ICU regex)                                                                                                                                                           |
| PostgreSQL                                        | PostgreSQL's own Spencer ARE engine, statically linked                                                                                                                                      |
| MySQL                                             | ICU                                                                                                                                                                                         |
| Perl                                              | Bundled microperl                                                                                                                                                                           |
| Dart                                              | Dart's own `RegExp` (V8-derived Irregexp) via the user's system Dart SDK, with a JavaScriptCore-based approximation as fallback if no SDK is installed                                      |
| Elixir                                            | Erlang's `:re` (PCRE2-based) via the user's system Elixir/Erlang install, with a PCRE2 approximation as fallback if it isn't installed                                                      |
| R                                                 | base R's TRE engine by default (POSIX, no lookaround), or PCRE2 (`perl=TRUE`) when selected — via the user's system R install, with a PCRE2 approximation as fallback if it isn't installed |
| Shell                                             | Bundled TRE, statically linked (POSIX BRE/ERE) — not BSD or GNU grep/sed/awk, which the sandboxed app cannot execute                                                                        |

**21 flavors in total:** JavaScript, TypeScript, Python, Java, C#, PHP, Go,
Rust, Swift, Kotlin, Ruby, PostgreSQL, MySQL, Shell, R, Dart, Scala, Perl,
Elixir, Groovy, Clojure.

### Other things it does

- **AI-assisted generation** — describe a pattern in plain English and get a
  working draft. Bring your own API key (OpenAI, Claude, Gemini, DeepSeek,
  Qwen, Mistral, Groq, OpenRouter, xAI), point it at any custom
  OpenAI-compatible endpoint, or run fully offline with LM Studio, Ollama,
  llama.cpp or LocalAI.
- **Voice dictation** — on-device, via a bundled Whisper model, 100 languages.
- **Live testing** — paste sample text; matches and capture groups highlight
  as you type, in both match and replace modes.
- **Pattern library** — curated patterns plus folders, tags, search, import
  and export.
- **Code generation** — copy your pattern as ready-to-paste code across all
  21 supported flavors.
- **Editor integration** — the [VS Code extension](#vs-code-extension) opens
  a selected regex literal in the app and can sync edits back to your file.
- **Color themes** — a bundled set of community-style themes (Dracula,
  Catppuccin, Nord, Tokyo Night, Gruvbox, Solarized, GitHub, Rosé Pine, One,
  Ayu, Everforest, Kanagawa, Night Owl, Tomorrow, plus the default), all
  free, with import/export for your own — see [Themes](#themes) below.
- **Localization** — English, Dutch, French, German in the shipping app; see
  [Translations](#translations) below for how the strings behind that are
  published and how to contribute one.
- **Privacy by construction** — no accounts, no telemetry, no analytics.
  Nothing is reported to a server on its own; creating a share link is
  always a deliberate action, never automatic.

## Free vs. licensed

RegexPilot is a **one-time purchase**, not a subscription.

**Free, forever:**

- The full visual builder — canvas, drag-and-drop, inspector
- JavaScript and TypeScript flavors only
- Match/replace testing
- Pattern library (local, capped at 20 patterns)
- Every bundled theme, plus theme import/export
- Export to JSON

**€19 one-time license** (v2 owners get a 50% upgrade discount) unlocks:

- All 21 flavors, with their authentic native engines
- AI-assisted regex generation (bring your own API key — AI is not available
  on the free tier, with or without your own key)
- Unlimited pattern library
- All export formats, including code generation, PNG/SVG, SnippetsLab and
  VS Code

**30-day free trial** of everything above starts the first time you select a
non-JavaScript flavor — no signup, no credit card. After the trial, the app
reverts to the free tier; your non-JS library patterns stay visible but
become read-only until you unlock.

There is no hosted-AI subscription and no free-tier AI: AI generation is
entirely bring-your-own-key, and it's one of the things the trial/license
unlocks alongside the other 19 flavors.

## Privacy

RegexPilot reports nothing to a server on its own. There's no telemetry and
no analytics. The only network requests the app makes are ones you
explicitly trigger: an AI call using your own API key, or creating a share
link. License validation happens locally against a signed token.

## Download

Every release is published on this repo's
[Releases page](https://github.com/Kristofp/RegexPilot/releases/latest),
built and uploaded by the same release run that ships the app:

- **App (DMG):** [RegexPilot-universal.dmg](https://github.com/Kristofp/RegexPilot/releases/latest/download/RegexPilot-universal.dmg)
  — universal binary (Apple Silicon + Intel), signed and notarized.
- **All themes:** [regexpilot-themes.zip](https://github.com/Kristofp/RegexPilot/releases/latest/download/regexpilot-themes.zip)
- **All translations:** [regexpilot-translations.zip](https://github.com/Kristofp/RegexPilot/releases/latest/download/regexpilot-translations.zip)
- **Mac App Store:** [apps.apple.com/us/app/regexpilot](https://apps.apple.com/us/app/regexpilot/id6764146125?platform=mac)
  — same app; Apple handles payment and updates.
- **Homebrew:**

  ```sh
  brew install --cask kristofp/regexpilot/regexpilot
  ```

**Requirements:** macOS 14 (Sonoma) or later.

## VS Code extension

Select a regex in your editor and send it to the app — right-click **Open in
RegexPilot**, or use the CodeLens shown above a detected regex literal. Pick
**Edit in RegexPilot** instead and the two stay in sync: rebuild the pattern
on the canvas and the edit lands back in your file.

Install from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=regexpilot.regexpilot-vscode)
(one-click install and auto-update).

## Reporting a bug or requesting a feature

This repo hosts the **public issue tracker**. The app's source is private;
this exists so bugs and feature requests are visible and trackable.

- [Open a bug report](https://github.com/Kristofp/RegexPilot/issues/new?template=bug_report.md)
- [Open a feature request](https://github.com/Kristofp/RegexPilot/issues/new?template=feature_request.md)
- Or start from the [issue chooser](https://github.com/Kristofp/RegexPilot/issues/new/choose)

Please include your RegexPilot version (RegexPilot menu → About RegexPilot),
macOS version, the regex flavor you were using (if relevant), and steps to
reproduce. Search existing issues first. For urgent account or billing
issues, email support@regexpilot.com instead of filing an issue.

## Themes

[`themes/`](themes/) holds every bundled color theme as a ready-to-import
JSON file — `<id>.json`, one per theme, generated straight from the app's
own theme catalog by the same serializer "Settings → Appearance →
Export Theme…" uses, so the file you'd get from the app and the file published here
are the same shape. See [`themes/README.md`](themes/README.md) for the full
list. To use one, download a file and import it via Settings → Appearance →
"Import Theme…".

## Translations

[`translations/`](translations/) holds the app's UI strings for every
shipping locale (English is the reference), plus a generated coverage table
and a short guide for contributing a fix or a new translation — see
[`translations/README.md`](translations/README.md). Short version:
edit the JSON, keep `{placeholder}` tokens intact, and open a pull request
or an issue with your changes attached. Contributions here are reviewed and
merged into the app's bundled catalog by hand before they ship in a
release — this repo isn't wired directly into the live app.

## License

The **RegexPilot application** (this repo does not contain its source) is
proprietary, closed-source software.

The contents of **[`themes/`](themes/)** and **[`translations/`](translations/)**
in this repository are licensed under the **MIT License** — see
[`LICENSE`](LICENSE) at the repository root, and the copy inside each of
those two directories.
