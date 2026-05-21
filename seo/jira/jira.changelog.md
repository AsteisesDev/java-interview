# Changelog: Разработка для Jira
Дата: 2026-05-13
Вопросов в теме: 27
Обработано: 27 из 27

## Ревизия v2 (аудит + редактура)

### Общие изменения
- [заголовки] Очищены от лишнего форматирования (убраны `**`, backticks)
- [грейды] Добавлены `<!-- grade: junior/middle/senior -->` ко всем 27 вопросам
- [структура] Добавлены `> **На собеседовании:**` блоки ко всем вопросам
- [структура] Добавлены `---` разделители между вопросами
- [структура] Длинные примеры кода обёрнуты в `<details>`
- [структура] Сравнения переведены в табличный формат
- [контент] Добавлены аналогии из жизни для ключевых концепций
- [якоря] Первое предложение каждого ответа — определение

### Распределение грейдов
- **Junior (5):** Платформы Jira, способы расширения, REST API, JQL, ScriptRunner
- **Middle (16):** Webhooks, Spring интеграция, Atlassian SDK, atlassian-plugin.xml, Spring в DC, Active Objects, REST-эндпоинт DC, Servlet Module, Event Listener, тестирование, отладка, Forge, Connect, Forge Storage/Custom UI, события Cloud, лимиты Cloud, best practices
- **Senior (6):** Custom Field Type, Workflow расширения, кластеризация DC, аутентификация Cloud, dual-platform проектирование

### Фактические ошибки
- Не обнаружено

### Таблицы добавлены/сохранены
- Платформы DC vs Cloud (характеристики)
- Способы расширения Jira (5 способов)
- Методы аутентификации (DC vs Cloud)
- Spring Scanner 2.x vs 1.x
- Spring Boot vs Jira Plugin (Spring Scanner)
- Типы модулей atlassian-plugin.xml
- Чек-лист кластерной совместимости
- Forge vs Connect сравнение
- Типы Forge Storage
- Обработка событий DC vs Cloud
- Rate Limiting и пагинация
- Forge-специфичные лимиты
- ScriptRunner vs кастомный плагин

### Аналогии добавлены
- Платформы Jira = типы жилья (дом, квартира, снесённый дом)
- JQL = поисковая строка Google для задач
- Webhook = SMS-уведомление от банка
- Active Objects = встроенный шкаф в квартире
- Forge = квартира-студия с мебелью от застройщика
- ScriptRunner = швейцарский нож для администратора

### SEO
- Batch 1: 14 вопросов (jira-batch1.seo.json)
- Batch 2: 13 вопросов (jira-batch2.seo.json)
- Новых тегов: 27 (jira, jira-platform, jira-data-center, jira-cloud, jira-plugin, jira-forge, jira-connect, jql, atlassian-sdk, active-objects, jax-rs, velocity, custom-field, jira-workflow, event-listener, clustering, debugging, serverless, typescript, integration, dependency-injection, servlet, workflow, groovy, scriptrunner, jira-authentication)
