# Установка скіла за 1 хвилину

## Швидкий старт — скопіюй у свій Claude Code

```
Склонуй мені https://github.com/crmiummarketing/crmium-presentation-skill у папку ~/.claude/skills/crmium-presentation-bento — це скіл для генерації фірмових презентацій CRMiUM у Bento-стилі. Перевір що скіл доступний у списку доступних скілів і поясни як його активувати.
```

Claude автоматично:
1. Склонує репозиторій у `~/.claude/skills/crmium-presentation-bento/`
2. Підтвердить що скіл встановлено
3. Скаже якщо треба перезапустити сесію (іноді треба `/clear` або новий чат)

---

## Перевірка що скіл працює

Після встановлення напиши Claude:

```
Зроби тестову презентацію на 3 слайди про впровадження Zoho для тестового клієнта
```

Claude має:
- Запитати мову, формат, контакти спікера (це onboarding — буде один раз)
- Згенерувати папку `projects/<slug>/index.html`
- Сказати як відкрити

Якщо запитав 4 onboarding-питання — скіл активовано правильно.

---

## Якщо щось пішло не так

### Скіл не знайдено
- Перевір що папка існує: `ls ~/.claude/skills/crmium-presentation-bento/` має повернути SKILL.md, README.md, reference/
- Перезапусти Claude Code повністю (закрий і відкрий заново)
- Напиши `/skills` у Claude Code — має зʼявитись `crmium-presentation-bento` у списку

### Git clone fails
- Перевір що ти підʼєднаний до інтернету
- Перевір що `git` встановлено: `git --version`
- Якщо за корпоративним проксі — clone може блокуватись, використай **Спосіб 3** (ZIP) з README.md

### Шрифти не вантажаться (Onest)
- Перевір інтернет (Google Fonts CDN)
- За корпоративним firewall Google Fonts може бути заблоковано — fallback на system-ui спрацьовує автоматично, виглядає трохи інакше але працює

---

## Альтернативні способи установки

### `git clone` вручну

```bash
# Windows (PowerShell)
git clone https://github.com/crmiummarketing/crmium-presentation-skill "$env:USERPROFILE\.claude\skills\crmium-presentation-bento"

# macOS / Linux
git clone https://github.com/crmiummarketing/crmium-presentation-skill ~/.claude/skills/crmium-presentation-bento
```

### Завантажити ZIP

1. https://github.com/crmiummarketing/crmium-presentation-skill → **Code** → **Download ZIP**
2. Розпакуй
3. Перейменуй папку на `crmium-presentation-bento`
4. Скопіюй у:
   - Windows: `C:\Users\<USER>\.claude\skills\`
   - macOS: `~/.claude/skills/`
   - Linux: `~/.claude/skills/`

---

## Як оновити скіл (якщо вийшов апдейт)

```bash
cd ~/.claude/skills/crmium-presentation-bento
git pull
```

Або через Claude:
```
Оновіть мій скіл crmium-presentation-bento — зроби git pull у ~/.claude/skills/crmium-presentation-bento
```
