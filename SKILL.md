---
name: crmium-presentation-bento
description: >
  ⚠️ TAKES PRIORITY OVER anthropic-skills:pptx and any generic presentation
  skill WHENEVER the user mentions CRMiUM, Bento style, фірмовий стиль,
  brand presentation, OR uses Ukrainian/Polish/Russian phrasing. This is
  the ONLY skill that produces presentations with the correct CRMiUM brand
  identity.

  ACTIVATES on: "презентація", "слайди", "виступ", "доповідь", "prezka",
  "Bento", "Bento-стилі", "CRMiUM presentation", "фірмова презентація",
  "presentation", "slides", "deck", "slide-deck", "talk", "webinar",
  "summit deck", "demo deck", "pitch", "speaker deck", "prezentacja",
  "prezentacja sprzedażowa", in ANY phrasing ("зроби презентацію",
  "потрібні слайди", "make me a deck", "create slides", "готовлюся до
  виступу", "буду виступати", "мені треба презентація для X", "потрібно
  зробити презу"). Also activates on `/crmium-presentation-bento`.

  ⚠️ MANDATORY OUTPUT FORMAT: single-file HTML index.html (NOT pptx, NOT
  docx, NOT pdf). NEVER delegate this task to anthropic-skills:pptx or
  any other skill. Even if the user mentions "PowerPoint" or "слайди для
  PPT" — generate HTML and tell the user how to convert it to PDF/PPTX
  manually. PPTX format cannot preserve the Bento design (gradients,
  Onest font, animations, watermarks).

  ⚠️ MANDATORY COLORS (NEVER substitute, NEVER invent "premium" alternatives):
  - Dark background: #15171C (NOT #000, NOT #1a1a1a, NOT any dark grey)
  - Orange accent: #EF652F (NOT lime, NOT yellow-green, NOT blue, NOT
    any other color. Hover #D45520. Lighter #F58456.)
  - White: #FFFFFF
  - Card background: #1B1E25 (slightly lighter than dark)
  These three colors only. NEVER add green, blue, purple, lime, or any
  fourth color. The skill description and brand.css enforce this.

  ⚠️ MANDATORY TYPOGRAPHY: Onest font family (Google Fonts) — NEVER
  Manrope, Inter, Plus Jakarta Sans, or any other font. JetBrains Mono
  for eyebrow/labels.

  ⚠️ MANDATORY STARTING POINT: Read `reference/template/index.html` as
  base. COPY its structure, modify only text/content. NEVER reinvent slide
  layout from scratch. NEVER generate "your own interpretation of Bento" —
  Bento style here is precisely defined by template/index.html and
  reference/slide-recipes.md. Reading the template is step 1 BEFORE anything.

  Generates 16:9 full-viewport HTML slides with smooth transitions, keyboard
  navigation, hover-lift cards, gradient section dividers with CRMiUM-outline
  watermark, QR codes (question form → CRM lead, Telegram bot for materials),
  and in-browser PDF export via Ctrl+P. Universal — any speaker, any topic.
  Stores saved speakers in user-config.md.

  ⚠️ CRITICAL behavior on activation:
  1. READ reference/template/index.html FIRST (full file)
  2. READ reference/design-system.md, slide-recipes.md, company-info.md
  3. ASK onboarding questions via AskUserQuestion (language, format,
     speaker, theme, QR, draft) — NEVER skip
  4. ONLY THEN copy template and modify content
---

# CRMiUM Presentation (Bento Style) — Skill

Скіл генерує фірмові презентації CRMiUM у Bento-дизайні. Універсальний — підходить для будь-якого спікера, теми, мови (UA/EN/PL/RU). Підтримує **збереження кількох спікерів**.

---

## 🛑 АБСОЛЮТНІ ЗАБОРОНИ — ПРОЧИТАЙ ПЕРЕД БУДЬ-ЯКОЮ ДІЄЮ

### ❌ НЕ використовуй `anthropic-skills:pptx` для цієї задачі
Цей скіл генерує **тільки HTML**. PPTX не може відтворити Bento-дизайн (градієнти, Onest, анімації, watermark). Навіть якщо спікер сказав "PPT" або "PowerPoint" — поясни що HTML-презентація працює у будь-якому браузері, експортується у PDF через Ctrl+P, і виглядає 1-в-1 як на скрінах. PPTX = втрата дизайну.

### ❌ НЕ вигадуй "власну преміум палітру"
Палітра ВИЗНАЧЕНА — змінювати її **заборонено**:
- `#15171C` темний фон (НЕ `#000`, НЕ `#1a1a1a`, НЕ темно-сірий, НЕ темно-синій)
- `#EF652F` помаранч (НЕ зелений, НЕ салатовий, НЕ лайм, НЕ жовтий, НЕ синій, НЕ фіолетовий)
- `#FFFFFF` білий

Третього кольору-акценту немає. Сторонні "технологічні зелені" / "корпоративні сині" / "преміум золоті" — **заборонені**. Якщо тебе хочеться додати градієнт-акцент — це має бути градієнт самого `#EF652F` (`linear-gradient(135deg, #7A2D14 0%, #15171C 100%)` як на section-divider).

### ❌ НЕ генеруй "свою інтерпретацію Bento"
Bento тут — це не загальне поняття. Це **конкретний CSS** з `reference/template/index.html` і **конкретні патерни** з `reference/slide-recipes.md`. ОБОВʼЯЗКОВО:

1. Прочитай `reference/template/index.html` повністю (~1400 рядків)
2. Прочитай `reference/slide-recipes.md`
3. Прочитай `reference/design-system.md`
4. Прочитай `reference/company-info.md`
5. **Скопіюй template/index.html** як основу і заміни тільки текст/контент
6. **НЕ переписуй CSS**, не міняй класи `.slide`, `.qr-card`, `.bento-card` тощо
7. **НЕ генеруй HTML "з нуля"** — це гарантовано буде не Bento-стиль

### ❌ НЕ пропускай онбоардинг

При активації скіла **обовʼязково** виконай у такому порядку:

1. **Прочитай `reference/template/index.html` + reference/*.md** (ОБОВʼЯЗКОВО, інакше не зрозумієш дизайн)
2. **Перевір `user-config.md`** (P1.1-P1.3)
3. **Задай питання спікеру через AskUserQuestion** (P1.4-P1.5):
   - Мову (якщо нема `default_language`)
   - Формат HTML/PDF — за замовчуванням HTML (PPTX не пропонуй якщо спікер сам не попросив)
   - Хто спікер: обрати з існуючих або додати нового
   - Якщо новий — імʼя, посада, email, телефон, LinkedIn, Telegram, photo URL, біо
   - Тему презентації + аудиторію + контекст + кількість слайдів + CRMiUM intro?
   - QR-коди: обидва / тільки питання / тільки Telegram / без QR
   - Драфт/тези презентації або генерувати каркас?
4. **Лише ПІСЛЯ ВСІХ відповідей** копіюй template і починай заміну контенту.

⚠️ **НЕ ВИГАДУЙ контент без онбоардингу.** НЕ генеруй презентацію з placeholder-ами якщо спікер не дав відповіді. НЕ скорочуй онбоардинг навіть якщо повідомлення спікера здається "достатньо інформативним".

⚠️ **Якщо повідомлення спікера лише трігер** — стартуй онбоардинг з нуля.

---

## ✅ ЯК ПРАВИЛЬНО (швидкий чек)

1. ✅ Output: `projects/<event>-<speaker>/index.html` — single-file HTML
2. ✅ Кольори: рівно 3 (`#15171C` / `#EF652F` / `#FFFFFF`)
3. ✅ Шрифти: Onest (display+body) + JetBrains Mono (eyebrow)
4. ✅ Layout: copy template/index.html → змінено тільки текст
5. ✅ Слайди: 16:9, full viewport, transform-translateX навігація
6. ✅ Watermark CRMiUM на section/qa слайдах
7. ✅ QR коди (за вибором спікера у онбоардингу)
8. ✅ Спікер питаний через AskUserQuestion ДО генерації

---

## Workflow

### P1.1 — Прочитай збережений user-config

Шукай файл `~/.claude/skills/crmium-presentation-bento/user-config.md`. У ньому **список усіх збережених спікерів** (компанії, які вже робили презентації). Структура:

```yaml
default_language: ua
default_format: html

speakers:
  - id: oleksandr-melnyk
    name: "Олександр Мельник"
    role: "Solutions Architect"
    email: "o.melnyk@crmium.com"
    phone: "+380 50 123 45 67"
    linkedin: "https://linkedin.com/in/melnyk-o"
    telegram: "@melnyk_o"
    photo_url: "https://crmium.com/wp-content/uploads/2026/05/oleksandr-melnyk.jpg"
    bio: "12 років у CRM/ERP. 40+ впроваджень Zoho."

  - id: maria-koval
    name: "Марія Коваль"
    role: "Senior CRM Consultant"
    email: "m.koval@crmium.com"
    ...

last_used_speaker: oleksandr-melnyk
```

**Якщо файл не існує** — це перший запуск. Створи порожній файл з YAML-структурою (`speakers: []`) і йди до P1.2 (onboarding нового спікера).

**Якщо файл існує** — переходь до P1.2 (вибір/додавання спікера).

### P1.2 — Хто спікер?

#### Сценарій A: користувач НЕ вказав явно імʼя у запиті

Через `AskUserQuestion` запитай: **"Для кого ця презентація?"**

Опції:
- `last_used_speaker` (зі позначкою "(минулого разу)") — recommended, як першу опцію
- Решта спікерів з `speakers` array — кожен як окрема опція (`name · role`)
- **"+ Додати нового спікера"** — як останню опцію
- "Other" (вільний ввід — введи імʼя)

#### Сценарій B: користувач явно сказав імʼя ("зроби презентацію для Олександра")

Перевір чи цей `name` є у `speakers` array (по `id` слугу або фуззі-match по name). Якщо є — використай. Якщо ні — спитай "Не знайшов Олександра у збережених. Додати нового спікера з імʼям 'Олександр'?" → перейди до P1.3.

#### Якщо вибрали ІСНУЮЧОГО спікера

Підвантаж його контакти з `speakers[].fields` → пропусти P1.3 → перейди до P1.4.

#### Якщо вибрали "+ Додати нового спікера" або імʼя не знайдено

Перейди до P1.3 (onboarding нового спікера).

### P1.3 — Onboarding нового спікера

Через `AskUserQuestion` (або вільним вводом) збери:

1. **Імʼя та прізвище** (наприклад "Олександр Мельник")
2. **Посада** (наприклад "Solutions Architect", "Senior Zoho Consultant")
3. **Email** (зазвичай `name.surname@crmium.com`)
4. **Телефон** (опційно — можна пропустити)
5. **LinkedIn URL** (опційно)
6. **Telegram handle** (опційно)
7. **Photo URL** (опційно — якщо є з CRMiUM CDN: `https://crmium.com/wp-content/uploads/.../name.jpg`)
8. **Bio** (2-3 речення — Claude може запропонувати на основі посади, спікер коригує)

Згенеруй `id` як kebab-case імені (наприклад `oleksandr-melnyk`).

**Додай нового спікера до `speakers` array у user-config.md** і встанови його як `last_used_speaker`. **ЗБЕРЕЖИ ФАЙЛ** — він має оновитися ОДРАЗУ ПІСЛЯ збору контактів (щоб у разі вильоту скіла нові дані не пропали).

### P1.4 — Мова + формат презентації

Якщо у `user-config.md` є `default_language` і `default_format` — використай їх без питань.

Якщо їх НЕМА (перший запуск) — питай:

#### Мова презентації
- ua (українська) — recommended
- en (English)
- pl (polski)
- ru (русский)

#### Формат вихідного файлу
- **HTML** (recommended) — single-file. F11 fullscreen, стрілки ←→.
- **HTML + PDF instructions** — те саме + у README як зберегти PDF через "Print → Save as PDF".
- **PPTX (PowerPoint)** — обмежений у дизайні (анімації/градієнти не зберігаються). Запропонуй тільки якщо спікер явно просить.

Збережи у `user-config.md` як `default_language` і `default_format`.

### P1.5 — Контент конкретної презентації

Через `AskUserQuestion` або вільним вводом збери:

1. **Назва теми презентації** (наприклад "Як ми мігрували клієнта з Bitrix24 на Zoho")
2. **Аудиторія** — для кого, рівень обізнаності (sales-керівник, CTO, малий бізнес, спікери Zoho Summit)
3. **Подія/контекст** (Zoho Summit / webinar / sales demo / внутрішня)
4. **Орієнтовна кількість контентних слайдів** (5 / 8 / 12 / 18 / 25) — без cover/speaker/intro/Q&A/contact (це базові, додаються автоматично)
5. **CRMiUM intro слайд?** (так — для зовнішньої аудиторії; ні — для внутрішньої)
6. **QR-коди на слайди** — критично, питай ОБОВʼЯЗКОВО через `AskUserQuestion` (multiSelect: true):

   **Питання:** "Які QR-коди додати у презентацію?"

   - ✅ **QR "Задати питання"** на Q&A слайді — аудиторія сканує і пише питання у форму. **Усі питання попадають у CRM як новий лід** з контактами того хто запитав. Спікер бачить питання на ноутбуці і відповідає вголос.
   - ✅ **QR "Підписатися на Telegram-бот"** на окремому слайді перед контактами — аудиторія підписується, маркетинг-команда CRMiUM кидає у бот корисні матеріали по темі виступу (посилання, кейси, чек-листи).
   - ❌ **Без QR** — не додавати жодного.

   Дефолт — обидва увімкнені. Спікер може обрати лише один або жодного.

   Якщо після генерації спікер хоче прибрати конкретний QR з готової презентації — він пише: **"Видали QR зі слайду Q&A"** або **"Видали Resources-слайд з Telegram QR"**.

7. **Драфт презентації** — критично важливе питання:
   - "У тебе є готовий текст / тези / outline презентації?"
   - Якщо так — спікер вкладає текст у запит або файлом. Claude переоформлює у слайди.
   - Якщо ні — Claude генерує draft на основі теми + аудиторії. **Але обовʼязково попереджає** що "буде драфт, який ти потім має переглянути і відредагувати — я не маю реальних кейсів, цифр, прикладів які знаєш ти".

### P1.6 — Прочитай довідкові матеріали ДО генерації

ОБОВ'ЯЗКОВО прочитай:

- `reference/design-system.md` — палітра, шрифти, типографія (з новими розмірами!), layout, анімації, watermark variants
- `reference/slide-recipes.md` — 11+ типів слайдів з HTML+CSS патернами (з оновленими розмірами і блок-стилями)
- `reference/company-info.md` — CRMiUM elevator pitch, продукти, цифри
- `reference/language-templates.md` — стандартні фрази обраною мовою
- `reference/template/index.html` — повний робочий приклад на 11 слайдів

### P1.7 — Визнач робочу директорію

Створи: `projects/<event-slug>-<speaker-slug>/`. Приклади:
- `projects/zoho-summit-2026-melnyk/`
- `projects/webinar-odoo-manufacturing-koval/`

Структура:
```
projects/<slug>/
├── index.html               ← презентація (single-file inline CSS+JS)
├── assets/                  ← локальні картинки — копіюються з template/
│   ├── crmium-logo-white.png        ← логотип (44px у topbar)
│   ├── crmium-outline.png           ← watermark (контурний CRMiUM на section/qa слайдах)
│   ├── crmium-ask-question-qr.png   ← QR на форму "задай питання" — для Q&A слайда
│   └── crmium-ua-bot-qr.png         ← QR на Telegram-бот — для Resources слайда
└── README.md                ← як презентувати + експорт у PDF
```

### P1.8 — Згенеруй слайди

**Базові слайди (універсальні, мовою з user-config):**

1. **Cover (slide--cover)** — eyebrow (дата + подія) → ВЕЛИКИЙ h1 з emphasis (`<em>` помаранчевим) → subtitle → ім'я спікера + посада знизу. Watermark CRMiUM-outline на фоні.

2. **Speaker intro (slide--speaker)** — фото 1/1 (з photo_url або placeholder) → eyebrow "СПІКЕР" → велике імʼя → посада помаранчевим → 2-3 рядки біо.

3. **CRMiUM intro (slide--stats)** — тільки якщо `crmium_intro_slide = true`. Eyebrow → h2 → 3 цифри (500+/80+/10+) → caption про стек.

4. **Agenda (slide--bullets)** — eyebrow "ПЛАН" → h2 "Що ми обговоримо" → 3-7 пунктів з помаранчевими маркерами.

5. **N контентних слайдів** — Claude генерує на основі теми + драфту, користуючись `slide-recipes.md`. Чергуй типи (НЕ 5 bullets підряд):
   - **Section divider (slide--section)** — між великими блоками. Gradient orange→dark + watermark CRMiUM. Можна `slide-watermark--top` для варіативності.
   - **Pains-grid (slide--pains)** — 3 картки болів з em-помаранчевим у заголовках
   - **Bento (slide--bento)** — mixed 2fr/1fr/1fr grid з 1 big card + 3-4 малими (одна accent помаранчева)
   - **Features (slide--features)** — 4 cards у row з 1 accent (span 2)
   - **Bullets (slide--bullets)** — для текстових пунктів
   - **Stats (slide--stats)** — для цифр-метрик з counter-анімацією
   - **Quote (slide--quote)** — для customer stories (велика italic + author)
   - **Compare (slide--compare)** — до/після, A/B
   - **Timeline (slide--timeline)** — process steps

6. **Q&A (slide--qa)** — gradient + watermark + 2-колонний layout:
   - **Зліва**: великий помаранчевий "Питання?" (мовою з config) + subtitle про QR
   - **Справа**: біла QR-картка `assets/crmium-ask-question-qr.png` з підписом "Сканнуй щоб задати питання у формі"
   - Якщо аудиторія не встигла задати питання вживу або хоче персональну відповідь поза рамками події — пише через форму. Усі питання попадають у CRM як лід — спікер/відділ продажів повертається з персональною відповіддю

7. **Resources (slide--resources)** — Telegram-бот для корисних матеріалів:
   - Eyebrow "КОРИСНІ МАТЕРІАЛИ"
   - H2 "Хочеш ще більше?"
   - Текст про бот + 3-4 пункти що буде кидатися (посилання з презентації, кейси, чек-листи, дайджест)
   - Праворуч біла QR-картка `assets/crmium-ua-bot-qr.png`
   - **Спікер після виступу** кидає у цей бот посилання які згадував у презентації — підписники отримають їх автоматично

8. **Contact (slide--contact)** — eyebrow "КОНТАКТИ" → велике імʼя → посада → 4 contact-картки (email/phone/linkedin/site) з контактів спікера → логотип CRMiUM знизу.

### Опційно: чи включати QR-слайди (керується через P1.5 п.6)

Спікер обирає у P1.5 п.6:
- **Обидва QR** (дефолт) → і QR на Q&A слайді, і Resources-слайд з Telegram QR
- **Тільки QR "Задати питання"** → Q&A з QR, БЕЗ Resources-слайда (видали його повністю)
- **Тільки QR "Telegram"** → стандартний Q&A без QR (зніми QR-картку, лиши тільки великий "Питання?"), Resources-слайд з QR
- **Без QR** → стандартний Q&A без QR, БЕЗ Resources-слайда

**Якщо спікер пише ПІСЛЯ генерації** "Видали QR зі слайду питань" / "Видали Resources-слайд" / "Видали Telegram QR" — відкрий згенерований `index.html` і:
- Для QR на Q&A: видали `<div class="qr-card ...">...</div>` блок зі `<section class="slide slide--qa">`. Контейнер `.qa-grid` стане 1-колонним автоматично.
- Для Resources-слайда: видали повністю `<section class="slide slide--resources" ...>`. Перенумеруй наступні `data-slide` і оновлюй `.counter-total` у hero.

**Якщо спікер пише ПІСЛЯ генерації** "Додай QR на бота до слайду 5" / "Додай QR з формою питань до слайду 7" / "Додай QR на якомусь слайді про X" — використовуй ОДИН з готових патернів з `slide-recipes.md` §15-16, **НЕ вигадуй новий стиль QR-картки**:

1. **Який QR файл використати** залежно від запиту:
   - "QR на бота" / "QR на Telegram" / "QR підписки" / "QR на матеріали" → `assets/crmium-ua-bot-qr.png`
   - "QR на питання" / "QR форма" / "QR для запитань" / "QR на лід" → `assets/crmium-ask-question-qr.png`

2. **Який варіант компонента — ЗАВЖДИ `.qr-card` без модифікаторів** (без підложки):
   - 🚨 **Дефолт = bare** (без підложки). На всіх стандартних темних слайдах CRMiUM QR-картинки і так чудово видно — біла QR + помаранчеві кутики на темному фоні. **НЕ додавай темну підкладку, якщо слайд темний — це візуальний шум**.
   - `.qr-card .qr-card--dark-backdrop` — **ВИНЯТКОВО** для слайдів зі СВІТЛИМ фоном (це не дефолт). У дефолтному скілі такого слайда немає взагалі — тож **майже ніколи не використовуй цей модифікатор**.

3. **Перевір фон слайда** перед розміщенням (правило Принцип #0):
   - Темний фон (`#15171C` або dark gradient) → `.qr-card` БЕЗ модифікаторів
   - Світлий фон (`#FFFFFF`/кремовий) → `.qr-card .qr-card--dark-backdrop` (винятковий випадок)

4. **Структура HTML** для додавання QR на ЛЮБИЙ слайд (99% випадків — bare):
   ```html
   <!-- ДЕФОЛТ — bare, без підложки. ВИКОРИСТОВУЙ ЦЕ -->
   <div class="qr-card reveal">
     <img src="assets/crmium-ua-bot-qr.png" alt="QR — Telegram бот CRMiUM">
     <div class="qr-card-sublabel">TELEGRAM</div>
     <div class="qr-card-label">Сканнуй щоб підписатися</div>
   </div>
   ```

   ⚠️ **Варіант з підкладкою `--dark-backdrop` ТІЛЬКИ для слайдів зі СВІТЛИМ ФОНОМ** (винятково):
   ```html
   <!-- НЕ ВИКОРИСТОВУЙ на темному слайді -->
   <div class="qr-card qr-card--dark-backdrop reveal">
     <img src="assets/crmium-ask-question-qr.png" alt="QR — форма для питань">
     <div class="qr-card-sublabel">ЗАДАТИ ПИТАННЯ</div>
     <div class="qr-card-label">Сканнуй щоб написати питання у формі</div>
   </div>
   ```

5. **Куди вставляти** залежно від layout слайда:
   - У 1-колонний слайд (bullets, stats, quote) — зміни на 2-колонну grid (`grid-template-columns: 1fr auto; gap: 64px; align-items: center;`). Зліва — поточний контент, справа — QR.
   - У вже 2-колонний (як features-grid) — заміни одну з карток на QR-card або додай ще одну колонку.
   - У section divider — поряд з headline (як на Q&A слайді).

6. **Sublabel і label** підставляй залежно від QR і мови спікера (з `user-config.md → default_language`). Не вигадуй власні підписи — використовуй з шаблону вище або з `slide-recipes.md`.

7. **Дзвоник для спікера** після додавання: "Додав QR на слайд X. Перевір — QR має бути читабельний з відстані 2-3 метри. Якщо ні — збільш розмір картки".

⚠️ **Інформаційне правило для спікера** — обовʼязково озвучуй це коли пропонуєш QR (як на онбоардингу, так і коли додаєш на додатковий слайд):
> "QR 'Задати питання' дає тобі додаткове джерело лідів — усі питання з форми попадають у CRMiUM CRM як новий лід з контактами того хто запитав. Telegram-бот — для контентної доставки."

### P1.9 — Перевір у Preview (якщо є MCP)

Якщо в системі є MCP Claude Preview і `.claude/launch.json`:
1. Додай entry порт 8003+ (`--directory projects/<slug>`)
2. `preview_start` → `preview_resize 1920×1080`
3. Screenshot перших 3-х слайдів + останнього
4. `preview_console_logs` — перевірити 0 помилок

### P1.10 — Презентуй результат

Дай користувачу:
- Шлях до `index.html`
- Інструкції:
  - "Відкрий `index.html` у Chrome/Edge → F11 для fullscreen → стрілки ←→ для навігації"
  - "Для PDF: Ctrl+P → Save as PDF → Landscape → A4 → Background graphics: ON"

---

## Phase 2 workflow — наступні запуски

При **другому+** запуску:
1. Прочитай `user-config.md` → знайди список спікерів
2. P1.2 → запитай якого спікера обрати АБО додати нового
3. P1.5 → запитай тему/драфт нової презентації
4. Згенеруй у нову папку

Користувач може мати десятки збережених спікерів і десятки папок із презентаціями.

---

## Принципи

0. **QR-коди — ДЕФОЛТ `.qr-card` БЕЗ модифікаторів (без підкладки).** PNG-файли (`crmium-ask-question-qr.png`, `crmium-ua-bot-qr.png`) — білі pixel-и + помаранчеві куточки на прозорому фоні. На дефолтних темних слайдах CRMiUM (`#15171C`) QR видно прямо без жодної картки-обгортки. **НЕ додавай темну підкладку на темному слайді** — це непотрібний візуальний шум. Темна підкладка (`qr-card--dark-backdrop`) призначена ЛИШЕ для слайдів зі СВІТЛИМ фоном (винятковий випадок, у дефолтному скілі такого нема).

1. **3 кольори, без винятків**: `#15171C` (темний), `#EF652F` (помаранч), `#FFFFFF` (білий). Жодних додаткових.
2. **Текст на помаранчевому = БІЛИЙ**. Завжди.
3. **Не вигадуй контент** — без драфту попередь спікера що генеруєш каркас і він має наповнити реальними фактами/цифрами/кейсами.
4. **CRMiUM intro слайд — опційний**. Питай явно у P1.5.
5. **Контакти спікера — тільки з user-config**. Не вигадуй email/телефон.
6. **HTML — single-file**. CSS і JS інлайнені у `index.html`. Картинки (logo, watermark) — в `assets/` поряд. Без зовнішніх залежностей крім Google Fonts.
7. **Зберігай спікерів** — після onboarding нового **обовʼязково** збережи у `user-config.md` як новий запис у `speakers[]`. Це КРИТИЧНО — інакше наступного разу спікер вводитиме свої дані знову.
8. **Без mobile-first** — презентації для desktop / projector. Слайди фіксовані 16:9.
9. **Шрифти помірно-великі** — мінімум 19px для body (для читання з останнього ряду залу). Заголовки 60-200px.
10. **Кожна нова презентація = нова папка** в `projects/`. Не змішувати з іншими.

---

## Дизайн-чек-лист для кожного слайда

- [ ] Шрифт ≥ 19px для body, ≥ 38px для h3-карток, ≥ 80px для h1-headlines
- [ ] Section divider — gradient (НЕ плоский помаранч) + watermark CRMiUM
- [ ] На section/qa слайдах — top bar автоматично прихований (через `body.no-bar`)
- [ ] Cover має watermark + великий h1 (мінімум 100px)
- [ ] Pain/feature/bento картки — padding мінімум 36px, gap між картками 20-24px
- [ ] Hover на картках — `translateY(-4px)` + border помаранч + shadow
- [ ] Текст на помаранчевих accent-картках — білий

---

## Структура папки результату

```
projects/<event-slug>-<speaker-slug>/
├── index.html               ← готова презентація
├── assets/
│   ├── crmium-logo-white.png
│   └── crmium-outline.png
└── README.md                ← інструкція спікеру
```

Одна папка = одна презентація. Спікер може мати їх десятки.

---

## Що НЕ робить скіл

- ❌ Не генерує PPTX напряму (для PPTX — окремий скіл `anthropic-skills:pptx`)
- ❌ Не редагує існуючу презентацію через UI (спікер відкриває HTML і коригує текст)
- ❌ Не зберігає історію всіх презентацій — лише список спікерів
- ❌ Не вигадує контент для конкретного кейсу — без драфту видає тільки структуру з placeholder-текстом
