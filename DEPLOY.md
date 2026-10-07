# 🚀 Развёртывание приложения

## Локальное использование

### 1. **Самый простой способ** (без сервера)
```bash
# Откройте файл в браузере
open index.html
# или
firefox index.html
```

### 2. **С локальным сервером** (рекомендуется)
```bash
# Python 3
python3 -m http.server 8000

# Откройте в браузере
http://localhost:8000
```

## Развёртывание на сервере

### Вариант 1: GitHub Pages

1. **Убедитесь, что репо публичный**
   ```bash
   git remote -v
   ```

2. **Включите GitHub Pages**
   - Перейдите Settings → Pages
   - Branch: `claude/burger-shop-nha-trang-0mbriq`
   - Нажмите Save

3. **Приложение будет доступно по адресу:**
   ```
   https://godoffear.github.io/junkchef_burgers/
   ```

### Вариант 2: Простой веб-сервер (VPS/Hosting)

```bash
# Скопируйте файлы на сервер
scp index.html user@server:/var/www/burgers/

# Или через git
ssh user@server
cd /var/www/burgers
git clone https://github.com/godoffear/junkchef_burgers.git
cd junkchef_burgers
```

### Вариант 3: Docker

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/
COPY demo-data.html /usr/share/nginx/html/
EXPOSE 80
```

```bash
# Сборка
docker build -t burger-shop .

# Запуск
docker run -p 80:80 burger-shop
```

### Вариант 4: Vercel/Netlify

**Vercel:**
```bash
npm i -g vercel
vercel
```

**Netlify:**
- Откройте https://netlify.com
- Drag & drop папку проекта

## Мобильная версия (PWA)

Приложение уже готово к установке как PWA.

### На Android (Chrome)
1. Откройте приложение
2. Нажмите меню (⋮)
3. "Установить приложение"
4. Подтвердите

### На iOS (Safari)
1. Откройте приложение в Safari
2. Нажмите "Поделиться" (↗️)
3. "На экран Домой"
4. Добавьте

## Структура проекта

```
junkchef_burgers/
├── index.html          # 📱 Основное приложение (всё в одном файле)
├── demo-data.html      # 🧪 Заполнение примерами данных
├── README.md           # 📖 Полная документация
├── QUICKSTART.md       # 🚀 Быстрый старт
├── DEPLOY.md           # 🔧 Инструкции по развёртыванию
└── .gitignore          # 🚫 Игнорируемые файлы
```

## Файлы для работы

| Файл | Размер | Назначение |
|------|--------|-----------|
| `index.html` | ~300 KB | Основное приложение (JavaScript встроен) |
| `demo-data.html` | ~6 KB | Заполнение демо-данных |
| `README.md` | ~15 KB | Документация |
| `QUICKSTART.md` | ~12 KB | Руководство пользователя |

**Все остальные файлы опциональные (документация, конфиг).**

## Требования

✅ **Всё работает в браузере**
- Любой современный браузер (Chrome, Firefox, Safari, Edge)
- Интернет не требуется (полностью офлайн)
- Мобильные браузеры поддерживаются

❌ **Не требуется:**
- Node.js
- Python
- Система сборки (webpack, gulp, etc.)
- База данных
- Сервер на Python/Node/PHP

## Резервное копирование данных

Приложение хранит данные в **LocalStorage** браузера.

### Экспорт данных (вручную)
```javascript
// Откройте DevTools (F12) → Console
// Выполните:

const weekKey = new Date().getFullYear() + '-W' + 
  Math.ceil(((new Date() - new Date(new Date().getFullYear(), 0, 1)) / 86400000 + 
  new Date(new Date().getFullYear(), 0, 1).getDay() + 1) / 7);

const data = {
  sales: JSON.parse(localStorage.getItem('burgerShop_week_' + weekKey)),
  inventory: JSON.parse(localStorage.getItem('burgerShop_inventory'))
};

console.log(JSON.stringify(data, null, 2));
// Скопируйте вывод в текстовый файл
```

### Импорт данных
```javascript
// После очистки LocalStorage:
const data = {...}; // ваши сохранённые данные

const weekKey = new Date().getFullYear() + '-W' + 
  Math.ceil(((new Date() - new Date(new Date().getFullYear(), 0, 1)) / 86400000 + 
  new Date(new Date().getFullYear(), 0, 1).getDay() + 1) / 7);

localStorage.setItem('burgerShop_week_' + weekKey, JSON.stringify(data.sales));
localStorage.setItem('burgerShop_inventory', JSON.stringify(data.inventory));

location.reload();
```

## Проблемы и решения

### "Мои данные исчезли!"
- ✅ Данные хранятся в LocalStorage
- ✅ Используйте "Экспорт в PDF" каждый день
- ✅ Периодически вручную сохраняйте JSON данных

### "Работает медленно"
- Попробуйте другой браузер
- Очистите кэш браузера
- Проверьте, не открыто ли слишком много вкладок

### "Кнопка экспорта не работает"
- Проверьте блокировщик рекламы
- Попробуйте Firefox или Chrome
- Убедитесь, что позволяете браузеру скачивать файлы

## Обновление приложения

Просто замените файл `index.html`:

```bash
# Загрузите новую версию
git pull origin claude/burger-shop-nha-trang-0mbriq

# Обновление автоматическое, данные не потеряются
```

## Использование в production

Если запускаете для реального ресторана:

1. **Резервная копия**: Экспортируйте отчёты ежедневно
2. **Обновления**: Тестируйте сначала на тестовом браузере
3. **Данные**: Не полагайтесь только на LocalStorage
4. **Печать**: Распечатывайте PDF отчёты для архива

## Поддержка

Приложение одностраничное (SPA), не требует специального хостинга:
- ✅ Отлично работает на любом веб-сервере
- ✅ Совместимо с GitHub Pages
- ✅ Работает в Vercel, Netlify, AWS S3 + CloudFront
- ✅ Может быть встроено в Electron/Cordova приложение

---

**Вопросы?** Проверьте README.md и QUICKSTART.md
