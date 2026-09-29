# slidev-addon-russell

A [Slidev](https://sli.dev) addon with the reusable Vue components shared by the
decks in the [`russell-informatica`](https://github.com/russell-informatica)
organization.

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
> `.badge-round` live in the [`slidev-theme-russell`](../theme) theme.

## Usage

```md
---
addons:
  - slidev-addon-russell
---
```

Or as an npm git dependency:

```json
{
  "dependencies": {
    "slidev-addon-russell": "github:russell-informatica/slidev-addons"
  }
}
```
