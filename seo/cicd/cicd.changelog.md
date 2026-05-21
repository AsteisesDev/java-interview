# Changelog: CI/CD
Дата: 2026-05-09
Вопросов в теме: 28
Обработано: 28 из 28

## Ревизия v2 (аудит + редактура)

### Общие изменения
- [заголовки] Очищены от лишнего форматирования
- [грейды] Добавлены `<!-- grade: junior/middle/senior -->` ко всем 28 вопросам
- [структура] Добавлены `###` подзаголовки, `> **На собеседовании:**`, `---` разделители
- [структура] Длинные примеры кода обёрнуты в `<details>`
- [структура] Сравнения переведены в табличный формат
- [контент] Добавлены аналогии из жизни
- [объём] 1699 → 2652 строк

### Распределение грейдов
- **Junior (5):** CI, CD, Зачем CI/CD, Jenkins, Nexus (что такое), типы репозиториев, зачем Nexus, триггеры
- **Middle (20):** Этапы пайплайна, best practices, стратегии деплоя, архитектура Jenkins, Jenkinsfile, секции, параметры/условия/параллельные стадии, плагины, credentials, Docker+Jenkins, Multibranch, Maven+Nexus, Gradle+Nexus, Nexus Docker Registry, деплой артефактов, branching strategies, Merge vs Rebase
- **Senior (3):** Shared Libraries, CI/CD для банковских микросервисов, безопасность пайплайна

### Фактические ошибки
- [fix] Jenkins создаёт 3 переменные для username/password credentials, а не 2
- [fix] Добавлена заметка о поведении параметров при первом запуске Pipeline

### Таблицы добавлены/улучшены
- Стратегии деплоя (Rolling, Blue-Green, Canary, A/B, Recreate)
- Declarative vs Scripted Pipeline
- Типы подключения агентов Jenkins
- Типы credentials
- Альтернативы Jenkins
- Секции Jenkinsfile
- CI/CD антипаттерны
- Типы репозиториев Nexus
- Git Flow vs GitHub Flow vs Trunk-based
- Merge vs Rebase
- scp vs rsync
- screen vs tmux

### Аналогии добавлены
- CI = заводской конвейер
- CD = конвейер посылок
- Стратегии деплоя = замена двигателя самолёта
- Master/Agent = диспетчер такси
- Shared Libraries = шаблоны договоров
- Nexus = склад запчастей
- Branching strategies = дорожные развязки
