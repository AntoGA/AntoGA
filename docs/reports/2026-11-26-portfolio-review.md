# Обзор публичных репозиториев — 26 ноября 2026

Это инвентаризация публичного портфеля и целевая проверка четырёх проектов,
а не утверждение о полном аудите каждого файла. Закрытые репозитории сюда не включены.
Состояние предыдущих PR проверено через GitHub; ни один PR автоматически не слит.

## Исправления этого прохода

| Репозиторий | Исправление | Проверка / PR |
| --- | --- | --- |
| agentmind | Циклический импорт, MemoryConfig export, опасное удаление без metadata-фильтра, пустой ID-list, поиск и cleanup | 18 offline tests; [PR #1](https://github.com/AntoGA/agentmind/pull/1) |
| Ai-race | React cleanup возвращал Socket и ломал TypeScript build | local production build; [PR #1](https://github.com/AntoGA/Ai-race/pull/1) |
| legal-ai-chatbot | Некорректный URL вне error handling; добавлен CI | 10 tests; [PR #2](https://github.com/AntoGA/legal-ai-chatbot/pull/2) |
| static-ai-leadbot | Гонка дедупликации заявки, malformed URL; добавлен CI | 9 tests; [PR #2](https://github.com/AntoGA/static-ai-leadbot/pull/2) |
| AntoGA | Telegram-ссылка была относительным GitHub-путём @ayndx | Заменена на https://t.me/ayndx; личность владельца аккаунта отдельно не проверялась |

Отчёты с границами проверки находятся в каждом code PR:
`docs/reports/2026-11-26-audit.md`. Внешние AI/Telegram/embedding API не вызывались.
Данные тестов синтетические; платные запросы не выполнялись.

## Что ещё корректировать, по приоритету

1. **Завершить review существующих PR.** Vk #6, intake-agent #1,
   social-graph-friend-finder #3 и Ultrasound-AI-Assistant #1 остаются открыты.
   Зелёный CI ветки не означает, что main содержит её исправления.
   Для Vk определить судьбу пересекающихся PR #1/#2/#5/#6 до merge,
   чтобы не вернуть старые контракты поверх нового кода.
2. **agentmind packaging/dependencies.** Разделить optional backends и OpenAI
   dependencies на extras, проверить install из wheel; scalable/concurrent search
   и реальная embedding quality требуют отдельного протокола.
3. **Ai-race dependency/runtime audit.** Проверить актуальные advisories и lockfiles
   отдельно от build-fix; добавить тесты runtime chat/streaming/DB authorization.
   Текущий build success не подтверждает безопасность dependency stack.
4. **qwen-skills.** На main есть Python-примеры и requirements, но нет tests/CI.
   Следующий проход: offline imports, calculator/tool boundaries, проверка
   совместимости SDK и mocked provider responses. Содержимое всех примеров этим
   обзором не проверено, ошибки в них не объявляются без воспроизведения.
5. **langgraph-rag.** Main содержит только README; открытый PR #1 — описание
   архитектуры/structure check. Нужны runnable pipeline, fixtures, negative tests
   и evaluation protocol, прежде чем показывать как работающий RAG-кейс.
6. **Ultrasound-AI-Assistant.** Main содержит только LICENSE; README/status audit
   подготовлен в PR #1. Реализации для тестирования пока нет.
7. **Soup.** Около 1.5 тысяч файлов, workflows присутствуют; endpoint последних
   runs вернул пустой список. Это не доказательство исправности или неисправности.
   Нужен отдельный scope и подходящее CPU/GPU окружение; полный тестовый прогон
   в этом проходе не выполнялся.
8. **Документационные репозитории.** Open-Ai и Deppsekk-skills содержат похожие
   наборы guide-файлов; решить, какой является основным источником, затем сверить
   содержимое и ссылки. Midjourney имеет пустые example-каталоги .gitkeep —
   заполнить реальными примерами либо обозначить отсутствие примеров.
9. **agents-contract.** CI присутствует и последний проверенный run успешен.
   При изменении benchmark claims повторять bench согласно собственному AGENTS.md.
   В этом проходе benchmark не перезапускался и числа не менялись.

## Порядок продолжения

После review текущих исправлений: qwen-skills offline tests → Ai-race runtime и
dependencies → agentmind packaging → отдельный согласованный участок Soup.
Новые публичные заявления о production, скорости или качестве должны опираться
на сохранённые результаты, а не на наличие README или зелёный syntax check.
