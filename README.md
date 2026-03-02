<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Мировое время - Все страны</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #1e1e2f;
            color: #ffffff;
            text-align: center;
            margin: 0;
            padding: 40px 20px;
        }

        h1 {
            font-size: 2.5em;
            margin-bottom: 20px;
            color: #00d2ff;
        }

        /* Стиль для строки поиска */
        .search-box {
            margin-bottom: 40px;
        }

        #searchInput {
            padding: 15px 25px;
            font-size: 1.1em;
            width: 100%;
            max-width: 500px;
            border-radius: 30px;
            border: 2px solid #00d2ff;
            background: #2a2a40;
            color: #fff;
            outline: none;
            transition: box-shadow 0.3s ease;
        }

        #searchInput:focus {
            box-shadow: 0 0 15px rgba(0, 210, 255, 0.5);
        }

        #searchInput::placeholder {
            color: #8c8c9f;
        }

        .clocks-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            max-width: 1400px;
            margin: 0 auto;
        }

        .clock-card {
            background: #2a2a40;
            border-radius: 15px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.3);
            padding: 25px;
            width: 240px;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .clock-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 24px rgba(0, 210, 255, 0.2);
        }

        .country {
            font-size: 1.4em;
            font-weight: bold;
            color: #00d2ff;
        }

        .capital {
            font-size: 1em;
            color: #a0a0b8;
            margin-bottom: 15px;
        }

        .time {
            font-size: 2.5em;
            font-weight: bold;
            letter-spacing: 2px;
            margin: 10px 0;
        }

        .date {
            font-size: 0.9em;
            color: #8c8c9f;
        }

        /* Скрываем карточки, которые не подходят под поиск */
        .hidden {
            display: none !important;
        }
    </style>
