# User Config (example з множинним спікером)

Файл `~/.claude/skills/crmium-presentation-bento/user-config.md` зберігає **список усіх спікерів** які робили презентації + глобальні налаштування.

---

## Структура

```yaml
# Глобальні налаштування (запитуються один раз при першому запуску)
default_language: ua   # ua / en / pl / ru
default_format: html   # html / html-pdf / pptx

# Список збережених спікерів (додаються через onboarding кожного нового)
speakers:
  - id: oleksandr-melnyk
    name: "Олександр Мельник"
    role: "Solutions Architect"
    email: "o.melnyk@crmium.com"
    phone: "+380 50 123 45 67"
    linkedin: "https://linkedin.com/in/melnyk-o"
    telegram: "@melnyk_o"
    photo_url: "https://crmium.com/wp-content/uploads/2026/05/oleksandr-melnyk.jpg"
    bio: |
      12 років у CRM/ERP. Веду команду з 6 архітекторів.
      За плечима 40+ впроваджень Zoho для виробничих
      і дистрибʼюторських компаній.

  - id: maria-koval
    name: "Марія Коваль"
    role: "Senior CRM Consultant"
    email: "m.koval@crmium.com"
    phone: "+380 50 234 56 78"
    linkedin: "https://linkedin.com/in/maria-koval"
    telegram: ""
    photo_url: ""
    bio: |
      6 років консультант з CRM/ERP. Спеціалізація — Zoho для
      e-commerce та B2B-сервісу.

  - id: andriy-tkach
    name: "Андрій Ткач"
    role: "Marketing Director"
    email: "a.tkach@crmium.com"
    phone: ""
    linkedin: "https://linkedin.com/in/atkach"
    telegram: "@andriy_tkach"
    photo_url: "https://crmium.com/wp-content/uploads/2026/05/andriy-tkach.jpg"
    bio: |
      Веду маркетинг CRMiUM. 8 років у B2B-комунікаціях.
      Спікер на 30+ конференціях.

# Який спікер обирався минулого разу (для дефолту у dropdown)
last_used_speaker: oleksandr-melnyk
```

---

## Як це працює

### Перший запуск (файла нема)
1. Скіл створює порожній файл з `speakers: []`
2. Питає мову + формат (зберігає у `default_*`)
3. Питає онбоардинг першого спікера (P1.3 у SKILL.md)
4. Зберігає спікера у `speakers[]`, встановлює `last_used_speaker`

### Другий запуск (1 спікер у списку)
1. Скіл читає файл, бачить 1 спікера
2. Питає: "Презентація для **Олександр Мельник** (минулого разу) — продовжуємо чи додати нового спікера?"
3. Якщо "продовжуємо" — використовує його дані
4. Якщо "додати нового" — повторний P1.3, додає до списку

### Третій+ запуск (декілька спікерів)
1. Скіл показує **список** усіх спікерів через `AskUserQuestion`:
   - Олександр Мельник · Solutions Architect (минулого разу)
   - Марія Коваль · Senior CRM Consultant
   - Андрій Ткач · Marketing Director
   - **+ Додати нового спікера**
2. Спікер обирає → завантажуються його контакти
3. Якщо "+ Додати нового" → P1.3 → новий запис у `speakers[]`

---

## Як змінити вручну

Відкрий `user-config.md` у VS Code / Notepad. Структура — стандартний YAML.

### Змінити контакти існуючого спікера
Знайди блок `- id: <speaker-id>` → редагуй поля → збережи. Скіл прочитає нові дані при наступному запуску.

### Видалити спікера
Видали його блок з `speakers[]`. Якщо це був `last_used_speaker` — постав інший id у `last_used_speaker`.

### Скинути все
Видали файл. При наступному запуску скіл попросить онбоардинг знову.

### Експортувати/перенести між машинами
Скопіюй `user-config.md` на іншу машину у `~/.claude/skills/crmium-presentation-bento/`. Спікери разом з ним.

---

## Що НЕ зберігається у user-config

- **Тема презентації** — кожного разу нова, питається при кожному запуску
- **Кількість слайдів** — теж на кожен запуск
- **Контент / драфт слайдів** — спікер дає при кожному запуску
- **Логотип CRMiUM** — фіксований шлях `assets/crmium-logo-white.png`
- **Watermark** — фіксований шлях `assets/crmium-outline.png`

---

## Приклад порожнього стартового файлу (генерується при першому запуску)

```yaml
default_language: ""
default_format: ""
speakers: []
last_used_speaker: ""
```

Скіл заповнить ці поля під час онбоардингу.
