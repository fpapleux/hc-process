# Import Log

## 2026-07-12 Initial Leantime Theme Import

Source:

```text
Repository: https://github.com/Leantime/leantime
Branch: master
Commit: 4bd9f644785f3656a2caee7bd5e1edac7ab1e19c
License: AGPL-3.0
```

Copied upstream files:

- `LICENSE` -> `upstream/LICENSE.AGPL-3.0`
- `public/theme/default/theme.ini`
- `public/theme/default/css/light.css`
- `public/theme/default/css/dark.css`
- `public/theme/minimal/theme.ini`
- `public/theme/minimal/css/light.css`
- `public/theme/minimal/css/dark.css`
- `public/assets/css/components/accessibility.css`
- `public/assets/css/components/forms.css`
- `public/assets/css/components/nav.css`
- `public/assets/css/components/tables.css`
- `public/assets/css/components/text-styles.css`
- `public/assets/css/components/structure.css`
- `public/assets/css/components/glass.css`
- `app/Core/UI/Theme.php`
- `app/Views/Composers/Header.php`
- `app/Views/Templates/sections/header.blade.php`

Immediate local adaptations:

- Added normalized Human-Codex token aliases in `../tokens/light.css`,
  `../tokens/dark.css`, and `../tokens/tokens.json`.
- Added framework-neutral component and layout starter CSS in
  `../components/base.css` and `../layouts/document.css`.
- Kept copied upstream files under `upstream/` unchanged except for path
  relocation.

Intentionally skipped in this import:

- Product-specific Leantime boards, calendars, tours, media manager, editor,
  and modal styles.
- Blade component trees not needed for the first portable theme contract.
- Build pipeline and PHP runtime integration files.
