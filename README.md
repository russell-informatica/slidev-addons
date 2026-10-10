# slidev-addons-russell

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
| `ListTracker` | `items`, `trackers`, `highlight`, `showIndexes` | Draws a list as a grid with small index labels and one moving pointer per tracked variable, driven by clicks. |

### `ListTracker`

Draws the elements of a list in a grid, with the indexes written small below each
cell. Each entry of `trackers` is a pointer that slides to the index it holds at
the current click. Values use the same absolute click numbers as `v-click`:

```md
<ListTracker
  :items="[2, 4, 6, 8, 10]"
  :trackers="{ i: { 1: 0, 4: 1, 7: 2, 10: 3, 13: 4, 16: 5 } }"
/>
```

At click `c` a pointer takes the value of the greatest key `<= c`; before the
first key, or when the value is `null`, it is hidden. Indices `-1` and
`items.length` place the pointer half a cell outside the grid, so loop-exit
states (`i = -1`, `i = n`) stay visible. Multiple pointers can be tracked at
once, each on its own lane:

```md
<ListTracker
  :items="[1, 2, 3, 4, 5, 6]"
  :trackers="[
    { name: 'i', values: { 1: 0, 4: 1, 7: 2 } },
    { name: 'j', values: { 1: 5, 4: 4, 7: 3 } },
  ]"
/>
```

> The components are unstyled beyond their markup: `.badge` / `.badge-accent` /
> `.badge-round` live in the [`slidev-theme-russell`](../theme) theme.

## Usage

```md
---
addons:
  - slidev-addons-russell
---
```

Or as an npm git dependency:

```json
{
  "dependencies": {
    "slidev-addons-russell": "github:russell-informatica/slidev-addons"
  }
}
```
