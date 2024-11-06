# Лабораторная работа №3. Работа с базой данных
Томилинов

## Что делает приложение?
1. Состоит из:
   - Меню activity1
   - Экрана вывода данных из таблицы "Одногруппники" #activity2.
   - Т.к. происходит взаимодействие с БД, был создан отдельный класс `DBHelper` (файл `DBHelper.java`).
2. Создает базу данных и таблицу "Одногруппники" при запуске, если они не существуют.
3. Удаляет все записи и добавляет 5 новых при первом запуске.
4. Выводит данные из таблицы на отдельный экран при нажатии на кнопку "посмотреть".
5. Добавляет новую запись при нажатии на кнопку "добавить".
6. Обновляет последнюю запись при нажатии на кнопку "заменить".
7. При изменении версии базы данных удаляет таблицу «Одногруппники» и создает таблицу «Одногруппники», содержащую больше полей (ветка: task-2).

---
### <a id="activity1"> Меню </a>

Меню состоит из 3 кнопок:
- **"посмотреть"**. По тапу открывается Экран вывода данных из таблицы "Одногруппники" activity2.
- **"добавить"**. По тапу добавляется новая строка в таблицу "Одногруппники":
  - для ветки `master`
    -  ``` java
        btnAddRecord.setOnClickListener(v -> {
            ContentValues contentValues = new ContentValues();
            contentValues.put("FIO", "Новый Одногруппник");
            db.insert("classmates", null, contentValues);
        });
        ```
- **"заменить".** По тапу последняя запись таблицы изменяется на "Иванов Иван Иванович":
  - ``` java
    btnUpdateLastRecord.setOnClickListener(v -> {
            updateLastRecord();
        });
    ```
   - для ветки `master`
     -  ``` java
        private void updateLastRecord() {
          Cursor cursor = db.rawQuery("SELECT * FROM classmates ORDER BY ID DESC LIMIT 1", null);
          if (cursor != null && cursor.moveToFirst()) {
            @SuppressLint("Range") int id = cursor.getInt(cursor.getColumnIndex("ID"));
            ContentValues contentValues = new ContentValues();
            contentValues.put("FIO", "Иванов Иван Иванович");
            db.update("classmates", contentValues, "ID = ?", new String[]{String.valueOf(id)});
          }
          cursor.close();
        }
        ```
---
### <a id="activity2"> Экран вывода данных из таблицы "Одногруппники" </a>
На этом экране также происходит обращение к БД.

#### Для ветки `master`
Таблица "Одногруппники" состоит из полей:
- ID
- ФИО
- время добавления записи
