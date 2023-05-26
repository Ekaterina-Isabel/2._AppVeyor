## Project Sample  
[![Build status](https://ci.appveyor.com/api/projects/status/w93bbvad6utb6ehl?svg=true)](https://ci.appveyor.com/project/Ekaterina-Isabel/2-appveyor-pi3i8)


# Домашнее задание к занятию «1.2. Тестирование API, CI»

Задачи этого занятия нужно делать в одном репозитории.

## Задача №1 - Настройка CI

**Что надо сделать:**  
1. взять проект [с лекции](https://github.com/netology-code/aqa-code/tree/master/api-ci/rest)
- целевой сервис (SUT - System under test), расположен в файле [app-mbank.jar](app-mbank.jar), Вам нужно его положить в каталог `artifacts` вашего проекта.
2. настроить для него CI
- зарегистрироваться на AppVeyor
- создать проект
- авторизировать AppVeyor в качестве OAuth App
- выбрать репозиторий
- разместить код Status Badge в файле `README.md` для отображения текущего статуса вашего проекта
3. Удостоверяетесь, что CI показывает, что сборка падает (в процессе сборки будут автоматически прогоняться все авто-тесты).

**Итого:** отправьте на проверку ссылку на гитхаб-репозиторий с вашим проектом.


## Задача №2 - JSON Schema

JSON Schema предлагает нам инструмент валидации JSON-документов.

Как строится схема: 
```js
{
  "$schema": "http://json-schema.org/draft-07/schema", // версия схемы: https://json-schema.org/understanding-json-schema/reference/schema.html
  "type": "array", // тип корневого элемента: https://json-schema.org/understanding-json-schema/reference/type.html
  "items": { // какие элементы допустимы внутри массива: https://json-schema.org/understanding-json-schema/reference/array.html#items
    "type": "object", // должны быть объектами: https://json-schema.org/understanding-json-schema/reference/object.html
    "required": [ // должны содержать следующие поля: https://json-schema.org/understanding-json-schema/reference/object.html#required-properties
      "id",
      "name",
      "number",
      "balance",
      "currency"
    ],
    "additionalProperties": false, // дополнительных полей быть не должно 
    "properties": { // описание полей: https://json-schema.org/understanding-json-schema/reference/object.html#properties
      "id": {
        "type": "integer" // целое число: https://json-schema.org/understanding-json-schema/reference/numeric.html#integer
      },
      "name": {
        "type": "string", // строка: https://json-schema.org/understanding-json-schema/reference/string.html
        "minLength": 1 // минимальная длина - 1: https://json-schema.org/understanding-json-schema/reference/string.html#length
      },
      "number": {
        "type": "string", // строка: https://json-schema.org/understanding-json-schema/reference/string.html
        "pattern": "^•• \\d{4}$" // соответствует регулярному выражению: https://json-schema.org/understanding-json-schema/reference/string.html#regular-expressions
      },
      "balance": {
        "type": "integer" // целое число: https://json-schema.org/understanding-json-schema/reference/numeric.html#integer
      },
      "currency": {
        "type": "string" // строка: https://json-schema.org/understanding-json-schema/reference/string.html
      }
    }
  }
}
```

**Что нужно сделать:**

1. Добавить зависимость

```groovy
dependencies {
    testImplementation 'io.rest-assured:rest-assured:4.3.0'
    testImplementation 'io.rest-assured:json-schema-validator:4.3.0'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.6.1'
}
```

2. Сохраните схему в ресурсах (каталог `resources` в `src/test`) 
3. Включить проверку схемы

Модифицируйте существующий тест так, чтобы он проверял соответствие схеме. Для этого:

```java
      // код теста
      .then()
          .statusCode(200)
          // static import для JsonSchemaValidator.matchesJsonSchemaInClasspath
          .body(matchesJsonSchemaInClasspath("accounts.schema.json"))
      ;
```

Удостоверьтесь, что тесты проходят при соответствии ответа схеме и падают, если вы поменяете что-то в схеме.

4. Доработать схему

Изучите документацию на тип [`object`](https://json-schema.org/understanding-json-schema/reference/object.html) и найдите способ валидации значения поля на два из возможных значения: "RUB" или "USD".

Доработайте схему соответствующим образом, удостоверьтесь, что тесты проходят (в том числе в CI).

Поменяйте "RUB" на "RUR" и удостоверьтесь, что тесты падают (в том числе в CI).

**Итого:** Пришлите на проверку ссылку на ваш репозиторий.  

**Результат выполнения задания:** [ссылка на текущий репозиторий](https://github.com/Ekaterina-Isabel/2._AppVeyor)
