# SOGo themes

Public mirror of the edulution SOGo webmail themes, fetched by the edulution
platform to check for and apply SOGo theme updates.

Mirrored from the (private) `edulution-io/edulution-mail` repository at
`build/templates/sogo/`.

| File | `@theme` |
| --- | --- |
| `custom-theme.css` | `edulution-dark` |
| `light-theme.css` | `edulution-light` |

Each file carries a header the platform parses:

```css
/* @theme edulution-dark
   @version 0.1.9
*/
```

When the theme changes in `edulution-mail`, update these files (and bump the
`@version`) so deployments detect the new version.
