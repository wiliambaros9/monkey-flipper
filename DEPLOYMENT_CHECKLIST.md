# 🚀 Чеклист деплоя 1v1 режима

## ✅ Что уже готово (локально работает)

- [x] Seeded RNG для синхронизации платформ
- [x] Socket.IO сервер с матчмейкингом
- [x] MatchmakingScene (поиск оппонента)
- [x] Opponent ghost rendering
- [x] Синхронизация позиций (100ms updates)
- [x] UI элементы (таймер, счет)
- [x] Results screen (победа/поражение)
- [x] Обработка дисконнектов

## 📋 Шаги для деплоя на продакшн

### 1. Деплой сервера на Render.com

```bash
# 1. Создайте новый Web Service на https://render.com
# 2. Настройки:
#    - Repository: ваш GitHub репозиторий
#    - Build Command: npm install
#    - Start Command: node server.js
#    - Environment: Node
```

**После деплоя получите URL**, например: `https://monkey-flipper-abc123.onrender.com`

### 2. Обновите SERVER_URL в клиенте

В файле `src/index.js` найдите:

```javascript
const SERVER_URL = window.location.hostname === 'localhost' 
    ? 'http://localhost:3000'
    : '';
```

Замените на:

```javascript
const SERVER_URL = window.location.hostname === 'localhost' 
    ? 'http://localhost:3000'
    : 'https://monkey-flipper-abc123.onrender.com'; // ВАШ URL
```

### 3. Задеплойте клиент на Vercel

```bash
vercel --prod
```

### 4. Проверьте работу

1. Откройте продакшн URL (например: https://monkey-flipper-test-key-1.vercel.app)
2. Нажмите "1v1 Online"
3. Откройте ЕЩЕ ОДНО окно браузера с тем же URL
4. В обоих окнах нажмите "1v1 Online"
5. Они должны соединиться и начать игру!

## 🔍 Как проверить что сервер работает

### Проверка через API

```bash
# Статистика сервера
curl https://monkey-flipper-abc123.onrender.com/api/stats

# Должен вернуть:
# {"queueSize":0,"activeGames":0,"connectedPlayers":0}

# Health check
curl https://monkey-flipper-abc123.onrender.com/api/health

# Должен вернуть:
# {"status":"ok","timestamp":1234567890}
```

### Проверка через DevTools

В браузере откройте DevTools (F12) → Console:

**Успешное подключение:**
```
🔌 Подключение к серверу: https://monkey-flipper-abc123.onrender.com
✅ Подключено к серверу Socket.IO: xA-JRf1vrZPX8fzFAAAB
🔍 Поиск... Игроков в очереди: 1
```

**Успешный матчмейкинг:**
```
🎮 Игра началась! {roomId: "room_...", seed: 123456, opponent: {...}}
🎮 1v1 режим активирован!
   Seed: 123456
   Room: room_1761546109627_abc
   Opponent: Player2
👻 Opponent ghost создан для: Player2
```

## ⚠️ Возможные проблемы

### Проблема: "Connection error!" в MatchmakingScene

**Причина:** Сервер не доступен или неправильный URL

**Решение:**
1. Проверьте что сервер задеплоен и запущен на Render
2. Проверьте `SERVER_URL` в коде
3. Проверьте CORS настройки в `server.js` (должно быть `origin: '*'`)

### Проблема: Сервер "засыпает" на Free tier Render

**Причина:** Free план Render останавливает сервис после 15 минут неактивности

**Решение:**
- Первое подключение займет ~30 секунд (сервер просыпается)
- Для продакшна используйте платный план ($7/месяц)
- Или используйте Railway.app (500 часов бесплатно в месяц)

### Проблема: Игроки не находят друг друга

**Причина:** Два разных сервера или разные URL

**Решение:**
- Убедитесь что `SERVER_URL` одинаковый на обоих клиентах
- Проверьте логи сервера: `heroku logs --tail` или в дашборде Render

## 🎯 Быстрый тест локально

```bash
# Терминал 1: Запустить сервер
node server.js

# Терминал 2: Запустить HTTP сервер
python3 -m http.server 8000

# Открыть ДВА окна браузера:
# Окно 1: http://localhost:8000
# Окно 2: http://localhost:8000 (в режиме инкогнито или другой браузер)

# В обоих нажать: Start → 1v1 Online
```

## 📊 Мониторинг продакшн сервера

### Render Dashboard
- Логи: https://dashboard.render.com → Ваш сервис → Logs
- Метрики: CPU, Memory, Network
- Автоматические рестарты при крашах

### Логи сервера (что смотреть)

**Нормальная работа:**
```
🚀 Monkey Flipper 1v1 Server запущен на порту 3000
✅ Новое подключение: abc123
🔍 Поиск матча: Player1
➕ Игрок добавлен в очередь. Всего в очереди: 1
✅ Новое подключение: def456
🔍 Поиск матча: Player2
🎮 Создана комната: room_123
   Игрок 1: Player1
   Игрок 2: Player2
   Seed: 789012
```

**Окончание игры:**
```
🗑️ Комната room_123 удалена
```

**Проблемы:**
```
❌ Ошибка: [описание ошибки]
🔌 Отключение: abc123
🗑️ Комната удалена из-за дисконнекта
```

## ✨ Готово!

После выполнения всех шагов:
1. ✅ Сервер работает на Render/Railway
2. ✅ Клиент работает на Vercel
3. ✅ Игроки могут найти друг друга
4. ✅ 1v1 режим полностью функционален

**Enjoy your multiplayer game! 🎮🐵**

---

## 🆘 Нужна помощь?

Проверьте:
1. Логи сервера (Render Dashboard → Logs)
2. DevTools Console в браузере (F12)
3. Network tab (проверьте WebSocket соединения)
4. `SERVER_URL` правильный в коде

Удачи! 🚀
