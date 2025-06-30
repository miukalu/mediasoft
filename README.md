# mediasoft
<h3 align="center">Задание 1</h3>

![image](https://github.com/user-attachments/assets/f0dd8e83-8cd9-4220-aee7-36842d4b7856)
![image](https://github.com/user-attachments/assets/be6f63d2-ddf3-4020-af48-e27fdb057ca8)
<h3 align="center">Задание 2</h3>

1. Создание и управление списками покупок
2. Работа с товарами (добавление/удаление, отметки, комментарии, редакция)
3. 	Категоризация товаров
4.	Синхронизация между устройствами
5.	Регистрация и авторизация
6.	История покупок
7.	Напоминания и уведомления
8.	Настройки приложения
<h3 align="center">Задание 3</h3>

Аутентификация:
- POST /api/auth/register – регистрация нового пользователя,
- POST /api/auth/login – вход в систему,
- POST /api/auth/logout – выход из системы.
  
Управление профилем:
- GET /api/users/{userId} – получение данных пользователя,
- PUT /api/users/{userId} – обновление профиля.
  
Работа с чек-листом:
- POST /api/lists – создание нового списка,
- GET /api/lists/{listId} – просмотр конкретного списка,
- PUT /api/lists/{listId} – редактирование списка,
- DELETE /api/lists/{listId} – удаление списка,
- GET /api/lists?userId={userId} – получение всех списков пользователя.
  
Управление продуктами:
- POST /api/lists/{listId}/items – добавление товара в список,
- GET /api/lists/{listId}/items – просмотр всех товаров списка,
- PATCH /api/lists/{listId}/items/{itemId} – обновление статуса товара (куплен/не куплен),
- DELETE /api/lists/{listId}/items/{itemId} – удаление товара из списка.
  
Работа с каталогом продуктов:
- GET /api/products?search={query} – поиск товаров по названию.
  
![image](https://github.com/user-attachments/assets/efcea497-9847-4b5c-8d14-213fb547c870)
![image](https://github.com/user-attachments/assets/eb9d3c0d-04c1-4f84-8469-e9bc6a84f081)
![image](https://github.com/user-attachments/assets/48619dfc-f28e-4577-82f3-b3cc01b1049f)
![image](https://github.com/user-attachments/assets/cce81a08-302f-48e8-89ec-43f32dfd6a14)
<h3 align="center">Задание 4</h3>

![image](https://github.com/user-attachments/assets/ebdbeed9-f09e-4391-8f15-4f300e0bd9f8)
![image](https://github.com/user-attachments/assets/32112e3b-7e80-45a6-b4a4-042b4c26535b)
https://www.figma.com/design/G8zxKRSyhnP480wHAXNrRM/Mediasoft?node-id=0-1&t=L7C4tanN38atq4L1-1
<h3 align="center">Задание 5</h3>
<h4>Сценарий покупки</h4>
Пользователь формирует список покупок, добавляя товары из готовой базы с помощью кнопки «add». Во время похода в магазин он нажимает кнопку корзины рядом с товаром, что отмечает его как купленный. Приложение визуально подтверждает действие, изменяя фон товара на зеленый, заменяя значок корзины на крестик, обновляя статус товара на «purchased» в базе данных и фиксируя метку последнего изменения (last_modified). В онлайн-режиме данные сразу синхронизируются с сервером, а в оффлайн-режиме изменения сохраняются в локальной очереди (sync_queue) для последующей синхронизации.
<h4>Сценарий отмены покупки</h4>
Для отмены покупки пользователь нажимает крестик рядом с товаром. Приложение возвращает исходный фон и значок корзины, изменяет статус товара на «not_purchased» и обновляет метку last_modified. Как и при покупке, синхронизация происходит мгновенно в онлайн-режиме или откладывается в оффлайн-режиме до появления сети.

<h4>Используемые API</h4>
- PATCH /api/lists/{list_id}/items/{item_id} – обновление статуса товара (покупка/отмена).
<h5>Тело:</h5>
```json
{
  "status": "purchased" | "not_purchased",
  "last_modified": "2025-06-30T17:19:00Z"
}
```
<h5>Ответ:</h5>
```json
{
  "id": "item_1",
  "status": "purchased",
  "synced": true,
  "new_last_modified": "2025-06-30T17:20:00Z"
}
```
- POST /api/lists/{list_id}/sync – пакетная синхронизация изменений в оффлайн-режиме.
<h5>Тело:</h5>
```json
{
  "changes": [
    {
      "item_id": "item_1",
      "status": "purchased",
      "last_modified": "2025-06-30T17:19:00Z"
    }
  ]
}
```
<h5>Ответ:</h5>
```json
{
  "result": "success",
  "synced_items": [
    {
      "item_id": "item_1",
      "status": "purchased",
      "new_last_modified": "2025-06-30T17:20:00Z"
    }
  ]
}
```
Данные о покупках хранятся в двух базах данных: локальной и глобальной, что обеспечивает поддержку оф-флайн-режима и синхронизацию. Синхронизация между базами выполня-ется с использованием пакетного эндпоинта POST /api/lists/{list_id}/sync, где сервер обновляет глобальную БД и возвращает новые метки времени.
![image](https://github.com/user-attachments/assets/933ea7cd-6ac8-4557-9b1e-ba778dccea1b)
<h3 align="center">Задание 6</h3>
Сложности:
Удаление списка одним пользователем, когда второй его редактирует
Настройки (прав, интерфейса)
Что может вызвать пуш-уведомление
UX/UI-вызовы
Адаптация под разные устройства (начиная от платформы заканчивая размером экрана)
Вопросы:
Требуется ли оффлайн-режим?
Какие роли пользователей поддерживаются в приложении?
Понадобится ли в будущем интеграция с магазинами?
Нужна ли история изменений списка?
Должны ли списки сортировать продукты по категориям?
Используется ли единая форма для входа и регистрации или это отдельные экраны с разной логикой?
<h3 align="center">Задание 7</h3>
```sql
SELECT authors.authorname, SUM(books.price) AS totalcost FROM authors JOIN books ON authors.id = books.authorid GROUP BY authors.authorname ORDER BY totalcost DESC
```
 ![image](https://github.com/user-attachments/assets/729247ba-10d1-4c20-9c38-b9b854fcdb25)
```sql
2.	SELECT authors.authorname, SUM(books.price) AS totalcost FROM authors JOIN books ON authors.id = books.authorid GROUP BY authors.authorname HAVING SUM (books.price) > 1500
```
 ![image](https://github.com/user-attachments/assets/600c9b86-5a35-494d-9caf-804bc9ac0688)
```sql
3.	SELECT authors.authorname, COUNT(books.bookname) AS bookcount FROM authors LEFT JOIN books ON authors.id = books.authorid GROUP BY authors.authorname
```
 ![image](https://github.com/user-attachments/assets/f31d0ae5-7cae-438d-9d11-13ddf4d9c3da)
```sql
4.	SELECT authors.authorname AS bookcount FROM authors LEFT JOIN books ON authors.id = books.authorid WHERE books.bookname IS NULL GROUP BY authors.authorname
```
 ![image](https://github.com/user-attachments/assets/43bc5d2c-2e18-44df-b5a7-99e2aa9afacc)
