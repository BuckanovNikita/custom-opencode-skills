# corp-skills

Коллекция навыков в формате Agent Skills, совместимая с `npx skills` и OpenCode.

## Доступные навыки

| Навык | Назначение |
| --- | --- |
| [opencode-skill-author](skills/opencode-skill-author/SKILL.md) | Создание и адаптация навыков для OpenCode с Kimi-K2.6 или DeepSeek-V4.1-Flash. |

## Просмотр и установка

Нужны Node.js с `npx`, Git и доступ к этому приватному репозиторию через GitHub SSH.
Проверить список навыков без установки:

```bash
npx skills add git@github.com:BuckanovNikita/corp-skills.git --list
```

Установить выбранный навык для OpenCode в текущий проект:

```bash
npx skills add git@github.com:BuckanovNikita/corp-skills.git \
  --skill opencode-skill-author --agent opencode
```

Для глобальной установки добавьте `--global`. Установка навыка не настраивает
провайдера, модель или права инструментов OpenCode.

## Использование

В OpenCode выберите настроенную Kimi-K2.6 или DeepSeek-V4.1-Flash и попросите:

```text
Use opencode-skill-author to create a standalone skill that reviews CSV quality
against a supplied schema and writes a Markdown report. Do not install it.
```

Рекомендации для моделей и сценарии проверки находятся в каталоге навыка.
Границы выполненных проверок описаны в
[отчёте](skills/opencode-skill-author/evaluation-2026-10-08.md): совместимость
упаковки не означает проверку поведения на реальных целевых моделях.

## Проверка локальной копии

Из корня репозитория:

```bash
npx skills add . --list
npx markdownlint-cli2 README.md 'skills/**/*.md'
```

Если репозиторий недоступен, проверьте права GitHub и SSH-аутентификацию. Если
навык не появляется в OpenCode, проверьте каталог установки и разрешения
`permission.skill` в конфигурации OpenCode.
