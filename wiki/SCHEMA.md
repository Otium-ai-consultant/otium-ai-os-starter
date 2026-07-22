# Knowledge Wiki — Rules

Three rules. That's all.

1. **One topic per page**, in `wiki/pages/<topic>.md`. Short lowercase-dash filenames (`supplier-pricing.md`).
2. **Everything is linked.** Every page: YAML `tags:` + `[[wikilinks]]` to related pages, and one line in `wiki/index.md`. Obsidian draws the graph from these links.
3. **Facts carry provenance.** A claim from outside gets its source (URL or "told by X, date").

Page template:

```markdown
---
tags: [<area>]
---
# <Topic>

<the knowledge, short and factual>

## Related
- [[<other-page>]]
```
