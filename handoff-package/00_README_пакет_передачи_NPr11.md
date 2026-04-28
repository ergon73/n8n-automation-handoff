# Пакет передачи автоматизации NPr11

## Что передается

Автоматизация: `[TEST] NPr10 - CRM Orders Payments - Safe`

Бизнес-сценарий: обработка заявки клиента, оценка бюджета через AI, создание заказа в Bpium CRM, создание платежа YooKassa и уведомление менеджера в Telegram.

## Состав пакета

| Файл | Назначение |
|---|---|
| `01_workflow_export_TEST_NPr10_CRM_Orders_Payments_Safe_sanitized.json` | Основной JSON-файл workflow для импорта в n8n |
| `02_workflow_screenshot.md` | Документ со скриншотом актуальной схемы workflow |
| `02_architecture_workflow_screenshot.png` | PNG-файл скриншота, используется в документах `02` и `04` |
| `03_passport_automation.md` | Паспорт автоматизации |
| `04_architecture_description.md` | Описание архитектуры и логики работы |
| `05_integrations_dependencies.md` | Интеграции, зависимости, credentials и env |
| `06_user_instruction.md` | Простая инструкция для пользователя |
| `07_maintenance_instruction.md` | Техническая инструкция сопровождения |
| `08_support_regulation.md` | Регламент поддержки и инцидентов |
| `09_change_log.md` | Журнал изменений |
| `10_submission_text.md` | Краткий текст для сдачи домашнего задания |

## Важное про JSON

Файл JSON санитизирован для передачи:

| Что удалено/заменено | Почему |
|---|---|
| Bpium `connect.sid` заменен на `$env.BPIUM_SESSION_COOKIE_TEST` | cookie является секретом и не должен передаваться открытым текстом |
| pinned demo data заменены на обезличенные тестовые значения | email и телефон не должны попадать в экспорт как персональные данные |
| реальные токены не включены в JSON | токены должны храниться в Credentials или env окружения |
| реальные URL окружения заменены на env-переменные или публичные примеры | публичный репозиторий не должен раскрывать внутреннюю инфраструктуру |

Перед запуском после импорта нужно настроить credentials и env, указанные в `05_integrations_dependencies.md`.

Для реальной передачи заказчику такой пакет лучше хранить в приватном репозитории и выдавать доступ индивидуально.

## Что приложить в ответе на платформе

Минимальный набор для сдачи:

| Тип | Файл |
|---|---|
| JSON workflow | `01_workflow_export_TEST_NPr10_CRM_Orders_Payments_Safe_sanitized.json` |
| Паспорт | `03_passport_automation.md` |
| Скриншот workflow | `02_workflow_screenshot.md` + `02_architecture_workflow_screenshot.png` |
| Архитектура | `04_architecture_description.md` |
| Интеграции | `05_integrations_dependencies.md` |
| Инструкция пользователя | `06_user_instruction.md` |
| Инструкция сопровождения | `07_maintenance_instruction.md` |
| Регламент поддержки | `08_support_regulation.md` |
| Журнал изменений | `09_change_log.md` |

Если платформа принимает только текст и скрины, можно вставить текст из `10_submission_text.md` и приложить остальные файлы как архив.

