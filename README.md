<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Часы для Notion</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            font-size: 20px;
            background: #181818;
            color: #fff;
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }
        .clock {
            margin: 10px;
        }
        select, button {
            margin: 10px;
            padding: 10px;
            font-size: 16px;
        }
    </style>
</head>
<body>

    <h2>Выбери часовые пояса</h2>
    <div>
        <select id="timezoneSelect">
            <option value="1">Милан (UTC+1)</option>
            <option value="-4">Нью-Йорк (UTC-4)</option>
            <option value="0">Лондон (UTC+0)</option>
            <option value="3">Москва (UTC+3)</option>
            <option value="9">Токио (UTC+9)</option>
            <option value="8">Пекин (UTC+8)</option>
            <option value="-7">Лос-Анджелес (UTC-7)</option>
        </select>
        <button onclick="addClock()">Добавить</button>
    </div>

    <div id="clocks"></div>

    <script>
        let timezones = [
            { label: "Милан", offset: 1 },
            { label: "Нью-Йорк", offset: -4 },
            { label: "Токио", offset: 9 }
        ];

        function updateClocks() {
            document.getElementById("clocks").innerHTML = "";
            timezones.forEach((zone, index) => {
                const now = new Date();
                const utc = now.getTime() + now.getTimezoneOffset() * 60000;
                const localTime = new Date(utc + (3600000 * zone.offset));

                const clockDiv = document.createElement("div");
                clockDiv.classList.add("clock");
                clockDiv.innerHTML = `${zone.label}: ${localTime.toLocaleTimeString()} 
                <button onclick="removeClock(${index})">❌</button>`;
                document.getElementById("clocks").appendChild(clockDiv);
            });
        }

        function addClock() {
            if (timezones.length >= 5) {
                alert("Можно добавить максимум 5 часовых поясов!");
                return;
            }
            const select = document.getElementById("timezoneSelect");
            const selectedOption = select.options[select.selectedIndex];
            const offset = parseInt(select.value);
            const label = selectedOption.textContent;

            if (!timezones.some(zone => zone.offset === offset)) {
                timezones.push({ label, offset });
                updateClocks();
            }
        }

        function removeClock(index) {
            timezones.splice(index, 1);
            updateClocks();
        }

        setInterval(updateClocks, 1000);
        updateClocks();
    </script>

</body>
</html>
