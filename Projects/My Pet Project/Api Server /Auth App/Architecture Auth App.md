# Auth App Architecture
#hosting-bot #auth_app
## Регистрация пользователей
###### Архитектура базы данных

```plantuml

	class User{
		+id: int
		+email: str
		+created: datetime
		+updated: datetime
	}

	class Session{
		+id: int
		+user: User
		+expire_date: datetime
		+last_activation: datetime
		+created: datetime
		+updated: datetime
		+is_deactivated: bool
	    +device_agent_id: str
	    +device_agent_represent: str
	    +refresh_token_hash: str
		+generate_refresh_token()
		+check_refresh_token()
		+deactivate()
		+token()
	}
  
	Session::user --> User

	class PushToken {
		+session: Session
		+token: str
		+created: datetime
		+updated: datetime
	}
	PushToken::session --> Session
```

##### Логика рагистрации и Авторизации

1. Пользователь выполняет вход в систему, предоставляя свои учетные данные.
2. При успешной аутентификации генерируется JWT-токен, содержащий информацию о пользователе и его правах и секретный токен для обнавления jwt.
3. Оба токена отправляются обратно пользователю в качестве ответа на запрос входа в систему.
4. В JWT-токене включается дополнительная информация, такая как время создания токена (timestamp) и время истечения токена (expiration time).
5. Когда JWT-токен истекает, пользователь должен обновить его для продолжения использования системы без повторной аутентификации.
6. Пользователь отправляет запрос на обновление токена, предоставляя текущий JWT-токен и секретный токен для обнавления.
7. Сервер проверяет валидность текущего JWT-токена и время его истечения.
8. Если текущий токен валиден и не истек, сервер генерирует новый JWT-токен со свежим временем истечения.
9. Новый JWT-токен отправляется пользователю в ответ на запрос обновления токена.
10. Пользователь сохраняет новый JWT-токен и использует его для дальнейших запросов к системе.
11. При каждом успешном обновлении токена, время истечения продлевается, чтобы пользователь мог продолжать использовать систему без повторной аутентификации.
12. Если обновление токена не удалось (например, из-за недействительного текущего токена или истечения времени жизни), пользователь должен повторно войти в систему для получения нового JWT-токена.

```plantuml
actor Пользователь
participant "Сервер" as Server
database "База данных" as Database
participant "Email-сервис" as EmailService

group POST /register member
activate Пользователь
Пользователь -> Server: Регистрация (POST /register)
activate Server
Server -> Server: Проверка уникальности email
Server -> Database: Проверка уникальности email
activate Database
Database --> Server: Результат проверки
Server -> Server: Создание пользователя
Server -> Database: Создание пользователя
deactivate Database
Server -> EmailService: Отправка кода подтверждения
activate EmailService
EmailService --> Server: Подтверждение отправлено
deactivate EmailService
Server --> Пользователь: Ответ на регистрацию
deactivate Server
end

group POST /confirm member
Пользователь -> Server: Подтверждение почты (POST /confirm)
activate Server
Server -> Server: Проверка кода подтверждения
Server -> Database: Поиск пользователя по коду подтверждения
activate Database
Database --> Server: Пользователь найден
Server -> Server: Пометка пользователя как подтвержденного
Server --> Пользователь: Ответ с подтверждением возврат пользователю jwt и refresh токен
deactivate Database
deactivate Server
end

group POST /login member
Пользователь -> Server: Вход (POST /login)
activate Server
Server -> Server: Аутентификация пользователя
Server -> Database: Поиск пользователя
activate Database
Database --> Server: Пользователь найден
Server -> Database: Создание сессии для пользователя
Server -> Server: Генерация JWT-токена и refresh токен
Server --> Пользователь: Ответ с JWT-токеном и refresh токен
deactivate Database
deactivate Server
end

group POST /confirm member
Пользователь -> Server: Обновление токена (POST /refresh)
activate Server
Server -> Server: Проверка валидности текущего токена
Server -> Server: Проверка валидности refresh токена
Server -> Server: Генерация нового JWT-токена
Server --> Пользователь: Ответ с новым JWT-токеном
deactivate Server
end
```

### Схема взаимодействия модулей

```puml
package use_case <<Folder>> {
  interface IRegistrationUS {
    + execute()
  }

  interface IConfirmUS {
    + execute()
  }

  interface ILoginUS {
    + execute()
  }

  interface IRefreshUS {
    + execute()
  }

  interface ILogoutUS {
    + execute()
  }

  class LogoutUS implements ILogoutUS {
    + execute()
  }

  class RefreshUS implements IRefreshUS {
    + execute()
  }

  class ConfirmUS implements IConfirmUS {
    + execute()
  }

  class RegistrationUS implements IRegistrationUS {
    + execute()
  }

  class LoginUS implements ILoginUS {
    + execute()
  }
}

package repository <<Folder>> {
  interface IUserRepo {
    + get()
    + create()
  }

  interface ISessionRepo {
    + get()
    + create()
    + deactivate()
    + update_expire_date()
    + check_refresh_token()
  }

  interface IPushTokenRepo {
    + get()
    + create()
  }

  class UserRepository implements IUserRepo {
    + get()
    + create()
  }

  class SessionRepository implements ISessionRepo {
    + get()
    + create()
    + deactivate()
    + update_expire_date()
    + check_refresh_token()
  }

  class PushTokenRepository implements IPushTokenRepo {
    + get()
    + create()
  }
}

package service <<Folder>> {
  interface ICacheService{
    + get()
    + set()
    + delete()
  }

  interface ISenderService{
    + send_email_user_confirm_code()
  }
  
  interface IDateTimeManagementService{
    + get_now()
  }
  
  interface ILoggerService{
    + info()
    + error()
    + debug()
    + warning()
  }

  class CacheService implements ICacheService {
    + get()
    + set()
    + delete()
  }

  class SenderService implements ISenderService {
    + send_email_user_confirm_code()
  }

  class DateTimeManagementService implements IDateTimeManagementService {
    + get_now()
  }

  class LoggerService implements ILoggerService {
    + info()
    + error()
    + debug()
    + warning()
  }
}

RegistrationUS --> IUserRepo
RegistrationUS --> ICacheService
RegistrationUS --> ISenderService

ConfirmUS --> IUserRepo
ConfirmUS --> ISessionRepo
ConfirmUS --> IPushTokenRepo
ConfirmUS --> ICacheService
ConfirmUS --> ILoggerService

LoginUS --> IUserRepo
LoginUS --> ICacheService
LoginUS --> ISenderService

RefreshUS --> ISessionRepo
RefreshUS --> ILoggerService
RefreshUS --> IDateTimeManagementService

LogoutUS --> ISessionRepo
```
