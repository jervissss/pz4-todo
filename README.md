# Практическое занятие №4
## Группа: ЭФМО-02-25  
## ФИО: Евдоков Богдан Вадимович

## 🎯 Цели работы

- Освоить маршрутизацию HTTP-запросов с использованием роутера **chi**
- Реализовать полнофункциональный CRUD-сервис "Список задач"
- Научиться работать с middleware (логирование, CORS)
- Создать REST API с обработкой всех основных HTTP-методов

## 🚀 Что умеет этот сервер?

Сервер представляет собой REST API для управления задачами (To-Do list) с использованием роутера **chi** и включает полный набор CRUD-операций.

### 📍 Доступные эндпоинты:

| Метод | URL | Действие |
|-------|-----|-----------|
| `GET` | `/health` | Проверка статуса сервера |
| `GET` | `/api/tasks` | Получить список всех задач |
| `POST` | `/api/tasks` | Создать новую задачу |
| `GET` | `/api/tasks/{id}` | Получить задачу по ID |
| `PUT` | `/api/tasks/{id}` | Обновить задачу |
| `DELETE` | `/api/tasks/{id}` | Удалить задачу |

## 📦 Требования для запуска

- **Go 1.21+** - [скачать с официального сайта](https://go.dev/dl/)
- **Git** - [скачать с официального сайта](https://git-scm.com/downloads)

## 🛠 Быстрый старт

### Шаг 1: Скачивание и подготовка проекта

```powershell
# Клонируем репозиторий
git clone https://github.com/jervissss/pz4-todo.git
cd pz4-todo

# Инициализируем модуль Go
go mod init example.com/pz4-todo

# Устанавливаем зависимости
go get github.com/go-chi/chi/v5
go get github.com/go-chi/chi/v5/middleware
```

### Шаг 2: Запуск сервера
```powershell
# Через исходный код
go run main.go
```

При успешном запуске вы увидите сообщение: `listening on :8080`

## 🔍 Тестирование API
### 1. Проверка здоровья сервера
```powershell
curl -i http://localhost:8080/health
```
Ответ: `OK`

### 2. Создание новой задачи
```powershell
curl -Method POST http://localhost:8080/api/tasks `
  -Headers @{"Content-Type"="application/json"} `
  -Body '{"title":"Выучить chi"}'
```
Ответ: `{"id":1,"title":"Изучить роутер chi","done":false,"created_at":"2024-01-15T10:30:00Z","updated_at":"2024-01-15T10:30:00Z"}`
### 3. Получение списка задач
```powershell
curl -i http://localhost:8080/api/tasks
```
### 4. Получение конкретной задачи
```powershell
curl -i http://localhost:8080/api/tasks/1
```
### 5. Обновление задачи
```powershell
curl -Method PUT http://localhost:8080/api/tasks/1 `
  -Headers @{"Content-Type"="application/json"} `
  -Body '{"title":"Выучить chi глубже","done":true}'
```
### 6. Удаление задачи
```powershell
curl -Method DELETE http://localhost:8080/api/tasks/1
```
## 📁 Структура проекта

<img width="335" height="396" alt="image" src="https://github.com/user-attachments/assets/849a06cd-805b-4dbe-96b4-84710c7f536d" />

## 🔧 Ключевые особенности реализации
### 🎯 Маршрутизация с chi
Использован роутер chi для чистой и читаемой маршрутизации:

```go
r.Route("/api", func(api chi.Router) {
    api.Mount("/tasks", h.Routes())
})
```
### Middleware
* Логирование - записывает метод, путь и время выполнения
* CORS - разрешает кросс-доменные запросы
* RequestID и Recoverer от chi


### 📋 Результаты тестирования
Маршрут	Метод	Запрос	Ожидаемый ответ	Фактический ответ
`/health	GET`	-	200 OK	200 OK

`/api/tasks	GET`	-	200 OK

`/api/tasks	POST	{"title":"Тест"}` -	201 Created

`/api/tasks/1	GET`	-	200 OK

`/api/tasks/1	PUT	{"title":"Название","done":true}` -	200 OK

`/api/tasks/1	DELETE`	-	204 No Content

`/api/tasks/999	GET`	-	404 Not Found

`/api/tasks	POST	{"title":""}` -	400 Bad Request

## 📸 Отчетные материалы
`http://localhost:8080/health `
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/184bd448-c5db-4a6e-9ad3-5163e80cea4e" />

```powershell
curl -Method POST http://localhost:8080/api/tasks `
  -Headers @{"Content-Type"="application/json"} `
  -Body '{"title":"Выучить chi"}'
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/32c1879a-ed7e-4f0c-abf2-9c6b4db6150b" />

```powershell
curl http://localhost:8080/api/tasks
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4faaa486-95db-44ae-9f8f-8fd122d69ef9" />

```powershell
curl -Method PUT http://localhost:8080/api/tasks/1 `
  -Headers @{"Content-Type"="application/json"} `
  -Body '{"title":"Выучить chi глубже","done":true}'
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c93b00db-00ee-4db0-9450-da049cef6c16" />

```powershell
curl -Method DELETE http://localhost:8080/api/tasks/1
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b720cc69-92cd-475a-ba56-aa7d9af60cd6" />

---
*Этот проект был создан в качестве учебного задания.*
