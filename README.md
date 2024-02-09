# Messenger

# Обработка ответов
Возвращаемые HTTP-коды ответа:
- 2XX - операция выполнена успешно
- 4XX - операция не выполнена, возвращается сообщение с ошибкой клиенту. Тело ответа:
```
{
	"error": "value"
}
```
- 401 - Не авторизован (требуется переполучить токен или залогиниться снова)
- 5XX - ошибки сервера, данные об ошибке обычно не возвращаются

# Swagger UI
Swagger UI доступен по адресу: https://[host]/swagger/index.html с документацией API-методов, подходит для тестирования API-методов, не требующих Bearer-токен.

# API-методы

## /api/Info - получить данные о конфигурации сервера
##### HTTP-метод: GET
Тело ответа:
```
{
  "serverName": "Test Server",
  "serverType": 0
}
```
Значения для serverType:
```
enum ServerType
{
    Public = 0,
    Private = 1
}
```
Если сервер публичный, то для регистрации используется метод <b>/api/Account/Register</b>.
Если сервер приватный, то для регистрации используется метод <b>
/api/Account/RegisterWithInviteCode</b>. Для регистрации на приватном сервере нужно иметь код-приглашение от пользователя, который уже есть зарегистрирован. Зарегистрированный пользователь может сгенерировать 1 пригласительный код при помощи метода <b>/api/Users/GenerateInviteCode</b>.

------------


## /api/Account/Register - зарегистрироваться без кода-приглашения
##### HTTP-метод: POST
Тело запроса:
```
{
  "username": "value",
  "password": "value"
}
```

------------


## /api/Account/RegisterWithInviteCode - зарегистрироваться с использованием кода-приглашения
##### HTTP-метод: POST
Тело запроса:
```
{
  "username": "DCYgF3ICHyQDaj5z",
  "password": "string",
  "inviteCode": "string"
}
```

------------


## /api/Account/Delete - удалить аккаунт
##### HTTP-метод: DELETE
Нужен Bearer-токен

------------


## /api/Auth/RefreshToken - выполнить вход по refresh-токену
##### HTTP-метод: POST
Тело запроса:
```
{
  "refreshToken": "string"
}
```
Тело ответа:
```
{
  "user": {
    "id": "dba34617-0a66-45a1-94bb-ce1ba166fa48",
    "username": "string"
  },
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1laWQiOiJkYmEzNDYxNy0wYTY2LTQ1YTEtOTRiYi1jZTFiYTE2NmZhNDgiLCJuYmYiOjE3MDc1MDc2MjAsImV4cCI6MTcwNzUwODIyMCwiaWF0IjoxNzA3NTA3NjIwLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjUwMDAiLCJhdWQiOiJodHRwOi8vbG9jYWxob3N0OjUwMDAifQ.S0baLKSiNy1UWsbojnypqbOv6OxZh0uoyCxjLd-BCHo",
  "accessTokenExpirationMinutes": 10,
  "refreshToken": "2f73039e-dcd4-419c-922b-0f9a241e0810",
  "refreshTokenExpirationMinutes": 131400
}
```
------------

## /api/Auth/Login - выполнить вход
##### HTTP-метод: POST
Тело запроса:
```
{
  "username": "string",
  "password": "string"
}
```
Тело ответа:
```
{
  "user": {
    "id": "dba34617-0a66-45a1-94bb-ce1ba166fa48",
    "username": "string"
  },
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1laWQiOiJkYmEzNDYxNy0wYTY2LTQ1YTEtOTRiYi1jZTFiYTE2NmZhNDgiLCJuYmYiOjE3MDc1MDc2MjAsImV4cCI6MTcwNzUwODIyMCwiaWF0IjoxNzA3NTA3NjIwLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjUwMDAiLCJhdWQiOiJodHRwOi8vbG9jYWxob3N0OjUwMDAifQ.S0baLKSiNy1UWsbojnypqbOv6OxZh0uoyCxjLd-BCHo",
  "accessTokenExpirationMinutes": 10,
  "refreshToken": "2f73039e-dcd4-419c-922b-0f9a241e0810",
  "refreshTokenExpirationMinutes": 131400
}
```

------------

## /api/Contacts/All - получить список всех контактов пользователя
##### HTTP-метод: GET
Нужен Bearer-токен
Тело ответа:
```
{
    "contacts": [
        {
            "id": "4f41165a-2426-482b-bb33-e9bcb41e01e4",
            "username": "pussy",
            "onlineStatus": 0
        }
    ]
}
```

