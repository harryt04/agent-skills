# Azure DevOps Wiki Markdown Reference

> Compact syntax reference for AI agents writing Azure DevOps wiki page content.
> Source: https://learn.microsoft.com/en-us/azure/devops/project/wiki/markdown-guidance?view=azure-devops

---

## Critical Behavioral Differences from Standard Markdown

- **Line breaks**: Two trailing spaces + Enter = soft break within paragraph. Enter alone merges lines. Two Enters = new paragraph.
- **No JavaScript or iframes** (except via `::: video :::` wrapper).
- **Underline**: No native syntax — use `<u>text</u>` HTML.
- **Anchor IDs**: Auto-generated from headings: spaces → hyphens, uppercase → lowercase, most special chars → hyphens.
- **TOC tag is case-sensitive**: `[[_TOC_]]` works; `[[_toc_]]` does not.
- **Flowcharts**: Use `graph` not `flowchart`. `---->` and subgraph links are unsupported.

---

## Headers

```md
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

Only `#` syntax headers appear in the TOC — HTML heading tags (`<h1>`) are ignored.

---

## Paragraphs and Line Breaks

```md
Line one with two trailing spaces  
Line two directly below (soft break)

New paragraph (blank line above)
```

---

## Emphasis

| Style | Syntax | Example |
|---|---|---|
| Italic | `*text*` or `_text_` | *italic* |
| Bold | `**text**` or `__text__` | **bold** |
| Strikethrough | `~~text~~` | ~~struck~~ |
| Bold + italic | `**_text_**` | **_combined_** |
| Underline (HTML only) | `<u>text</u>` | underlined |

---

## Block Quotes

```md
> Single line or paragraph quote

>> Nested quote

>>> Triple nested
```

---

## Horizontal Rules

```md
---
```

Requires a blank line before the `---`.

---

## Code

### Inline code
```md
`inline code`
```

### Code block (with language highlighting)
````md
```js
const x = 1;
```
````

