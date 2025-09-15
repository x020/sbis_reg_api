# Партнерское API СБИС

Добро пожаловать в репозиторий документации по партнерскому API для интеграции с биллинговой системой СБИС (reg.tensor.ru). Это API позволяет партнерам Тензора автоматизировать работу с клиентами, лицензиями, госорганами и другими сервисами СБИС.

## Описание

Партнерское API предназначено для интеграции внешних CRM-систем с биллингом СБИС. Оно позволяет:
- Искать и создавать аккаунты клиентов.
- Добавлять и активировать лицензии.
- Работать с госорганами и кассами ОФД.
- Подключать клиентов к online.sbis.ru и СБИС 2.4.
- Синхронизировать данные с внешними системами.

API использует протокол JSON-RPC 2.0 поверх HTTPS. Текущая версия API: 1.3.6 (на основе истории изменений).

Обязательное условие: ведение учета лицензий в [reg.tensor.ru](https://reg.tensor.ru).

## Быстрый старт

### 1. Настройка взаимодействия
- Используйте **HTTPS** и **POST**-запросы.
- Эндпоинты:
  - Аутентификация: `https://reg.tensor.ru/auth/service/`
  - Основные методы: `https://reg.tensor.ru/partner_api/service/`
- Заголовки:
  - `Content-Type: application/json-rpc; charset=utf-8`
  - `Accept: application/json-rpc`
  - `X-SBISSessionID: <идентификатор_сессии>` (после аутентификации)

Подробности: [Настройки взаимодействия](partner-api-settings.md)

### 2. Аутентификация
- Вызовите метод `САП.Аутентифицировать` с логином и паролем.
- Получите идентификатор сессии и используйте его в заголовке `X-SBISSessionID`.
- Время жизни сессии: 1 сутки.

Пример:
```json
{
    "jsonrpc": "2.0",
    "method": "САП.Аутентифицировать",
    "params": {
        "login": "your_login",
        "password": "your_password"
    },
    "protocol": 2,
    "id": 0
}
```

Подробности: [Аутентификация](partner-api-authorization.md)

### 3. Формат запросов и ответов
- Протокол: JSON-RPC 2.0 с протоколом 2.
- Типы данных: строки, числа, даты (`ГГГГ-ММ-ДД`), деньги (с десятичной частью).
- Выборки данных: в формате `{ "s": [форматы], "d": [данные] }`.

Подробности: [Формат протокола JSON-RPC](partner-api-data.md)

## Документация по методам API

Все методы описаны в директории [API-metody-dlya-vzaimodeystviya](API-metody-dlya-vzaimodeystviya/API-metody-dlya-vzaimodeystviya.md).

### Основные разделы:
- **[Работа с клиентами](API-metody-dlya-vzaimodeystviya/Rabota-s-klientami/Rabota-s-klientami.md)**: Создание аккаунтов, поиск контрагентов, контактные данные.
- **[Закрепление клиентов](API-metody-dlya-vzaimodeystviya/Zakreplenie-klientov/Zakreplenie-klientov.md)**: Перепривязка аккаунтов и контрагентов.
- **[Работа с госорганами](API-metody-dlya-vzaimodeystviya/Rabota-s-gosorganami/Rabota-s-gosorganami.md)**: Подключение сертификатов, инспекций.
- **[Прайс действующих услуг](API-metody-dlya-vzaimodeystviya/Prais-deystvuyushchih-uslug/Prais-deystvuyushchih-uslug.md)**: Доступные сервисы.
- **[Работа с лицензиями](API-metody-dlya-vzaimodeystviya/Rabota-s-licenziyami/Rabota-s-licenziyami.md)**: Создание, активация, информация по лицензиям.
- **[Работа с кассами ОФД](API-metody-dlya-vzaimodeystviya/Rabota-s-kassami-OFD/Rabota-s-kassami-OFD.md)**: Привязка ККТ к лицензиям.
- **[Интеграция с внешними системами](API-metody-dlya-vzaimodeystviya/Integraciya-s-vneshnimi-sistemami/Integraciya-s-vneshnimi-sistemami.md)**: История изменений (Billing.GetHistory).
- **[Подключение к online.sbis.ru](API-metody-dlya-vzaimodeystviya/Podklyuchenie-k-online.sbis.ru/Podklyuchenie-k-online.sbis.ru.md)**: Подготовка кабинетов, приглашения.
- **[Подключение к СБИС 2](API-metody-dlya-vzaimodeystviya/Podklyuchenie-k-SBIS-2.md)**: Интеграция с СБИС 2.4.

Полный список методов: [API-metody-dlya-vzaimodeystviya](API-metody-dlya-vzaimodeystviya/API-metody-dlya-vzaimodeystviya.md)

## Примеры использования

Примеры реализации на:
- [Python](API-metody-dlya-vzaimodeystviya/Primery-ispolzovaniya-API/Primer-ispolzovaniya-partnerskogo-API-na-Python.md)
- [NodeJS](API-metody-dlya-vzaimodeystviya/Primery-ispolzovaniya-API/Primer-ispolzovaniya-partnerskogo-API-na-NodeJS.md)

Общий обзор примеров: [Примеры использования API](API-metody-dlya-vzaimodeystviya/Primery-ispolzovaniya-API/Primery-ispolzovaniya-API.md)

## Реферальные ссылки

Используйте реферальные ссылки для привлечения клиентов:
- Регистрация: `https://online.sbis.ru/auth/?tab=register&agent={КОД_ТОЧКИ_ПРОДАЖ}`
- Заявка: `https://sbis.ru/tenders/query?code={КОД_ТОЧКИ_ПРОДАЖ}`

Подробности: [Реферальные ссылки](Referalnye-ssylki.md)

## История изменений

- **v.1.3.6 (04.02.2023)**: Добавлено действие "Выставление счета" в Billing.GetHistory.
- **v.1.3.5 (19.03.2022)**: Новое действие "Активность контрагента", устаревшие поля.
- **v.1.3.4 (22.02.2020)**: Дополнительные поля в License.InfoByID.

Полная история: [История изменений API](История изменений API.md)

## Дополнительные файлы
- [Общий обзор API](partner-api.md)
- [Авторизация](partner-api-authorization.md)
- [Формат данных](partner-api-data.md)
- [Настройки](partner-api-settings.md)

## Вопросы и поддержка

Если у вас возникли вопросы по интеграции или использованию API, обращайтесь в группу **Партнерское API** (контакты в документации).

Этот репозиторий содержит полную документацию. Для вопросов по реализации - используйте issues на GitHub.