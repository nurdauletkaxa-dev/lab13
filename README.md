  Тема и Цель работы

* **Тема:** Разработка простого веб-приложения для управления коллекцией данных.
* **Цель:** Создание интерактивного интерфейса с использованием **HTML, CSS и чистого JavaScript** для реализации базовых операций **CRUD** (Создание, Чтение, Обновление, Удаление) и дополнительных функций (Поиск, Сортировка) над массивом данных, хранящихся на стороне клиента.

---

 Краткое описание функционала

Веб-приложение представляет собой одностраничный интерфейс, который позволяет пользователю управлять списком товаров.

Реализованные возможности:

1.  **Добавление элемента:** Форма ввода для добавления новых товаров (Название, Цена) в коллекцию.
2.  **Отображение (Чтение):** Все элементы коллекции отображаются в виде интерактивной таблицы.
3.  **Удаление элемента:** Каждая строка таблицы снабжена кнопкой "Удалить" для исключения элемента из коллекции.
4.  **Поиск:** Поле ввода, позволяющее **фильтровать** отображаемые элементы по совпадению с **Названием** товара.
5.  **Сортировка:** Кнопки для сортировки всей коллекции по полям **"Название"** (в алфавитном порядке) и **"Цена"** (по возрастанию/убыванию).

---

 Структура проекта

Проект реализован в виде **одного файла** для упрощения запуска и тестирования.

| Имя файла | Описание |
| :--- | :--- |
| `manager.html` (или `index.html`) | **Основной файл**. Содержит HTML-структуру, CSS-стили (внутри `<style>`) и всю логику JavaScript (внутри `<script>`). |

Технологии:
* **HTML5:** Структура страницы и элементы управления.
* **CSS3:** Базовое оформление и стилизация.
* **JavaScript (ES6+):** Вся логика работы с данными (массив `collection`), управление DOM и обработка событий.

---

 Листинг кода