### Supported language identifiers
`js`, `ts`, `csharp`, `python`, `bash`, `sql`, `json`, `xml`, `md`, and all [highlight.js languages](https://github.com/highlightjs/highlight.js/tree/stable-11/src/languages).

### 4-space indent → code block
```md
    This line auto-converts to code (4 leading spaces)
```

### Suggestion diff (pull requests only)
````md
```suggestion
  for i in range(A, B+100, C):
```
````

---

## Tables

```md
| Header 1 | Header 2 | Header 3 |
|:---------|:--------:|---------:|
| left     | center   | right    |
| data     | data     | data     |
```

- Align: `:---` left, `:---:` center, `---:` right
- Escape pipe: `\|`
- Line break in cell: `<br/>` (wiki only, not elsewhere)
- Add blank space before/after work item or PR references in table cells
- No checklists inside tables

---

## Lists

### Ordered
```md
1. First
1. Second
1. Third
```
(All items can use `1.` — auto-numbered on render)

### Unordered
```md
- Item
- Item
* Also valid
```

### Nested lists
```md
1. Top level
   - Nested bullet (3 spaces indent)
      - Double nested (6 spaces)
   1. Nested ordered
```

Blank line before and after the outermost list block; no extra breaks needed around nested lists.

---

## Links

```md
[Display text](https://url.com)
[Relative file](./page.md)
[Absolute git path](/folder/page.md)
[TFVC path]($/project/folder/page.md)
```

### Wiki page links
```md
[Child page](/parent-page/child-page)
[Anchor link](#heading-anchor-id)
[Cross-page anchor](other-page#section-id)
```

### Anchor ID rules
- Heading text → lowercase, spaces → `-`, most special chars → `-`
- Example: `## Team #1: Release Wiki!` → `#team-1--release-wiki`

### Work item auto-link
```md
#1234
```
Prefix `#` with `\` to suppress: `\#color-hex`.

### URL auto-linking
Plain `https://` URLs in wiki/PR comments auto-link.

> **Security**: `file://` links are not supported.

---

## Images

```md
![Alt text](./image.png)
![Alt text](https://external.url/image.jpg)
![Alt text](/media/folder/image.png =500x250)
```

### Size syntax
```md
![Alt](./img.png =500x250)   <!-- width=500, height=250 -->
![Alt](./img.png =300x)      <!-- width=300, auto height -->
```

No space before/after `x`; one space before `=`.

---

## Checklists / Task Lists

```md
- [x] Completed item
- [ ] Incomplete item
1. [x] Numbered completed
1. [ ] Numbered incomplete
```

- Supported in wiki pages and pull requests
- Not supported inside tables

---

## Emoji

```md
:smile:  :+1:  :tada:  :warning:
```

- Uses GitHub emoji names (most supported)
- Custom emoji (`:bowtie:`) not supported
- Escape with `\`: `\:smile:` renders as literal `:smile:`
- In PR comments, use `\\` to escape

Full list: https://github.com/ikatyang/emoji-cheat-sheet

---

## Escaping Special Characters

Prefix with `\` to render as literal:

```md
\# not a heading
\*not italic\*
\_not italic\_
\[not a link\]
\\ literal backslash
\` literal backtick
```

For backslash in some contexts use HTML: `&#92;`

---

## Mathematical Notation (KaTeX)

### Inline
```md
Area is $\pi r^2$
```

### Block
```md
$$
A_{triangle}=\frac{1}{2}({b}\cdot{h})
$$
```

### Examples
```md
$\alpha, \beta, \gamma, \delta$

$$\sum_{i=1}^{10} t_i$$

$$\int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x$$
```

Supported elements: symbols, Greek letters, operators, powers/indices, fractions, binomials. See [KaTeX docs](https://khan.github.io/KaTeX/function-support.html).

---

## Wiki-Specific Features

### Table of Contents
```md
[[_TOC_]]
```
- Case-sensitive — must be uppercase
- Generates from `#` headings only (not HTML tags)
- Only first instance on a page is rendered
- Extra formatting in headings is stripped in TOC entries

### Table of Subpages
```md
[[_TOSP_]]
```
- Lists all direct child pages of the current wiki page
- Case-sensitive

### Collapsible Sections
```html
<details>
  <summary>Click to expand!</summary>

  ## Heading inside collapsible
  - Content here (blank line after </summary> is required)
</details>
```

- Blank line after `</summary>` is **required**
- Blank line after `</details>` required when multiple collapsible sections follow each other
- Markdown and HTML both valid in body

### Embedded Video
```md
::: video
<iframe width="640" height="360" src="https://www.youtube.com/embed/VIDEO_ID" allowfullscreen style="border:none"></iframe>
:::
```

Supports YouTube and Microsoft Streams.

### Embedded Azure Boards Query Results
```md
:::
query-table <query-guid>
:::
```

### @ Mentions
```md
@<user-alias>
@<{identity-guid}>
```

Opens autosuggest in the editor; use GUID format when editing raw Markdown.

---

## Mermaid Diagrams

Wrap in `::: mermaid ... :::` syntax.

```md
::: mermaid
<diagram-type>
  <diagram content>
:::
```

### Supported diagram types

| Type keyword | Description |
|---|---|
| `sequenceDiagram` | Interaction/sequence flows |
| `gantt` | Gantt/project timeline charts |
| `graph LR` / `graph TB` | Flowcharts (use `graph`, NOT `flowchart`) |
| `classDiagram` | OOP class relationships |
| `stateDiagram-v2` | State machine diagrams |
| `journey` | User journey maps |
| `pie` | Pie charts |
| `requirementDiagram` | Requirements and verifications |
| `gitGraph` | Git commit/branch graphs |
| `erDiagram` | Entity Relationship diagrams |
| `timeline` | Chronological event timelines |

### Mermaid limitations
- `flowchart` keyword unsupported — use `graph`
- `---->` (long arrow) unsupported
- Links to/from `subgraph` unsupported
- Most HTML tags inside Mermaid unsupported
- Font Awesome unsupported
- Does not render in Internet Explorer

### Sequence diagram example
```md
::: mermaid
sequenceDiagram
    Alice->>Bob: Hello Bob
    Bob-->>Alice: Hi Alice!
:::
```

### Flowchart example
```md
:::mermaid
graph LR;
    A[Start] -->|step| B(Process) --> C{Decision}
    C -->|Yes| D[Done]
    C -->|No| E[Retry]
:::
```

### Gantt example
```md
::: mermaid
gantt
    title Project Plan
    dateFormat YYYY-MM-DD
    section Phase 1
    Task A :a1, 2025-01-01, 7d
    Task B :after a1, 5d
:::
```

---

## HTML in Wiki Pages

Rich HTML is supported in wiki pages (not pull requests). Markdown can be used inside HTML blocks if a blank line follows the opening tag.

```html
<p>

**Markdown** works inside HTML blocks with a blank line after the opening tag.

</p>

<font color="blue">Blue text</font>
<center>Centered text</center>
<span style="color:green;font-weight:bold">Green bold</span>
<del>strikethrough</del>
<ins>inserted</ins>
<sup>superscript</sup>
<sub>subscript</sub>
<tt>teletype</tt>
<small>small text</small>
<big>bigger text</big>
<u>underlined</u>
```

### Embed video via HTML
```html
<video src="https://example.com/video.mp4" width=400 controls></video>
```

---

## Feature Support Matrix

| Feature | Wiki | PR | README | Widget | Done board |
|---|:---:|:---:|:---:|:---:|:---:|
| Headers | ✓ | ✓ | ✓ | ✓ | ✓ |
| Paragraphs/breaks | ✓ | ✓ | ✓ | ✓ | ✓ |
| Block quotes | ✓ | ✓ | ✓ | ✓ | ✓ |
| Horizontal rules | ✓ | ✓ | ✓ | ✓ | ✓ |
| Emphasis | ✓ | ✓ | ✓ | ✓ | ✓ |
| Code highlighting | ✓ | ✓ | ✓ | — | — |
| Tables | ✓ | ✓ | ✓ | ✓ | — |
| Lists | ✓ | ✓ | ✓ | ✓ | ✓ |
| Links | ✓ | ✓ | ✓ | ✓ | ✓ |
| Images | ✓ | ✓ | ✓ | ✓ | — |
| Checklists | ✓ | ✓ | — | — | — |
| Emoji | ✓ | ✓ | — | — | — |
| Escape/ignore | ✓ | ✓ | ✓ | ✓ | ✓ |
| Attachments | ✓ | ✓ | — | — | — |
| Math (KaTeX) | ✓ | ✓ | — | — | — |
| Mermaid diagrams | ✓ | — | — | — | — |
| TOC `[[_TOC_]]` | ✓ | — | — | — | — |
| Subpages `[[_TOSP_]]` | ✓ | — | — | — | — |
| Collapsible sections | ✓ | — | — | — | — |
| Embedded video | ✓ | — | — | — | — |
| Query results table | ✓ | — | — | — | — |
| @ mentions | ✓ | — | — | — | — |
| HTML tags | ✓ | — | — | — | — |

---

## Supported Attachment Types (Wiki & PR)

| Category | Extensions |
|---|---|
| Images | `.png` `.gif` `.jpeg` `.jpg` `.ico` |
| Documents | `.md` `.txt` `.pdf` `.doc` `.docx` `.xls` `.xlsx` `.csv` `.ppt` `.pptx` `.msg` `.mpp` |
| Code | `.cs` `.xml` `.json` `.html` `.htm` `.lyr` `.ps1` `.rar` `.rdp` `.sql` |
| Compressed | `.zip` `.gz` |
| Visio | `.vsd` `.vsdx` |
| Video | `.mov` `.mp4` |

> Note: Code attachments and `.msg` files are not supported in PR comments.
