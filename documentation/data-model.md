# Модель данных OpenCare
## 1. Основные сущности OpenCare
| Сущность       | Применение |
| --------       |    ---- |
|  User          | Хранит данные волонтёров, организаторов и администраторов   |
| Category       | Позволяет классифицировать объявления: экология, мероприятия, животные, IT |
| Announcement   |Хранит информацию о волонтёрской возможности|
| Application    |Фиксирует отклик волонтёра и решение организатора |
|ModerationAction| Хранит историю скрытия и восстановления объявлений администратором   |
---
## 2. Атрибуты и ключи
### 1. User
|Атрибут        | Тип      | Назначение  |
| --------      | -----    | ------|
| `id`          | UUID     | PK — уникальный идентификатор
| `name`        |VARCHAR   |Имя пользователя
|`email`        |VARCHAR   |Email, уникальный
|`password_hash`|VARCHAR   |Хеш пароля
|`role`         |  ENUM    |`volunteer`, `organizer`, `admin`
|`created_at`   |TIMESTAMP |Дата регистрации

### 2. Category
| Атрибут | Тип | Назначение
| ------ | ----- | ------ |
|`id`  |UUID |PK
|`name` |VARCHAR |Название категории
|`description`|TEXT |Описание категории

### 3. Announcement

| Атрибут | Тип | Назначение |
| :--- | :--- | :--- |
| **id** | UUID | PK |
| **organizer_id** | UUID | FK → User_id |
| **category_id** | UUID | FK → Category_id |
| **title** | VARCHAR | Заголовок |
| **description** | TEXT | Описание волонтёрской задачи |
| **event_start** | TIMESTAMP | Дата и время начала |
| **event_end** | TIMESTAMP, NULL | Дата и время окончания, если указано |
| **location** | VARCHAR, NULL | Адрес или место проведения |
| **format** | ENUM | offline, online |
| **volunteer_limit** | INTEGER | Максимум одобренных волонтёров |
| **status** | ENUM | published, closed, completed |
| **is_hidden** | BOOLEAN | Скрыто ли объявление модератором |
| **created_at** | TIMESTAMP | Дата создания |
| **updated_at** | TIMESTAMP | Дата последнего изменения |

### 4. Application

| Атрибут | Тип | Назначение |
| :--- | :--- | :--- |
| **id** | UUID | PK |
| **announcement_id** | UUID | FK → Announcement_id |
| **volunteer_id** | UUID | FK → User_id |
| **status** | ENUM | pending, approved, rejected, cancelled |
| **created_at** | TIMESTAMP | Дата подачи |
| **updated_at** | TIMESTAMP | Дата последнего изменения |

### 5. ModerationAction

| Атрибут | Тип | Назначение |
| :--- | :--- | :--- |
| **id** | UUID | PK |
| **announcement_id** | UUID | FK → Announcement_id |
| **admin_id** | UUID | FK → User_id |
| **action** | ENUM | hide, restore |
| **reason** | TEXT | Причина действия |
| **created_at** | TIMESTAMP | Дата и время действия |
---
## 3. Связи между сущностями

| Связь | Тип_ | Объяснение |
| :--- | :-------: | :--- |
| **User → Announcement** | 1 : N | Один организатор может создать много объявлений |
| **Category → Announcement** | 1 : N | Одна категория может использоваться во многих объявлениях |
| **Announcement → Application** | 1 : N | У одного объявления может быть много заявок |
| **User → Application** | 1 : N | Один волонтёр может подать много заявок |
| **Announcement → ModerationAction** | 1 : N | Одно объявление может иметь несколько действий модерации |
| **User → ModerationAction** | 1 : N | Один администратор может выполнить много действий модерации |
---