Данный код представляет собой полный и самодостаточный HTML-файл, который можно открыть в любом браузере.

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Управление коллекцией</title>
    <style>
        /* CSS-стили для базового оформления */
        body { 
            font-family: Arial, sans-serif; 
            margin: 20px; 
            background-color: #f4f4f9; 
            color: #333;
        }
        h1 { 
            color: #2c3e50; 
            text-align: center; 
            margin-bottom: 5px;
        }
        .header-info {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 20px;
        }
        h2 { 
            color: #34495e; 
            margin-top: 0; 
            margin-bottom: 10px;
        }
        table { 
            width: 100%; 
            border-collapse: collapse; 
            margin-top: 20px; 
            box-shadow: 0 2px 5px rgba(0,0,0,0.1); 
            background-color: white; 
        }
        th, td { 
            border: 1px solid #ddd; 
            padding: 12px; 
            text-align: left; 
        }
        th { 
            background-color: #ecf0f1; 
            color: #34495e; 
        }
        .controls { 
            margin-bottom: 30px; 
            padding: 20px; 
            border-radius: 8px; 
            background-color: #ffffff; 
            box-shadow: 0 2px 4px rgba(0,0,0,0.05); 
        }
        .input-group { 
            display: flex; 
            gap: 10px; 
            align-items: center; 
            flex-wrap: wrap; 
        }
        .controls input, .controls button { 
            padding: 10px; 
            border-radius: 4px; 
            border: 1px solid #ccc; 
        }
        .controls button { 
            cursor: pointer; 
            background-color: #3498db; 
            color: white; 
            border: none;
            transition: background-color 0.3s;
        }
        .controls button:hover { 
            background-color: #2980b9; 
        }
        .delete-btn { 
            background-color: #e74c3c; 
            color: white; 
            border: none; 
            padding: 6px 10px; 
            cursor: pointer;
            border-radius: 4px; 
        }
        .delete-btn:hover {
            background-color: #c0392b;
        }
        hr { border: 0; border-top: 1px solid #eee; margin: 15px 0; }
        
        /* Стили для кнопок сортировки */
        .sort-controls {
            display: flex;
            gap: 5px;
            margin-left: 10px; 
        }
        .sort-controls button {
            background-color: #f39c12;
            padding: 10px 15px;
        }
        .sort-controls button.active {
            background-color: #d35400; 
        }
    </style>
</head>
<body>

    <h1>Менеджер коллекции товаров</h1>
    <div class="header-info">Жумантаев Нурдаулет ИС 25-21 ТиПО</div>

    <div class="controls">
        <h2>Добавить товар</h2>
        <div class="input-group">
            <input type="text" id="nameInput" placeholder="Название товара" required>
            <input type="number" id="priceInput" placeholder="Цена" required min="0" step="0.01">
            <button onclick="addItem()">Добавить</button>
        </div>

        <hr>

        <div style="display: flex; justify-content: space-between; align-items: center;">
            <h2 style="margin:0;">Поиск и Сортировка</h2>
            <div class="sort-controls">
                <button id="sortNameBtn" onclick="sortTable('name')">Сортировать по Названию</button>
                <button id="sortPriceBtn" onclick="sortTable('price')">Сортировать по Цене</button>
            </div>
        </div>
        
        <div class="input-group" style="margin-top: 10px;">
            <input type="text" id="searchInput" placeholder="Введите название для поиска" style="flex-grow: 1;">
            <button onclick="searchItem()">Найти</button>
            <button onclick="renderTable()">Сбросить поиск</button>
        </div>
    </div>

    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>Название</th>
                <th>Цена</th>
                <th>Действия</th>
            </tr>
        </thead>
        <tbody id="collectionTableBody">
            </tbody>
    </table>

    <script>
        // 1. Инициализация коллекции данных
        let collection = [
            { id: 1, name: 'Книга "Путешествия"', price: 450.00 },
            { id: 2, name: 'Лэптоп', price: 65000.00 },
            { id: 3, name: 'Кофеварка', price: 3500.50 }
        ];

        let nextId = 4; // Счетчик для присвоения уникального ID
        let sortDirection = {}; 
        let currentSortField = null; 

        // 2. Отображение всех элементов (Read/Отобразить)
        function renderTable(data = collection) {
            const tbody = document.getElementById('collectionTableBody');
            tbody.innerHTML = ''; 

            if (data.length === 0) {
                tbody.innerHTML = '<tr><td colspan="4">Коллекция пуста или по результатам поиска ничего не найдено.</td></tr>';
                return;
            }

            data.forEach(item => {
                const row = tbody.insertRow();

                row.insertCell().textContent = item.id;
                row.insertCell().textContent = item.name;
                row.insertCell().textContent = item.price.toFixed(2) + ' тг'; 

                const actionCell = row.insertCell();
                const deleteBtn = document.createElement('button');
                deleteBtn.textContent = 'Удалить';
                deleteBtn.classList.add('delete-btn');
                deleteBtn.onclick = () => deleteItem(item.id); 
                actionCell.appendChild(deleteBtn);
            });
        }

        // 3. Добавление элемента (Create/Добавить)
        function addItem() {
            const nameInput = document.getElementById('nameInput');
            const priceInput = document.getElementById('priceInput');

            const name = nameInput.value.trim();
            const price = parseFloat(priceInput.value);

            if (name === "" || isNaN(price) || price <= 0) {
                alert("Пожалуйста, введите корректное название и цену (должна быть больше нуля).");
                return;
            }

            const newItem = {
                id: nextId++,
                name: name,
                price: price
            };

            collection.push(newItem);
            renderTable(); 
            
            nameInput.value = '';
            priceInput.value = '';
        }

        // 4. Удаление элемента (Delete/Удалить)
        function deleteItem(id) {
            collection = collection.filter(item => item.id !== id);
            renderTable(); 
        }

        // 5. Поиск элемента (Search/Искать)
        function searchItem() {
            const searchInput = document.getElementById('searchInput');
            const searchTerm = searchInput.value.trim().toLowerCase();

            if (searchTerm === "") {
                renderTable(); 
                return;
            }

            const searchResults = collection.filter(item => 
                item.name.toLowerCase().includes(searchTerm)
            );

            renderTable(searchResults); 
        }

        // 6. Сортировка коллекции (Дополнительно/Сортировать)
        function updateSortButtons(field, direction) {
            document.getElementById('sortNameBtn').classList.remove('active');
            document.getElementById('sortPriceBtn').classList.remove('active');
            
            const btn = document.getElementById(`sort${field.charAt(0).toUpperCase() + field.slice(1)}Btn`);
            btn.classList.add('active');

            let directionText = direction === 'asc' ? ' (▲)' : ' (▼)';
            document.getElementById('sortNameBtn').textContent = 'Сортировать по Названию' + (field === 'name' ? directionText : '');
            document.getElementById('sortPriceBtn').textContent = 'Сортировать по Цене' + (field === 'price' ? directionText : '');
        }

        function sortTable(field) {
            const direction = currentSortField === field && sortDirection[field] === 'asc' ? 'desc' : 'asc';
            sortDirection[field] = direction;
            currentSortField = field;

            collection.sort((a, b) => {
                let comparison = 0;
                
                if (field === 'name') {
                    comparison = a.name.localeCompare(b.name);
                } 
                else if (field === 'price') {
                    comparison = a.price - b.price;
                }
                
                return direction === 'asc' ? comparison : comparison * -1;
            });

            updateSortButtons(field, direction);
            renderTable(); 
        }

        // 7. Запуск при загрузке страницы
        window.onload = function() {
            renderTable();
            document.getElementById('sortNameBtn').textContent = 'Сортировать по Названию';
            document.getElementById('sortPriceBtn').textContent = 'Сортировать по Цене';
        };
    </script>

</body>
</html>
