# Модульний контроль 2

**Проєкт:** Вебзастосунок «CourseHub» — система онлайн-запису на курси
**Виконав:** Осіпович Ярослав
**Варіант:** 1

---

## Частина А. Тест

1. **б)** уніфікована мова моделювання.
2. **б)** послідовності.
3. **б)** роль користувача або зовнішня система.
4. **б)** обов’язкове використання іншого варіанта.
5. **б)** private.
6. **а)** частина не існує без цілого.
7. **б)** альтернативу (if–else).
8. **б)** розміщення артефактів на вузлах.        
9. **б)** бізнес-логіки.
10. **б)** Controller.
---

## Частина Б. Практичне завдання (на прикладі проєкту «CourseHub»)

### 1. Діаграма варіантів використання (Use Case)
*Актори: Студент, Викладач, Адміністратор. Прецеденти: Записатися на курс, Переглянути студентів, Керувати курсами, Перевірити місця (include), Надіслати лист (extend).*

```mermaid
flowchart LR
    Student((Студент))
    Teacher((Викладач))
    Admin((Адміністратор))

    subgraph System [Система CourseHub]
        UC1(Переглянути каталог)
        UC2(Записатися на курс)
        UC3(Перевірити місця)
        UC4(Надіслати лист)
        UC5(Переглянути студентів)
        UC6(Керувати курсами)
    end

    Student --> UC1
    Student --> UC2
    Teacher --> UC5
    Admin --> UC6

    UC2 -.->|<<include>>| UC3
    UC4 -.->|<<extend>>| UC2
```

### 2. Діаграма класів
*5 класів із атрибутами, операціями та кратністю.*

```mermaid
classDiagram
    class User {
        #int id
        #String email
        +login() void
    }
    class Student {
        +String studentCardId
        +enroll() void
    }
    class Course {
        +String title
        +int maxSeats
        +getAvailableSeats() int
    }
    class Enrollment {
        +Date enrollmentDate
        +String status
        +confirm() void
    }
    class Payment {
        +double amount
        +processPayment() boolean
    }

    User <|-- Student
    Student "1" -- "*" Enrollment : makes
    Course "1" -- "*" Enrollment : has
    Enrollment "1" -- "1" Payment : requires
```

### 3. Діаграма послідовності з фрагментом alt
*Сценарій: Студент записується на курс (перевірка наявності місць).*

```mermaid
sequenceDiagram
    autonumber
    actor S as Студент
    participant Sys as Система
    participant C as Course
    participant E as Enrollment

    S->>Sys: enroll(Course)
    activate Sys
    Sys->>C: getAvailableSeats()
    activate C
    C-->>Sys: int available
    deactivate C

    alt [available > 0]
        Sys->>E: create(Student, Course)
        activate E
        E-->>Sys: success
        deactivate E
        Sys-->>S: Запис успішний
    else [available == 0]
        Sys-->>S: Помилка: місць немає
    end
    deactivate Sys
```

### 4. Архітектурний стиль та шаблон проєктування

*   **Архітектурний стиль:** **Клієнт-серверна (3-рівнева) архітектура з використанням патерну MVC на сервері**. 
    *   *Обґрунтування:* Це дозволяє чітко розділити інтерфейс користувача (View), бізнес-логіку (Controller) та зберігання даних (Model). Для MVP проєкту це забезпечує швидку розробку, легкість тестування та можливість незалежного оновлення клієнтської частини.
*   **Шаблон проєктування:** **Singleton (Одинак)**.
    *   *Обґрунтування:* У системі буде використовуватися підключення до бази даних. Шаблон Singleton гарантує, що буде створено лише один екземпляр підключення до БД (Database Connection), що дозволить економити ресурси сервера та уникнути конфліктів під час одночасних запитів від різних користувачів.
