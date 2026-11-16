# Chess Assistant — Android Overlay App

Нативное Android-приложение на Kotlin. Рисует поверх Chess.com через
`TYPE_APPLICATION_OVERLAY`, анализирует позицию встроенным движком и
подсказывает лучший ход.

---

## Структура проекта

```
chess-assistant/
├── .github/
│   └── workflows/
│       └── build.yml              ← GitHub Actions: сборка + подпись APK
│
├── app/
│   └── src/main/
│       ├── java/com/chessassistant/
│       │   ├── ChessApp.kt                 ← Application class, каналы уведомлений
│       │   ├── ui/
│       │   │   └── MainActivity.kt         ← Экран ключа, запрос overlay-разрешения
│       │   ├── overlay/
│       │   │   └── OverlayService.kt       ← Foreground service, пузырь + панель
│       │   ├── engine/
│       │   │   └── ChessEngine.kt          ← Minimax движок, FEN, подсказки
│       │   └── utils/
│       │       ├── KeyManager.kt           ← EncryptedSharedPreferences, валидация
│       │       └── BootReceiver.kt
│       │
│       ├── res/
│       │   ├── layout/
│       │   │   ├── activity_main.xml       ← Экран входа с ключом
│       │   │   ├── overlay_bubble.xml      ← Плавающий кружок
│       │   │   └── overlay_panel.xml       ← Панель ассистента
│       │   ├── drawable/
│       │   │   ├── bg_chess_grid.xml
│       │   │   └── circle_green.xml
│       │   ├── anim/
│       │   │   └── shake.xml
│       │   ├── values/
│       │   │   ├── colors.xml
│       │   │   ├── strings.xml
│       │   │   └── themes.xml
│       │   └── mipmap-*/
│       │       └── ic_launcher.png         ← ⚠️ ДОБАВЬ ВРУЧНУЮ (см. ниже)
│       │
│       └── AndroidManifest.xml
│
├── build.gradle
├── settings.gradle
├── gradle.properties
└── README.md
```

---

## ⚠️ Assets — добавь вручную

Логотип нужно положить в mipmap-папки самому
(GitHub Actions его не генерирует):

| Папка | Размер | Файл |
|-------|--------|------|
| `res/mipmap-hdpi/`    | 72×72 px  | `ic_launcher.png` |
| `res/mipmap-xhdpi/`   | 96×96 px  | `ic_launcher.png` |
| `res/mipmap-xxhdpi/`  | 144×144 px | `ic_launcher.png` |
| `res/mipmap-xxxhdpi/` | 192×192 px | `ic_launcher.png` |

Для плавающего пузыря используется тот же `@mipmap/ic_launcher`.

---

## Запуск через GitHub Actions

1. Запушь проект в репозиторий на GitHub
2. GitHub Actions запустится автоматически при пуше в `main`
3. Скачай APK: **Actions → последний запуск → Artifacts → chess-assistant-debug**

### Подписанный Release APK

Добавь в **Settings → Secrets → Actions**:
- `KEYSTORE_BASE64` — base64 от твоего `.jks` файла
- `KEY_ALIAS` — алиас ключа
- `KEY_PASSWORD` — пароль ключа
- `STORE_PASSWORD` — пароль хранилища

Создай тег `v1.0.0` → автоматически создастся GitHub Release с APK.

---

## Права приложения

| Разрешение | Зачем |
|-----------|-------|
| `SYSTEM_ALERT_WINDOW` | Overlay поверх Chess.com |
| `FOREGROUND_SERVICE` | Держать оверлей активным |
| `INTERNET` | Опционально: Stockfish API |
| `RECEIVE_BOOT_COMPLETED` | Зарезервировано |

---

## Ключ доступа

```
Unfly---Reborn---Fest8294810Mjk
```

Хранится зашифрованно через `EncryptedSharedPreferences` (AES-256-GCM).

---

## Улучшения (опционально)

- Заменить встроенный движок на **Stockfish через NDK**
- Подключить **lichess.org API** для анализа в облаке
- Добавить **Accessibility Service** для чтения позиции с доски Chess.com
