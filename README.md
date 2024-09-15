### Документация по Mircord API на JavaScript: Отправка статистики бота

## Требования

- `Node.js` 16.9.0 или выше
- `discord.js v14`
- `axios` для HTTP-запросов

#### **Пример кода на JavaScpipt**
• Ниже приведён пример отправки статистики бота с использованием JavaScript:

```js
const axios = require('axios');
const { Client, GatewayIntentBits } = require('discord.js');

const apiKey = 'ключ';
const apiURL = 'https://mircord.xyz/api/stats-update';

const client = new Client({
    intents: [GatewayIntentBits.Guilds]
});

client.once('ready', async () => {
    console.log(`Бот ${client.user.tag} успешно запущен!`);

    const serverCount = client.guilds.cache.size;
    const shards = client.shard ? client.shard.count : 1;

    const botStats = {
        servers: serverCount,
        shards: shards
    };

    const headers = {
        Authorization: apiKey
    };

    try {
        const response = await axios.post(apiURL, botStats, { headers });
        switch (response.status) {
            case 200:
                console.log('Статистика успешно обновлена.');
                break;
            default:
                console.log(`Неожиданный ответ: ${response.status}`);
        }
    } catch (error) {
        if (error.response) {
            switch (error.response.status) {
                case 400:
                    console.error('Ошибка 400: API-ключ не найден или недействителен.');
                    break;
                case 429:
                    console.error('Ошибка 429: Превышен лимит запросов.');
                    break;
                default:
                    console.error(`Ошибка: ${error.response.status}, ${error.response.data}`);
            }
        } else {
            console.error('Ошибка при отправке запроса:', error.message);
        }
    }
});

client.login('токен');

```

<h2>Коды ответа</h2>
<p>Возможные ответы от API:</p>
<ul>
  <li><strong>200 OK:</strong> Статистика успешно обновлена.</li>
  <li><strong>400 Bad Request:</strong> API-ключ не найден или недействителен.</li>
  <li><strong>429 Too Many Requests:</strong> Превышен лимит запросов.</li>
</ul>

<h2>Ограничения</h2>
<ul>
  <li>Вы можете отправлять <strong>не более 2 запросов в минуту</strong>. Превышение этого лимита приведёт к ошибке с кодом 429.</li>
</ul>

<h2>Использование</h2>
<p>
<li>Вставьте ваш Discord токен и API-ключ в код.
Замените apiKey и client.login('токен') реальными значениями.
Запустите бота</li></p>
