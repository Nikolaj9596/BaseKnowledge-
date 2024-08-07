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