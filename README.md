# CRMiUM Presentation Skill (Bento Style)

Скіл для Claude Code, який генерує фірмові презентації CRMiUM у Bento-дизайні. Темна тема `#15171C` + помаранч `#EF652F` + Onest шрифт. Універсальний — кожен спікер створює власні презентації для будь-якого івенту.

## Що це дає

- HTML-презентація у фірмовому стилі — 1 файл, ~30 KB
- Презентувати з браузера (F11 fullscreen), стрілки ←→ для навігації
- Експорт у PDF через Ctrl+P → Save as PDF
- Готові базові слайди: cover, speaker intro, CRMiUM intro, agenda, Q&A з **QR на форму питань**, Resources з **QR на Telegram-бот**, contacts
- 11+ типів контентних слайдів: bullets, stats, bento, quote, image, compare, timeline, pains-grid, features-grid
- 4 мови: UA / EN / PL / RU
- Контакти спікера запитуються один раз і зберігаються (multi-speaker підтримка)
- 2 QR-коди по замовчуванню:
  - **На Q&A слайді** — аудиторія може задати питання анонімно через форму
  - **На Resources слайді** — підписка на Telegram-бот, куди після виступу кидаються корисні матеріали

---

## Установка

### Спосіб 1 — через Claude Code (recommended)

Скопіюй цей рядок у свій Claude Code:

```
Склонуй мені https://github.com/crmiummarketing/crmium-presentation-skill у папку ~/.claude/skills/crmium-presentation-bento — це скіл для генерації презентацій CRMiUM. Перевір що скіл доступний.
```

Claude склонує репо, після цього перезапусти Claude Code (якщо треба) — і скіл готовий до використання.

### Спосіб 2 — вручну через git

```bash
git clone https://github.com/crmiummarketing/crmium-presentation-skill ~/.claude/skills/crmium-presentation-bento
```

Перезапусти Claude Code.

### Спосіб 3 — завантажити ZIP