</head>
<body>

    <h1>🌍 Мировое время</h1>
    
    <div class="search-box">
        <input type="text" id="searchInput" placeholder="Введите название страны или столицы...">
    </div>

    <div class="clocks-container" id="clocks">
        </div>

    <script>
        // Расширенный список стран
        const locations = [
            // СНГ и соседи
            { country: 'Казахстан', capital: 'Астана', timezone: 'Asia/Almaty' },
            { country: 'Россия', capital: 'Москва', timezone: 'Europe/Moscow' },
            { country: 'Украина', capital: 'Киев', timezone: 'Europe/Kyiv' },
            { country: 'Беларусь', capital: 'Минск', timezone: 'Europe/Minsk' },
            { country: 'Узбекистан', capital: 'Ташкент', timezone: 'Asia/Tashkent' },
            { country: 'Армения', capital: 'Ереван', timezone: 'Asia/Yerevan' },
            { country: 'Грузия', capital: 'Тбилиси', timezone: 'Asia/Tbilisi' },
            { country: 'Азербайджан', capital: 'Баку', timezone: 'Asia/Baku' },
            
            // Европа
            { country: 'Великобритания', capital: 'Лондон', timezone: 'Europe/London' },
            { country: 'Франция', capital: 'Париж', timezone: 'Europe/Paris' },
            { country: 'Германия', capital: 'Берлин', timezone: 'Europe/Berlin' },
            { country: 'Италия', capital: 'Рим', timezone: 'Europe/Rome' },
            { country: 'Испания', capital: 'Мадрид', timezone: 'Europe/Madrid' },
            { country: 'Польша', capital: 'Варшава', timezone: 'Europe/Warsaw' },
            { country: 'Турция', capital: 'Стамбул', timezone: 'Europe/Istanbul' },
            { country: 'Швеция', capital: 'Стокгольм', timezone: 'Europe/Stockholm' },

            // Азия
            { country: 'Китай', capital: 'Пекин', timezone: 'Asia/Shanghai' },
            { country: 'Япония', capital: 'Токио', timezone: 'Asia/Tokyo' },
            { country: 'Южная Корея', capital: 'Сеул', timezone: 'Asia/Seoul' },
            { country: 'Индия', capital: 'Нью-Дели', timezone: 'Asia/Kolkata' },
            { country: 'ОАЭ', capital: 'Абу-Даби', timezone: 'Asia/Dubai' },
            { country: 'Саудовская Аравия', capital: 'Эр-Рияд', timezone: 'Asia/Riyadh' },
            { country: 'Таиланд', capital: 'Бангкок', timezone: 'Asia/Bangkok' },
            { country: 'Сингапур', capital: 'Сингапур', timezone: 'Asia/Singapore' },
            { country: 'Вьетнам', capital: 'Ханой', timezone: 'Asia/Ho_Chi_Minh' },

            // Америка
            { country: 'США (Восток)', capital: 'Вашингтон', timezone: 'America/New_York' },
            { country: 'США (Запад)', capital: 'Лос-Анджелес', timezone: 'America/Los_Angeles' },
            { country: 'Канада', capital: 'Оттава', timezone: 'America/Toronto' },
            { country: 'Мексика', capital: 'Мехико', timezone: 'America/Mexico_City' },
            { country: 'Бразилия', capital: 'Бразилиа', timezone: 'America/Sao_Paulo' },
            { country: 'Аргентина', capital: 'Буэнос-Айрес', timezone: 'America/Argentina/Buenos_Aires' },
            { country: 'Колумбия', capital: 'Богота', timezone: 'America/Bogota' },
            { country: 'Чили', capital: 'Сантьяго', timezone: 'America/Santiago' },

            // Африка и Океания
            { country: 'Египет', capital: 'Каир', timezone: 'Africa/Cairo' },
            { country: 'ЮАР', capital: 'Кейптаун', timezone: 'Africa/Johannesburg' },
            { country: 'Австралия', capital: 'Канберра', timezone: 'Australia/Sydney' },
            { country: 'Новая Зеландия', capital: 'Веллингтон', timezone: 'Pacific/Auckland' }
        ];

        const container = document.getElementById('clocks');
        const searchInput = document.getElementById('searchInput');
        const clockElements = []; // Здесь будем хранить ссылки на элементы времени, чтобы не перерисовывать всё

        // 1. Функция для создания карточек при загрузке страницы
        function initClocks() {
            // Сортируем по алфавиту по названию страны для удобства
            locations.sort((a, b) => a.country.localeCompare(b.country));

            locations.forEach((loc, index) => {
                const card = document.createElement('div');
                card.className = 'clock-card';
                card.innerHTML = `
                    <div class="country">${loc.country}</div>
                    <div class="capital">${loc.capital}</div>
                    <div class="time" id="time-${index}">--:--:--</div>
                    <div class="date" id="date-${index}">--</div>
                `;
                container.appendChild(card);

                // Сохраняем ссылки на элементы, чтобы быстро обновлять их
                clockElements.push({
                    card: card,
                    timeEl: document.getElementById(`time-${index}`),
                    dateEl: document.getElementById(`date-${index}`),
                    timezone: loc.timezone,
                    searchString: `${loc.country} ${loc.capital}`.toLowerCase()
                });
            });
        }

        // 2. Функция для обновления времени (вызывается каждую секунду)
        function updateTime() {
            const now = new Date();

            clockElements.forEach(item => {
                const timeString = new Intl.DateTimeFormat('ru-RU', {
                    timeZone: item.timezone, hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false
                }).format(now);

                const dateString = new Intl.DateTimeFormat('ru-RU', {
                    timeZone: item.timezone, year: 'numeric', month: 'long', day: 'numeric'
                }).format(now);

                item.timeEl.textContent = timeString;
                item.dateEl.textContent = dateString;
            });
        }

        // 3. Функция для поиска
        searchInput.addEventListener('input', function(e) {
            const searchTerm = e.target.value.toLowerCase();
            
            clockElements.forEach(item => {
                // Если строка поиска пустая ИЛИ название страны/столицы содержит введенный текст
                if (searchTerm === '' || item.searchString.includes(searchTerm)) {
                    item.card.classList.remove('hidden');
                } else {
                    item.card.classList.add('hidden');
                }
            });
        });

        // Запускаем
        initClocks(); // Создаем карточки
        updateTime(); // Ставим правильное время
        setInterval(updateTime, 1000); // Запускаем часы

    </script>
</body>
</html>
