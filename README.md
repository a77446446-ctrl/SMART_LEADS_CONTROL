# Smart Leads Control

Отдельная центральная панель для реестра клиентских приложений Smart Leads. Здесь хранятся карточки клиентов, ключи подключения в виде хешей, последний технический отчёт и журнал обслуживания. Каждое клиентское приложение работает со своей базой и передаёт агрегированные метрики по HTTPS.

## Развёртывание в Coolify

1. Создайте отдельную PostgreSQL для реестра.
2. Добавьте этот GitHub-репозиторий как приложение с типом сборки **Dockerfile**.
3. Укажите Base Directory `/`, Dockerfile Location `/Dockerfile.operator`, Ports Exposes `3000` и HTTPS-домен.
4. Добавьте только серверные Runtime Variables:

   ```dotenv
   OPERATOR_DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DB
   OPERATOR_PUBLIC_URL=https://control.example.ru
   OPERATOR_ADMIN_KEY=случайный_секрет_не_короче_32_символов
   ```

5. Выполните Deploy и проверьте `/api/health` на своём домене.

Если сборка Dockerfile завершилась ошибкой `open Dockerfile: no such file or directory`, Coolify оставил стандартный путь `/Dockerfile`. В настройках этого приложения укажите **Dockerfile Location** `/Dockerfile.operator`, сохраните и повторите Deploy. При сборке через Railpack панель не запускается: в журнале будет `No start command detected`.

Полный порядок подключения клиента и памятка менеджера: [docs/operator-control.md](docs/operator-control.md).

Клиентская часть находится в отдельном [репозитории Smart-Leads](https://github.com/a77446446-ctrl/Smart-Leads). Ей нужны `SMART_LEADS_CONTROL_URL` и выданный этой панелью `SMART_LEADS_INSTANCE_KEY`.