1. Зайди на https://github.com/crmiummarketing/crmium-presentation-skill
2. Code → Download ZIP
3. Розпакуй у `~/.claude/skills/crmium-presentation-bento/` (Windows: `C:\Users\<USER>\.claude\skills\crmium-presentation-bento\`)
4. Перезапусти Claude Code

---

## Як використовувати

У Claude Code просто напиши:

- "Зроби презентацію про впровадження Zoho для виробничої компанії"
- "Створи слайди для мого виступу на Zoho Summit"
- "Потрібна презентація на тему \"Як ми мігрували клієнта з Bitrix24 на Zoho\""

### Перший запуск

Скіл задасть 5 запитань:
1. **Мова** презентації (UA / EN / PL / RU)
2. **Формат** файлу (HTML / HTML+PDF / PPTX)
3. **Контакти спікера** — імʼя, посада, email, телефон, соцмережі, фото-URL, біо
4. **Тема цієї презентації** + аудиторія + контекст + кількість слайдів + чи додавати CRMiUM intro
5. **Драфт/тези** — якщо є готовий текст, передай його. Якщо ні — Claude згенерує драфт-каркас

Відповіді 1-3 збережуться у `~/.claude/skills/crmium-presentation-bento/user-config.md`.

### Multi-speaker — підтримка кількох спікерів

При **наступних запусках** скіл показує **список усіх збережених спікерів**:

```
Для кого ця презентація?
○ Олександр Мельник · Solutions Architect (минулого разу)
○ Марія Коваль · Senior CRM Consultant
○ Андрій Ткач · Marketing Director
○ + Додати нового спікера
```

Якщо вибрати існуючого — контакти автоматично підставляться. Якщо "+ Додати нового" — скіл проведе онбоардинг нового спікера і збереже його у список.

Один файл `user-config.md` = десятки спікерів. Жоден не вводить свої контакти двічі.

---

## Як презентувати

1. Відкрий згенерований `index.html` у Chrome / Edge / Firefox.
2. Натисни `F11` — fullscreen без браузерних панелей.
3. Навігація:
   - `→` `Space` `PageDown` — наступний слайд
   - `←` `PageUp` `Backspace` — попередній слайд
   - `Home` / `End` — перший / останній
   - `0-9` — стрибнути на слайд N
   - `F` — toggle fullscreen
   - `P` — print preview (для PDF)
   - `Esc` — вийти з fullscreen

---

## Експорт у PDF

1. Відкрий `index.html` у Chrome.
2. Натисни `Ctrl+P` (Cmd+P на Mac) або `P` всередині презентації.
3. Налаштування Print:
   - **Destination**: Save as PDF
   - **Layout**: Landscape
   - **Paper size**: A4 (або Custom 297mm × 167mm для 16:9)
   - **Margins**: None
   - **Options → Background graphics**: ✅ **УВІМКНИ** (інакше темний фон зникне)
4. Save → отримаєш `.pdf` файл який можна слати поштою.

---

## FAQ

**Як змінити збережені контакти спікера?**
Відкрий `~/.claude/skills/crmium-presentation-bento/user-config.md` у редакторі. Знайди блок з потрібним `id:` у списку `speakers:` — зміни поля → збережи.

**Як додати ще одного спікера вручну (не через скіл)?**
Додай новий блок у `speakers:` у `user-config.md`:
```yaml
  - id: новий-спікер
    name: "Імʼя"
    role: "Посада"
    email: "email@crmium.com"
    ...
```

**Як видалити спікера зі списку?**
Видали його блок з `speakers:`. Якщо це був `last_used_speaker` — встанови інший id.

**Як змінити мову для наступної презентації?**
Або відредагуй `user-config.md` (рядок `default_language:`), або скажи Claude "Згенеруй презентацію англійською" — він використає тимчасову мову не змінюючи конфіг.

**Як перенести налаштування на іншу машину?**
Скопіюй `~/.claude/skills/crmium-presentation-bento/user-config.md` на іншу машину у те ж місце. Усі спікери разом з ним.

**Як замінити фото спікера?**
Слайд `slide--speaker` має `<img src="...">`. Завантаж своє фото на CRMiUM CDN (через WP Media або через маркетинг-команду), отримай URL, заміни. Або тимчасово на безкоштовний хостинг (imgur, cloudinary).

**Як додати ще слайди до згенерованої презентації?**
Відкрий `index.html` у редакторі, скопіюй один із `<section class="slide ...">` блоків, заміни контент. Не змінюй структуру класів, бо JS-навігація розраховує total з кількості `.slide` елементів. Або попроси Claude "Додай ще 3 слайди типу stats про X" — він додасть.

**Працює офлайн?**
Майже. Google Fonts (Onest) підгружаються онлайн при першому відкритті. Якщо нема інтернету — застосується system-ui fallback (виглядає трохи інакше, але працює). Для повного офлайн — завантаж шрифти локально і інлайн `@font-face` у `<style>`.

**А якщо я не CRMiUM-спікер? Чи можу використати скіл?**
Так, скіл публічний (MIT). Просто заміни лого з CRMiUM CDN на своє у двох місцях у `index.html`, заміни `company-info.md` своїм elevator-pitch, і користуйся.

**Що це за QR-коди на Q&A і Resources слайдах?**
- **QR на Q&A слайді** — посилання на форму "задай питання". Аудиторія сканує і пише питання анонімно. Спікер бачить питання на ноутбуці і відповідає вголос. Корисно для шиньковатих або для онлайн-формату.
- **QR на Resources слайді** — посилання на Telegram-бот CRMiUM. Аудиторія підписується після виступу. Маркетинг-команда після виступу кидає у бот корисні матеріали (посилання які спікер згадував, кейси, чек-листи).

**Не хочу QR — як їх вимкнути?**
Скажи Claude при генерації: "без QR на питання" або "без Resources-слайда". Можна також відкрити готовий `index.html` і видалити `<section class="slide slide--qa">` / `<section class="slide slide--resources">` блоки — JS автоматично пере-порахує загальну кількість слайдів.

**Як замінити QR-коди на свої?**
Replace файли `assets/crmium-ask-question-qr.png` (питання) і `assets/crmium-ua-bot-qr.png` (Telegram) на свої. Розмір — 320×320px рекомендовано, PNG з прозорим або білим фоном.

---

## Технічні характеристики

- Single-file HTML (~30 KB)
- Без npm / build tools / фреймворків
- Шрифти: Google Fonts (Onest + JetBrains Mono)
- Анімації: CSS keyframes + JS-controlled transform
- Сумісність: Chrome 90+, Edge 90+, Firefox 90+, Safari 14+
- Тестовано: Windows / macOS / Linux

---

## Підтримка

Якщо щось зламалося — пиши у Slack `#marketing` або тегни `@alex` (CRMiUM).

Для зовнішніх pull requests — заходь на GitHub repo і відкривай Issue / PR.

---

## Ліцензія

MIT — використовуй вільно у CRMiUM і поза нею.
