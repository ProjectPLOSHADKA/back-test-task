# Тестовое задание для Junior Python Backend Developer (FastAPI + TortoiseORM)

## Цель
Проверить навыки разработки асинхронных API с использованием:
- **FastAPI**
- **TortoiseORM**
- JWT-аутентификации
- Кеширования
- Фоновых задач
- Сложных запросов к БД

## Задача: Система блога с подписками и уведомлениями

### 1. Модели данных (TortoiseORM)

#### User
- `id` (UUID)
- `username` (уникальный)
- `email` (уникальный)
- `hashed_password`
- `is_active` (по умолчанию `True`)
- `created_at`

#### Post
- `id` (UUID)
- `title`
- `content`
- `author` (ForeignKey → User)
- `created_at`
- `updated_at`

#### Like
- `id` (UUID)
- `user` (ForeignKey → User)
- `post` (ForeignKey → Post)
- `created_at`

#### Subscription
- `id` (UUID)
- `subscriber` (ForeignKey → User)
- `target_user` (ForeignKey → User)
- `created_at`

#### Notification
- `id` (UUID)
- `user` (ForeignKey → User)
- `message`
- `is_read` (по умолчанию `False`)
- `created_at`

### 2. API Endpoints

#### Аутентификация (JWT)
- `POST /auth/register/` – регистрация
- `POST /auth/login/` – вход (возвращает `access_token`)
- `GET /auth/me/` – информация о текущем пользователе

#### Посты
- `POST /posts/` – создание поста
- `GET /posts/` – список всех постов (с пагинацией)
- `GET /posts/{post_id}/` – детали поста
- `PUT /posts/{post_id}/` – обновление поста
- `DELETE /posts/{post_id}/` – удаление поста

#### Лайки
- `POST /posts/{post_id}/like/` – поставить/убрать лайк
- `GET /posts/{post_id}/likes/` – список лайкнувших

#### Подписки
- `POST /users/{user_id}/subscribe/` – подписаться/отписаться
- `GET /users/subscriptions/` – список моих подписок

#### Уведомления
- `GET /notifications/` – список непрочитанных уведомлений
- `POST /notifications/{notification_id}/read/` – пометить как прочитанное

### 3. Дополнительные требования

#### Фоновые задачи
- Генерация уведомлений для подписчиков при создании поста (Celery/FastAPI BackgroundTasks)

#### Кеширование
- Кеширование TOP-10 постов по лайкам (Redis)

#### Аналитика
- `GET /analytics/likes/` – статистика лайков за период

### 4. Тестирование
Требуется покрытие:
- Аутентификации
- Работы с постами
- Логики лайков/подписок
- Кеширования

### 5. Развертывание
- Docker-контейнеры (FastAPI + PostgreSQL + Redis)
- Автоматические миграции (Aerich)

## Критерии оценки
✅ Работоспособность всех эндпоинтов  
✅ Корректное использование async/await  
✅ Оптимизированные запросы к БД  
✅ Покрытие тестами (>80%)  
✅ Документация (Swagger/Redoc)  
✅ Реализация кеширования  

## Пример запроса
```http
POST /posts/
Headers: Authorization: Bearer {token}
Body:
{
    "title": "My first post",
    "content": "Hello, world!"
}
```

## Срок выполнения: 3-5 дней
## Формат сдачи: GitHub-репозиторий с инструкцией по запуску
