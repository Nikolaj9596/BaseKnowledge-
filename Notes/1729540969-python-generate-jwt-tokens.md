---
id: 1729540969-python-generate-jwt-tokens
aliases:
  - Python generate jwt tokens
tags:
  - python
  - jwt
---
# Python generate jwt tokens

Для генерации access_token и refresh_token с использованием JWT, мы можем воспользоваться встроенной библиотекой PyJWT (jwt).
Эта библиотека позволяет легко создавать и декодировать JWT (JSON Web Tokens).
Обычно access_token имеет короткий срок жизни, а refresh_token — более длинный, чтобы можно было обновлять доступ без повторной аутентификации.

## Шаги:
1. Установка PyJWT: 

Если еще не установлена, можно установить через pip:

```bash
pip install PyJWT
```
2. Метод для генерации токенов:

Ниже представлен метод для генерации access_token и refresh_token.
Время жизни каждого токена можно настраивать через параметры.

```python
import jwt
import datetime
from typing import Tuple

SECRET_KEY = "your_secret_key"  # Используйте безопасный секретный ключ
ALGORITHM = "HS256"  # Стандартный алгоритм для подписи JWT

def generate_tokens(user_id: int, user_type: str, session_id: str) -> Tuple[str, str]:
    """Генерация access и refresh токенов с использованием JWT."""
    
    # Время жизни токенов
    access_token_expires_in = datetime.timedelta(minutes=15)  # Access token живет 15 минут
    refresh_token_expires_in = datetime.timedelta(days=7)     # Refresh token живет 7 дней

    # Текущее время
    now = datetime.datetime.utcnow()

    # Создание access_token
    access_token_payload = {
        "user_id": user_id,
        "user_type": user_type,
        "session_id": session_id,
        "exp": now + access_token_expires_in,  # Время истечения
        "iat": now,  # Время создания токена
    }
    access_token = jwt.encode(access_token_payload, SECRET_KEY, algorithm=ALGORITHM)

    # Создание refresh_token
    refresh_token_payload = {
        "user_id": user_id,
        "session_id": session_id,
        "exp": now + refresh_token_expires_in,  # Время истечения
        "iat": now,  # Время создания токена
    }
    refresh_token = jwt.encode(refresh_token_payload, SECRET_KEY, algorithm=ALGORITHM)

    # Возвращаем оба токена
    return access_token, refresh_token
```

## Пример использования:
```python
user_id = 123
user_type = "customer"
session_id = "session123"
access_token, refresh_token = generate_tokens(user_id, user_type, session_id)
print("Access Token:", access_token)
print("Refresh Token:", refresh_token)
```
## Расшифровка токена:
Для проверки подлинности или получения данных из токена, можно использовать метод jwt.decode:

```python
def decode_token(token: str) -> dict:
    """Расшифровка и проверка JWT токена."""
    try:
        decoded_payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        return decoded_payload
    except jwt.ExpiredSignatureError:
        raise Exception("Token has expired")
    except jwt.InvalidTokenError:
        raise Exception("Invalid token")
```
