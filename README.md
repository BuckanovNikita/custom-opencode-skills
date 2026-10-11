# corp-skills

Коллекция навыков в формате Agent Skills, совместимая с `npx skills` и OpenCode.

## Доступные навыки

| Навык | Назначение |
| --- | --- |
| [opencode-skill-author](skills/opencode-skill-author/SKILL.md) | Создание и адаптация навыков для OpenCode: по умолчанию DeepSeek-V4.1-Flash, с сохранением совместимости с Kimi-K2.6. |
| [opencode-orchestration](skills/opencode-orchestration/SKILL.md) | Планирование и оркестрация Python/ML-задач: DeepSeek-V4.1-Flash по умолчанию для всех ролей, Kimi-K2.6 и Qwen3-Coder-Next как явно выбранные альтернативы. |

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

По умолчанию выберите **DeepSeek-V4.1-Flash**, включите thinking и максимальный
поддерживаемый адаптером уровень рассуждения (`max` в официальном API).
Используйте эту модель для планирования, реализации, исследования, проверки и
создания навыков. Цена и расход токенов не служат основанием для снижения качества.
Это предварительная рекомендация по опубликованным результатам, а не доказанная
победа во всех задачах: [сравнение и ограничения](skills/opencode-orchestration/references/model-selection-2026-10-11.md).

Для начала с планирования выберите встроенный агент **Plan** и задайте
`default_agent: "plan"`. Модель и режим агента настраиваются отдельно.
[Неактивный пример конфигурации](skills/opencode-orchestration/examples/opencode.plan.json)
показывает выбор модели и стартового агента Plan; перед подключением замените
идентификаторы-заглушки и проверьте существующие настройки.
[Порядок перехода к реализации](skills/opencode-orchestration/references/opencode-setup.md#start-in-plan)
сохраняет выбранную модель и разрешает запись только после выхода из Plan и
авторизации выполнения. Навык не переключает режим и не устанавливает конфигурацию.

После перехода к разрешённому выполнению можно попросить:

```text
Use opencode-skill-author to create a standalone skill that reviews CSV quality
against a supplied schema and writes a Markdown report. Do not install it.
```

Рекомендации для моделей и сценарии проверки находятся в каталоге навыка.
Границы первоначальных проверок описаны в
[отчёте](skills/opencode-skill-author/evaluation-2026-10-08.md): совместимость
упаковки не означает проверку поведения на реальных целевых моделях.
Проверки обновлённого выбора моделей и запуска с Plan описаны в
[отчёте от 2026-10-11](verification/2026-10-11-model-selection.md).

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