Значения для Online Status:
```
enum OnlineStatus
{
	Offline = 0,
	Online = 1
}
```

------------


## /api/Contacts/Add - добавить пользователей в контакты
##### HTTP-метод: POST
Нужен Bearer-токен
Тело запроса:
```
{
  "userIds": [
    "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  ]
}
```

------------

## /api/Contacts/Delete - удалить пользоватеелй из контактов
##### HTTP-метод: DELETE
Нужен Bearer-токен
Тело запроса:
```
{
  "userIds": [
    "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  ]
}
```


------------
## /api/Messages/Send - отправить сообщение пользователю
##### HTTP-метод: POST
Нужен Bearer-токен
Тело запроса:
```
{
    "receiverId": "8f870841-f145-4b18-b552-91d41cf7cec7",
    "content": "TEXT",
    "messageType": 1
}
```
Тело ответа: - ID сообщения
```
"c3c4297f-701c-4c1e-a0f6-9bdb5f1afbbd"
```

Значения для Message Type:
```
enum MessageType
{
	Default = 0,
	Encrypted = 1
}
```

------------

## /api/Users/Get - получить данные о пользователях
##### HTTP-метод: POST
Нужен Bearer-токен
Тело запроса:
```
{
  "userIds": [
    "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  ]
}
```

Тело ответа:
```
{
    "users": [
        {
            "id": "8f870841-f145-4b18-b552-91d41cf7cec7",
            "username": "admin",
            "onlineStatus": 1
        },
        {
            "id": "b152ca46-534a-43d2-9b25-ad4af260d763",
            "username": "string",
            "onlineStatus": 0
        }
    ]
}
```


------------

## /api/Users/GenerateInviteCode - Сгенерировать код-приглашение
##### HTTP-метод: GET
Нужен Bearer-токен
Тело ответа:
```
89153
```

# Real-time уведомления с использованием SignalR
Для получения уведомлений используется ServerSentEvents и Long Polling.

Пример подключения к хабу:
```
 _hub = new HubConnectionBuilder()
     .WithUrl($"{host}/{NotificationHub}", options =>
     {
         options.AccessTokenProvider = () => Task.FromResult(accessToken);
         options.Transports = HttpTransportType.ServerSentEvents | HttpTransportType.LongPolling;
     })
     .WithAutomaticReconnect()
     .Build();
```

Названия событий хаба:
```
public const string RECEIVE_MESSAGE = "ReceiveMessage";
public const string CONTACT_ADDED   = "ContactAdded";
public const string CONTACT_DELETED = "ContactDeleted";
public const string USER_OFFLINE    = "UserOffline";
public const string USER_ONLINE     = "UserOnline";
```

Подписка на события хаба:
```
// Контакт вышел из сети
_hub.On<Guid>(EventNames.USER_OFFLINE, (userId) =>
{
	OnUserGetsOffline(userId);
});

// Контакт подключился к сети
_hub.On<Guid>(EventNames.USER_ONLINE, (userId) =>
{
	OnUserGetsOnline(userId);
});

// Новое сообщение
_hub.On<TextMessageNotificationDTO>(EventNames.RECEIVE_MESSAGE, (notification) =>
{
	OnMessageReceived(notification);
});

// Добавлен контакт
_hub.On<NewContactNotificationDTO>(EventNames.CONTACT_ADDED, (notification) =>
{
	OnContactAdded(notification);
});

// Удалён контакт
_hub.On<ContactDeletedNotificationDTO>(EventNames.CONTACT_DELETED, (notification) =>
{
	OnContactDeleted(notification);
});

// При переподключении
_hub.Reconnecting += OnHubReconnecting; // При необходимости
```

### Описание объектов уведомлений:

Объект TextMessageNotificationDTO:
```
{
	"id": "guid_value", // ID сообщения
	"senderId": "guid_value",
	"receiverId": "guid_value",
	"content": "value",
	"messageType": 0,
}
```

------------

Объект NewContactNotificationDTO:
```
{
	"userId": "guid_value",
	"onlineStatus": 0,
}
```

------------


Объект ContactDeletedNotificationDTO:
```
{
	"userId": "guid_value"
}
```

## Использование консольного клиента для прослушивания событий:
Выделены вводимые данные

Хост: <b>https://localhost:7251</b>

Имя пользователя: <b>string</b>

Пароль: <b>string</b>

23:18:36:
Вошёл как string, id: dba34617-0a66-45a1-94bb-ce1ba166fa48
