# slidev-addon-seconde

A [Slidev](https://sli.dev) addon with the reusable Vue components used by the
**Informatica 2c** deck.

Slidev auto-registers components shipped by an addon, so no import is needed in
the slides:

```md
<Badge accent>new</Badge>
<Badge round>vs</Badge>
<Counter :count="3" />
```

## Components

| Component | Props | Description |
| --------- | ----- | ----------- |
| `Badge`   | `accent`, `round` | Small uppercase pill label. |
| `Counter` | `count` | Minimal -/+ counter. |

> The components are unstyled beyond their markup: `.badge` / `.badge-accent` /
> `.badge-round` live in the [`slidev-theme-seconde`](../theme) theme.

## Usage

```md
---
addons:
  - slidev-addon-seconde
---
```

Or as an npm git dependency:

```json
{
  "dependencies": {
    "slidev-addon-seconde": "github:russell-informatica/slidev-addon-seconde"
  }
}
```
