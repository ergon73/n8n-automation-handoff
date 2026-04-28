# Интеграции и зависимости

## Используемые сервисы

| Сервис | Для чего используется | Где хранится доступ |
|---|---|---|
| n8n Self Hosted | Исполнение workflow | Dokploy / n8n instance |
| ProxyAPI/OpenAI | Оценка бюджета и генерация короткого КП | n8n Credential `ProxyAPI Header Auth` |
| Bpium | CRM, хранение заявки и платежных данных | env `BPIUM_SESSION_COOKIE_TEST` или отдельный credential |
| YooKassa | Создание платежа | n8n HTTP Basic Auth credential |
| Telegram Bot API | Уведомление менеджера | n8n Telegram credential `Telegram Bot API` |
| Dokploy | Env-переменные и контейнер n8n | панель Dokploy |

## Где хранятся данные

| Данные | Место хранения |
|---|---|
| Контакты клиента | Bpium CRM |
| Текст заявки | Bpium CRM |
| Оценка бюджета | Bpium CRM |
| Короткое КП | Bpium CRM |
| ID платежа и ссылка оплаты | YooKassa и Bpium CRM |
| Execution logs | n8n Executions |
| Telegram-уведомление | Telegram-чат менеджера |

## Credentials

| Credential | Тип | Назначение |
|---|---|---|
| `ProxyAPI Header Auth` | HTTP Header Auth | Авторизация в ProxyAPI/OpenAI |
| `YooKassa Basic Auth` | HTTP Basic Auth | Авторизация в YooKassa |
| `Telegram Bot API` | Telegram API | Отправка уведомления менеджеру |

Секреты не должны передаваться в документации открытым текстом. При передаче проекта доступы нужно выдавать через n8n Credentials, менеджер паролей или отдельный защищенный канал.

## Env-переменные

| Переменная | Пример значения | Назначение |
|---|---|---|
| `NPR10_ENVIRONMENT` | `test` | Маркировка окружения в уведомлении |
| `NPR10_TELEGRAM_CHAT_ID_TEST` | test chat id | Чат для тестовых уведомлений |
| `BPIUM_BASE_URL_TEST` | `https://your-workspace.bpium.ru` | Базовый URL тестового пространства Bpium |
| `BPIUM_CATALOG_ID_TEST` | `17` | ID каталога Bpium для заказов |
| `BPIUM_SESSION_COOKIE_TEST` | `connect.sid=...` | Тестовая сессия Bpium для API |
| `YOO_KASSA_RETURN_URL` | `https://example.com/payment-return` | Return URL для платежа |
| `N8N_BLOCK_ENV_ACCESS_IN_NODE` | `false` | Разрешает чтение env в нодах n8n |

## Важное по Bpium

В санитизированном JSON реальный `connect.sid` удален и заменен на:

```text
={{ $env.BPIUM_SESSION_COOKIE_TEST }}
```

Перед запуском после импорта нужно добавить `BPIUM_BASE_URL_TEST`, `BPIUM_CATALOG_ID_TEST` и `BPIUM_SESSION_COOKIE_TEST` в окружение n8n или заменить ручной Cookie header на безопасный credential-подход.

## Зависимости и риски

| Зависимость | Что будет при сбое | Что проверить |
|---|---|---|
| ProxyAPI/OpenAI | Не будет оценки бюджета и КП | credential, баланс, лимиты, статус API |
| Bpium | Заказ не создастся или не обновится | base URL, catalog id, cookie/credential, доступность API |
| YooKassa | Платеж не создастся | credential, тестовый магазин, тело запроса |
| Telegram | Менеджер не получит уведомление | credential, chat id, доступность Telegram API |
| n8n env | Не подставятся chat id и cookie | Dokploy env, compose `environment`, redeploy |

## Что нужно для production

Для боевого запуска нельзя использовать тестовый workflow без перенастройки:

| Нужно заменить | Production-вариант |
|---|---|
| `[TEST]` workflow | отдельный `[PROD]` workflow |
| `ProxyAPI Header Auth` | production credential |
| `NPR10_TELEGRAM_CHAT_ID_TEST` | production chat id |
| `BPIUM_SESSION_COOKIE_TEST` | production Bpium access |
| YooKassa test credential | production YooKassa credential |
| pinned demo data | отключить или заменить безопасными production-тестами |

