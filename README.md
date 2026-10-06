[![Java CI with Maven](https://github.com/nesquikpro/cinema-spring-crud/actions/workflows/maven.yml/badge.svg)](https://github.com/nesquikpro/cinema-spring-crud/actions/workflows/maven.yml)

## Java Spring, Spring Security, Spring MVC, Hibernate, PostgreSQL

# 🎬 Cinema Spring CRUD

Проект, демонстрирующий работу CRUD‑операций на стеке **Spring Boot + Hibernate + PostgreSQL**.

## 🚀 Описание

Проект представляет собой веб‑приложение для управления сущностями кинотеатра (фильмы, сеансы, пользователи и т.д.).  
Используется архитектура **MVC** с разделением на контроллеры, сервисы и репозитории.

## 🧰 Технологии

- **Java 17+**
- **Spring Boot** (MVC, Security, Data JPA)
- **Hibernate / JPA**
- **PostgreSQL**
- **Thymeleaf**
- **Maven**

## 🗂 Структура проекта

```
cinema-spring-crud/
├── src/
│   ├── main/
│   │   ├── java/        # контроллеры, сервисы, модели, репозитории
│   │   └── resources/   # конфигурации, шаблоны, application.properties
├── pom.xml              # зависимости Maven
└── README.md
```

## ⚙️ Установка и запуск

1. Клонировать репозиторий:
   ```bash
   git clone https://github.com/nesquikpro/cinema-spring-crud.git
   cd cinema-spring-crud
   ```

2. Создать базу данных PostgreSQL (например, `cinema_db`).

3. Настроить файл `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/cinema_db
   spring.datasource.username=postgres
   spring.datasource.password=your_password
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.show-sql=true
   ```

4. Запустить приложение:
   ```bash
   ./mvnw spring-boot:run
   ```

5. Открыть в браузере:
   [http://localhost:8080](http://localhost:8080)

## 🔐 Авторизация

В проекте реализована базовая аутентификация через **Spring Security**.  
Настройки пользователей и ролей можно изменить в сервисе `UserService` или в базе данных.

## 🧪 API (пример)

| Метод | URL | Описание |
|-------|-----|----------|
| `GET` | `/movies` | Получить список фильмов |
| `POST` | `/movies` | Добавить фильм |
| `PUT` | `/movies/{id}` | Обновить фильм |
| `DELETE` | `/movies/{id}` | Удалить фильм |

## Physical data model
<img width="625" height="284" alt="image" src="https://github.com/user-attachments/assets/0d3445c5-23ac-49d4-a5d3-973ef441e014" />


