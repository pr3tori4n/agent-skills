# Industry reference sources

Consult these when a style question isn't answered by the repo's own conventions or by SKILL.md. Fetch the specific page rather than relying on memory when the exact rule matters (e.g., a word-list entry or a docstring tag).

## Style guides (precedence order)

| Source | URL | Use for |
|---|---|---|
| Google developer documentation style guide | https://developers.google.com/style | **Primary.** Voice, tone, grammar, formatting, word list, code samples, procedures, accessibility, inclusive language |
| Microsoft Writing Style Guide | https://learn.microsoft.com/style-guide/welcome/ | **Secondary.** Cloud/UI terminology, bias-free communication, global-ready writing, A–Z word list |
| Plain language guidelines (U.S. federal) | https://www.plainlanguage.gov/guidelines/ | Simplifying sentences, audience-first organization |

Cloud-vendor-specific terminology: follow the vendor's own docs for product names and capitalization (AWS, Azure, Google Cloud docs), e.g., "Amazon S3", "Azure Key Vault", "Cloud Run".

## Documentation structure

| Source | URL | Use for |
|---|---|---|
| Diátaxis | https://diataxis.fr/ | Choosing and keeping to one doc type: tutorial, how-to, reference, explanation |
| Write the Docs guide | https://www.writethedocs.org/guide/ | Docs-as-code, README guidance, doc maintenance practices |
| The Good Docs Project templates | https://www.thegooddocsproject.dev/ | Templates for README, how-to, tutorial, concept, reference, troubleshooting, release notes |
| Google technical writing courses | https://developers.google.com/tech-writing | Sentence and paragraph craft, audience analysis, organizing large docs |
| *Docs for Developers* (Bhatti et al., Apress 2021) | — | End-to-end process: audience research, drafting, code samples, measuring docs |

## Specific document formats

| Format | Source | URL |
|---|---|---|
| Architecture Decision Record | Michael Nygard's ADR format | https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions |
| ADR templates (MADR) | Markdown Any Decision Records | https://adr.github.io/madr/ |
| Changelog | Keep a Changelog | https://keepachangelog.com/ |
| Versioning notes | Semantic Versioning | https://semver.org/ |
| Commit messages | Conventional Commits | https://www.conventionalcommits.org/ |
| HTTP API reference | OpenAPI Specification | https://spec.openapis.org/oas/latest.html |
| Architecture diagrams | C4 model | https://c4model.com/ |
| Diagrams in Markdown | Mermaid | https://mermaid.js.org/ |

## Markdown

| Source | URL | Use for |
|---|---|---|
| CommonMark spec | https://spec.commonmark.org/ | Portable Markdown syntax |
| GitHub Flavored Markdown spec | https://github.github.com/gfm/ | Tables, task lists, autolinks, alerts on GitHub |
| markdownlint rules | https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md | Structural lint rules (heading levels, list style, fenced code language) |
| Vale | https://vale.sh/ | Prose linting; ships Google and Microsoft style packages |

## Doc comments by language

| Language | Source | URL |
|---|---|---|
| TypeScript | TSDoc | https://tsdoc.org/ |
| JavaScript | JSDoc | https://jsdoc.app/ |
| Python | PEP 257 (docstring conventions) | https://peps.python.org/pep-0257/ |
| Python | Google Python Style Guide §3.8 (docstrings) | https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings |
| Python | numpydoc format | https://numpydoc.readthedocs.io/en/latest/format.html |
| Java | How to Write Doc Comments for Javadoc | https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html |
| C# / .NET | XML documentation comments | https://learn.microsoft.com/dotnet/csharp/language-reference/xmldoc/ |
| Go | Go Doc Comments | https://go.dev/doc/comment |
| Rust | rustdoc book; API Guidelines (documentation) | https://doc.rust-lang.org/rustdoc/ · https://rust-lang.github.io/api-guidelines/documentation.html |
| Kotlin | KDoc | https://kotlinlang.org/docs/kotlin-doc.html |
| Shell / Terraform / YAML | No formal standard; follow repo convention and Google Shell Style Guide | https://google.github.io/styleguide/shellguide.html |
