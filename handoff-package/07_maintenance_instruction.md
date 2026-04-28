# Инструкция для сопровождения

## Где смотреть ошибки

Основное место диагностики: n8n → workflow `[TEST] NPr10 - CRM Orders Payments - Safe` → вкладка `Executions`.

Порядок проверки:

1. Открыть последний failed execution.
2. Найти первую красную ноду.
3. Открыть `Input`, `Output`, `Error details`.
4. Сравнить входные данные с ожидаемыми полями.
5. Исправить credential/env/API-проблему или повторить запуск после устранения причины.

## Критичные ноды

| Нода | Почему критична |
|---|---|
| `On form submission` | если не работает, сценарий не стартует |
| `SEC_Minimize_For_AI` | защищает от передачи ПДн в AI |
| `AI Request` | без AI нет оценки бюджета и КП |
| `Parse AI` | если JSON не парсится, дальше нет `price` и `offer` |
| `Create Order In Bpium` | основная CRM-запись |
| `Create YooKassa Payment` | создание платежа |
| `Update Bpium Order Payment Link` | запись платежной ссылки обратно в CRM |
| `Send Telegram Message` | уведомление менеджера |

## Типовые ошибки и действия

| Ошибка | Где возникает | Что делать |
|---|---|---|
| `access to env vars denied` | env в выражениях | проверить `N8N_BLOCK_ENV_ACCESS_IN_NODE=false`, compose `environment`, redeploy |
| `401/403` | ProxyAPI, Bpium, YooKassa | проверить credential, срок действия токена/cookie, права доступа |
| `AI response text not found` | `Parse AI` | открыть output `AI Request`, проверить формат ответа и лимиты |
| `JSON.parse` error | `Parse AI` | проверить, вернула ли модель чистый JSON без markdown |
| `Bad Request: chat not found` | Telegram | проверить chat id, запущен ли бот пользователем, правильный ли Telegram credential |
| `payment_id` пустой | YooKassa | проверить успешность `Create YooKassa Payment` и body запроса |
| заказ не найден | `GET Check Bpium Order` | проверить ID заказа и доступ к Bpium API |

## Базовые правила изменений

| Правило | Почему |
|---|---|
| Сначала делать копию workflow | чтобы не ломать рабочую версию |
| Перед правкой экспортировать JSON | в Community/free версии нет надежного встроенного версионирования |
| Не хранить токены в нодах | секреты должны быть в Credentials или защищенной env-конфигурации |
| Не отправлять email/phone в AI и Telegram | принцип минимизации данных |
| После изменения env делать redeploy | простой restart может не подхватить compose/environment |
| После изменения workflow делать тестовый запуск | проверить всю цепочку до финальной ноды |
| Все изменения писать в журнал | новый специалист должен понимать, зачем была правка |

## Как безопасно менять сценарий

1. Экспортировать текущий JSON.
2. Сделать копию workflow с префиксом `[TEST]`.
3. Внести изменение.
4. Запустить тестовую заявку.
5. Проверить `Executions`.
6. Проверить Bpium, YooKassa и Telegram.
7. Записать изменение в `09_change_log.md`.
8. Только после этого переносить подход в production.

## Что не менять без отдельного согласования

| Элемент | Причина |
|---|---|
| поля формы | изменение сломает маппинг в Bpium |
| fieldId Bpium | API использует числовые поля, ошибка приведет к неправильной записи |
| формат JSON от AI | `Parse AI` ожидает `price` и `offer` |
| YooKassa metadata | нужна для связи платежа с заказом |
| Telegram message policy | уведомление специально не содержит ПДн |

