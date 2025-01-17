# Тестирование функциональности интернет-магазина Altaivita.ru

## Описание проекта
Данный проект посвящен комплексному тестированию функциональности корзины интернет-магазина **Altaivita.ru**.  

Тесты включают:
- API и UI тестирование.
- SQL-запросы для анализа базы данных.
- Смоук-тестирование через **Postman**.  

**Основные технологии**: Python, Selenium, Requests, Postman, pytest, Allure.

Проект выполнен в 4 шага:
1. Создание чек-листа для функциональности поиска.
2. Разработка API и UI тестов корзины.
3. SQL-запросы для анализа базы данных.
4. Автоматизация тестирования.

---

## Шаг 1: Чек-лист функциональности поиска
Был создан чек-лист для тестирования функциональности поиска на сайте.  
Чек-лист сохранен в формате `.xlsx`.

### Основные проверки:
- **Поиск по существующему товару**:
  - Результаты поиска содержат ожидаемые товары.
  - Нет нерелевантных товаров.
- **Поиск по несуществующим запросам**:
  - Система сообщает, что ничего не найдено.
- **Проверка регистра**:
  - Поиск корректен вне зависимости от регистра (например, "мед" и "МЕД").
- **Поиск по ключевым словам**:
  - Отображаются результаты, связанные с введенными словами.

### Краевые случаи:
- **Поиск по пустой строке**:
  - Система уведомляет пользователя о необходимости ввести запрос.
- **Поиск по спецсимволам**:
  - Система уведомляет о некорректности либо ничего не отображает.

### Функциональные проверки:
- **Автодополнение на странице поиска**:
  - Корректность предложенных результатов.
- **Совпадение с категориями**:
  - Возвращаются как товары, так и категории при совпадении.

---

## Шаг 2: API тестирование корзины через Postman
Создана коллекция **Postman** для смоук-тестирования функциональности корзины.  

**Коллекция включает три основных запроса:**
1. **Добавление товара в корзину**:
   - Метод: `POST`.
   - Проверяется корректное добавление товара по его ID.

2. **Изменение количества товара**:
   - Метод: `POST`.
   - Увеличение количества товара в корзине с проверкой.

3. **Удаление товара из корзины**:
   - Метод: `POST`.
   - Удаление товара по ID и проверка работы.

**Коллекция сохранена**: `Cart Smoke Tests.postman_collection.json`

---

## Шаг 3: SQL-запросы для анализа данных
Составлены SQL-запросы для анализа информации о продуктах.  
Файл с запросами находится в `SQL.sql`.

### Примеры SQL-запросов:
-- 1. Все уникальные названия продуктов
SELECT DISTINCT name FROM products;

-- 2. ID, название и цена продуктов с содержанием клетчатки более 5 граммов
SELECT id, name, price
FROM products
WHERE fiber > 5;

-- 3. Название продукта с самым высоким содержанием белка
SELECT name
FROM products
ORDER BY protein DESC
LIMIT 1;

-- 4. Общая сумма калорий по категориям (исключая продукты с нулевым содержанием жира)
SELECT category_id, SUM(calories) AS total_calories
FROM products
WHERE fat > 0
GROUP BY category_id;

-- 5. Средняя цена товаров по категориям
SELECT categories.name AS category_name, AVG(products.price) AS avg_price
FROM products
INNER JOIN categories ON products.category_id = categories.id
GROUP BY categories.name;

## Шаг 4: Автоматическое тестирование корзины

Разработаны автоматические тесты: **2 API-теста** и **2 UI-теста**, которые покрывают ключевую функциональность корзины.

### Используемые технологии:
- **Язык**: Python
- **Библиотеки**: Selenium, Requests, pytest, Allure

### API-тесты:
- **Добавление товара в корзину**:
  - Проверяет успешность добавления товара в корзину.
  - Используется метод `POST`.

- **Изменение количества товара в корзине**:
  - Проверяет корректность увеличения количества товара.

### UI-тесты:
- **Добавление товара в корзину через интерфейс**:
  - Тестируется работа кнопки "Добавить в корзину".
  - Включает проверку сообщения об успешном добавлении.

- **Удаление товара из корзины через интерфейс**:
  - Проверяется удаление товара через корзину.

Код тестов сохранен в папке `tests/`.

---

## Установка и запуск тестов

### Установка зависимостей
Убедитесь, что вы установили все необходимые библиотеки. Запустите следующую команду в терминале:
```bash
pip install -r requirements.txt

### Установка драйвера для Selenium
Для корректной работы UI-тестов убедитесь, что установлен драйвер браузера:

- **Google Chrome**: используйте ChromeDriver.
- **Скачать драйвер**: [ChromeDriver Downloads](https://sites.google.com/chromium.org/driver).
- Поместите драйвер в систему `PATH`.

### Запуск тестов
- **Для запуска всех тестов**:
  ```bash
  pytest

- **Только API-тесты**:
  ```bash
  pytest -m api

- **Только UI-тесты:**:
  ```bash
  pytest -m UI

### Генерация Allure-отчета
- **Для генерации портативного отчета выполните следующие команды:**:
  ```bash
  pytest --alluredir=allure-results
  allure serve allure-results

## Структура проекта

SorokinA_15dec2024/
├── checklists.xlsx                # Чек-лист для поиска
├── cart_smoke_tests.postman_collection.json  # Коллекция Postman
├── Инструкция по работе с коллекцией Postman.docx  # Инструкция
├── SQL.sql                        # SQL-запросы
├── tests/
│   ├── test_api_cart.py           # API тесты
│   ├── test_ui_cart.py            # UI тесты
├── pytest.ini                     # Конфигурация pytest
├── requirements.txt               # Зависимости проекта
└── README.md                      # Документация
