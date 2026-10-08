# corp-skills

Коллекция навыков в формате Agent Skills, совместимая с `npx skills` и OpenCode.

## Доступные навыки

| Навык | Назначение |
| --- | --- |
| [opencode-skill-author](skills/opencode-skill-author/SKILL.md) | Создание и адаптация навыков для OpenCode с Kimi-K2.6 или DeepSeek-V4.1-Flash. |
| [opencode-orchestration](skills/opencode-orchestration/SKILL.md) | Оркестрация Python/ML-задач с Kimi-K2.6, DeepSeek-V4.1-Flash и Qwen3-Coder-Next: распределение работы, управление контекстом и проверка результатов. |

## Просмотр и установка

Нужны Node.js с `npx`, Git и доступ к этому приватному репозиторию через GitHub SSH.
Проверить список навыков без установки:

```bash
npx --yes skills@1.7.1 add git@github.com:BuckanovNikita/corp-skills.git --list
```

Установить выбранный навык для OpenCode в текущий проект:

```bash
npx --yes skills@1.7.1 add git@github.com:BuckanovNikita/corp-skills.git \
  --skill opencode-skill-author --agent opencode
```

Для глобальной установки добавьте `--global`. Установка навыка не настраивает
провайдера, модель или права инструментов OpenCode.
Для навыка оркестрации замените имя после `--skill` на `opencode-orchestration`.
Примеры агентов в его каталоге `examples/agents/` не активируются автоматически:
перед отдельным подключением укажите доступные идентификаторы моделей и проверьте
[совместимость конфигурации](skills/opencode-orchestration/references/opencode-setup.md).

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
DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list
npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
```

Если репозиторий недоступен, проверьте права GitHub и SSH-аутентификацию. Если
навык не появляется в OpenCode, проверьте каталог установки и разрешения
`permission.skill` в конфигурации OpenCode.

## Версии

Коллекция версионируется целиком по Semantic Versioning. Версия содержимого указана
в [VERSION](VERSION), изменения — в [CHANGELOG.md](CHANGELOG.md). Опубликованные
версии отмечаются аннотированными Git-тегами `vMAJOR.MINOR.PATCH`; пакеты и файлы
релизов не публикуются. Для воспроизводимой копии используйте checkout нужного тега.
Порядок проверки и выпуска описан в [RELEASE.md](RELEASE.md).
