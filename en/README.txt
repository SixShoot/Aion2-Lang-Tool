# Aion2-Steam-CN (永恒之塔2 国际服一键简繁汉化)

This repo is organized for cleaner locale support.

- English source assets are kept in `en/` and are preferred.
- Chinese (`zh/`) remains the fallback source.
- Polish (`pl/`) is reserved for future localized work and falls back to `zh/` until assets are added.
- The legacy root-level English resource files are accepted as a compatibility fallback.

Language layout:
- `zh/` : Simplified Chinese fallback
- `zh-TW/` : Traditional Chinese
- `en/` : English source locale (preferred)
- `pl/` : Polish translation target (future)

The scripts are configured to prefer `en/` for English and fall back to repo-root English or `zh/` if needed.
