# 🐒 Monkey Flipper - 1v1 Online Mode

Многопользовательский режим "Last Man Standing" для Telegram Mini App игры Monkey Flipper.

## 🎮 Описание

Игроки соревнуются на одинаковых платформах (синхронизированных через seed), побеждает тот, кто продержится дольше или наберёт больше очков за 2 минуты.

## 🚀 Быстрый старт

### Локальная разработка

1. **Клонируйте репозиторий:**
```bash
git clone https://github.com/YOUR_USERNAME/monkey-flipper.git
cd monkey-flipper
```

2. **Установите зависимости:**
```bash
npm install
```

3. **Запустите сервер:**
```bash
npm run server
```

4. **Запустите клиент (в другом терминале):**
```bash
python3 -m http.server 8000
```

5. **Откройте в браузере:**
```
http://localhost:8000?test=1
```

Параметр `?test=1` позволяет открыть игру в нескольких вкладках для тестирования.

## 📦 Деплой на продакшн

### Backend (Render.com)

1. Зайдите на [render.com](https://render.com)
2. Подключите этот GitHub репозиторий
3. Render автоматически обнаружит `render.yaml`
4. Скопируйте URL сервера

### Frontend (Vercel)

1. Обновите `SERVER_URL` в `src/index.js`:
```javascript
const SERVER_URL = 'https://your-render-url.onrender.com';
```

2. Задеплойте на Vercel:
```bash
npm install -g vercel
vercel --prod
```

## 🏗️ Архитектура

### Backend (`server.js`)
- Socket.IO сервер
- Управление комнатами (GameRoom)
- Матчмейкинг (FIFO очередь)
- Валидация игроков

### Frontend (`src/index.js`)
- Phaser 3 игровой движок
- MatchmakingScene - поиск оппонента
- GameScene - игровая сцена с синхронизацией
- SeededRandom - детерминированная генерация платформ

### Синхронизация
- Seed платформ отправляется обоим клиентам
- Обновления игроков каждые 100ms
- Интерполяция призрака оппонента
- Таймер 2 минуты

## 🎯 Условия победы

1. **Падение** - оппонент упал первым
2. **Двойное падение** - побеждает игрок с большим счётом
3. **Таймаут** - победа по очкам после 2 минут

## 📊 Производительность

**Free Tier (Render.com):**
- До 50 одновременных комнат
- Задержка ~100-200ms
- ⚠️ 15 мин сон после неактивности

## 🛠️ Технологии

- **Backend:** Node.js, Express, Socket.IO
- **Frontend:** Phaser 3, Socket.IO Client
- **Деплой:** Render.com (backend), Vercel (frontend)

## 📝 Скрипты

```bash
npm run server    # Запуск сервера с nodemon
npm start         # Запуск сервера
```

## 🔧 Конфигурация

### Переменные окружения (опционально)

```env
PORT=3000
NODE_ENV=production
```

## 🤝 Разработка

### Структура проекта

```
├── server.js           # Socket.IO сервер
├── src/
│   └── index.js        # Phaser игра + Socket.IO клиент
├── assets/             # Спрайты, звуки
├── render.yaml         # Конфиг для Render.com
└── package.json
```

## 📄 Лицензия

MIT

## 👨‍💻 Автор

Artur Sitikov - [@YOUR_TELEGRAM]

---

🐒 **Удачных игр!**
