# Cinema Spring CRUD

Учебное веб-приложение для управления кинотеатром: кинотеатры, залы, фильмы, сеансы, билеты, сотрудники и т.д.
Серверный MVC на Spring Boot, страницы рендерятся через Thymeleaf, данные хранятся в PostgreSQL.

[![CI](https://github.com/nnsquik/cinema-spring-crud/actions/workflows/ci.yml/badge.svg)](https://github.com/nnsquik/cinema-spring-crud/actions/workflows/ci.yml)

## Возможности

- Регистрация и вход (Spring Security, форма логина, пароли хешируются BCrypt)
- CRUD для каждой сущности: список, добавление, редактирование, удаление
- Поиск по названию: точное совпадение и поиск «по схожести»
- Страницы с подробной информацией (сотрудники, залы, сеансы, фильмы, билеты)
- Валидация форм (Bean Validation), например телефон в формате `7(999)-999-99-99`
- Общее навигационное меню (Thymeleaf-фрагмент)

## Стек

| Область        | Технологии                                    |
|----------------|-----------------------------------------------|
| Язык           | Java 17                                       |
| Фреймворк      | Spring Boot 2.6.0 (Web, Data JPA, Validation) |
| Безопасность   | Spring Security (JDBC-аутентификация, BCrypt) |
| Шаблоны        | Thymeleaf, Bootstrap 5                        |
| База данных    | PostgreSQL                                    |
| Сборка         | Maven                                         |

## Модель данных

```
Cinema ──< Ticket >── Place
              │  \
              │   └── Employee >── Post
              └── Hall >── Category

Genre ──< Film ──< Session >── Hall

User (app_user) ──< user_role
```

| Сущность   | Основные поля                                         |
|------------|-------------------------------------------------------|
| `Cinema`   | название, адрес, телефон                              |
| `Hall`     | название, вместимость (30–100), категория             |
| `Category` | название, цена билета (100–500)                       |
| `Film`     | название, длительность, описание, жанр                |
| `Genre`    | название                                              |
| `Session`  | дата, время, фильм, зал                               |
| `Ticket`   | кинотеатр, зал, место, сотрудник                      |
| `Place`    | ряд (1–13), место (1–13)                              |
| `Employee` | ФИО, почта, телефон, должность                        |
| `Post`     | название, оклад (45 000–150 000)                      |
| `User`     | логин, пароль, активность, роли (`USER`)              |

## Структура проекта

```
src/main
├── java/com/example/cinema
│   ├── App.java                      # точка входа
│   ├── config                        # WebSecurityConfig, MvcConfig
│   ├── controllers                   # CinemaController, RegistrationController
│   ├── enums                         # Role
│   ├── models                        # JPA-сущности
│   └── repository                    # Spring Data репозитории
└── resources
    ├── application.properties
    └── templates                     # Thymeleaf-шаблоны
        ├── main, add, edit,          # списки, формы добавления, редактирования,
        ├── filter, details           # поиск, страницы деталей
        └── footer.html, login.html, registration.html, mainpage.html
```

## Быстрый старт

### Требования

- JDK 17+
- Maven 3.8+ (или используйте `./mvnw`, если он есть в репозитории)
- PostgreSQL 12+

### 1. База данных

Через Docker:

```bash
docker run --name cinema-db \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=cinema_db \
  -p 5432:5432 -d postgres:15
```

Или создайте базу вручную:

```sql
CREATE DATABASE cinema_db;
```

Таблицы создаются автоматически (`spring.jpa.hibernate.ddl-auto=update`).

### 2. Настройка подключения

По умолчанию параметры берутся из `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/cinema_db
spring.datasource.username=postgres
spring.datasource.password=<пароль>
```

Не храните реальные пароли в репозитории. Переопределяйте их переменными окружения:

```bash
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/cinema_db
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=postgres
```

### 3. Запуск

```bash
mvn spring-boot:run
```

Или сборка и запуск jar:

```bash
mvn clean package
java -jar target/cinema-spring-crud-0.0.1-SNAPSHOT.jar
```

Приложение будет доступно на <http://localhost:8080>.

### 4. Первый вход

1. Откройте `/registration` и создайте пользователя.
2. Войдите на `/login`, после входа откроется `/mainpage`.
3. Через верхнее меню переходите к нужным разделам.

> Рекомендуемый порядок наполнения данными, чтобы работали выпадающие списки:
> должности → сотрудники, категории → залы, жанры → фильмы, фильмы + залы → сеансы,
> кинотеатры + места → билеты.

## Основные маршруты

| Раздел      | Список      | Добавление            | Поиск                   |
|-------------|-------------|-----------------------|-------------------------|
| Должности   | `/post`     | `/post/addPost`       | `/post/postFilter`      |
| Сотрудники  | `/employee` | `/employee/addEmployee` | `/employee/employeeFilter` |
| Категории   | `/category` | `/category/addCategory` | `/category/categoryFilter` |
| Залы        | `/hall`     | `/hall/addHall`       | `/hall/hallFilter`      |
| Сеансы      | `/session`  | `/session/addSession` | `/session/sessionFilter` |
| Фильмы      | `/film`     | `/film/addFilm`       | `/film/filmFilter`      |
| Жанры       | `/genre`    | `/genre/addGenre`     | `/genre/genreFilter`    |
| Кинотеатры  | `/cinema`   | `/cinema/addCinema`   | `/cinema/cinemaFilter`  |
| Билеты      | `/ticket`   | `/ticket/addTicket`   | `/ticket/ticketFilter`  |
| Места       | `/place`    | `/place/addPlace`     | —                       |

Редактирование: `/<раздел>/{id}/edit`, удаление: `/<раздел>/{id}/remove`, детали: `/<раздел>/{id}`.

Доступ без авторизации только к `/` и `/registration`, остальное требует входа.

## Тесты и CI

```bash
mvn -B verify
```

Сборка и тесты автоматически запускаются в GitHub Actions
(см. `.github/workflows/ci.yml`) при каждом push и pull request.
