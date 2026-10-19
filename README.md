# WCAG Scanner — архітектурні before/after fixtures

Власний, стабільний корпус before/after сторінок для регресійного
тестування [wcag-сканера](../..) — на відміну від сторонніх дзеркал
(напр. `legionerscu.github.io/bad-demo-mirror`), контент тут повністю
під нашим контролем і не може розійтися з очікуваним станом.

Кожна пара `<architecture>/{before,after}/index.html` містить кілька
навмисних, задокументованих дефектів під конкретні `ruleId` власних
Stage 3-модулів сканера. `manifest.json` — ground truth (хто писав
дефекти, той і знає точно, що має спрацювати, а що ні).

## Структура

| Папка | Архітектура | Дефекти |
|---|---|---|
| `static-html/` | Семантичний server-rendered HTML (gov/CMS) | heading-h1-missing, contrast-insufficient, image-alt-generic-or-filename, keyboard-positive-tabindex |
| `spa-hydration/` | Клієнтський SPA-віджет (модалка, fixed-хедер) | keyboard-trap, focus-obscured-minimum, keyboard-no-focus-indicator |
| `shadow-dom/` | Custom element з відкритим shadow root | contrast-insufficient, motion-autoplay-no-control (навмисно лише ці два — документована межа scope для інших модулів) |
| `div-soup-layout/` | Div-only верстка без landmark-тегів | heading-h1-missing + задокументований reading_level-пробіл (FN сьогодні, не регресія) |
| `narrow-viewport-reflow/` | Фіксована ширина / вузький viewport | reflow-horizontal-scroll-320, target-size-too-small |

## Використання

Хоститься як GitHub Pages — кожна сторінка повинна бути доступна за
стабільним URL (`https://<user>.github.io/wcag-scanner-fixtures/<page>/before/`
і `.../after/`). Після публікації ці URL підставляються як новий
corpus-файл у `scripts/validation/corpora/` продуктового репозиторію —
той самий `run-*`/`classify`/`diff-baseline` інструмент, що й для ACT
Rules/GDS корпусів.
