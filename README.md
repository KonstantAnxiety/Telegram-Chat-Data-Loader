# Загрузка данных из Telegram для аналитики

В Телеграме несколько основных типов чатов:
- Чат 1х1 с пользователем – с такими чатами скрипт не работает
- Группы (чаты с менее чем N участниками) – для таких чатов выгружаются следующие таблицы:
  - chats.csv
  - info.csv
  - messages.csv
  - participants.csv
- Мегагруппы (группы с большим количеством пользователей или выделенным тегом) – для таких чатов выгружается также статистика, подготовленная телеграмом (которую можно также увидеть самому в приложении; некоторые графики могут не подгружаться, потому что у телеграма недостаточно данных для его построение, при этом файл с данными графика не будет сохранен, а в логе появится сообщение вида `Cannot load new_members_by_source_graph: Not enough data to display.`):
  - actions_graph.csv
  - growth_graph.csv
  - languages_graph.csv
  - members_graph.csv
  - messages_graph.csv
  - overview.csv
  - top_admins.csv
- Большие каналы, куда могут писать только админы (broadcast) – для них тоже есть подготовленная телеграмом статистика, но она не поддержана (нет каналов, с которыми можно было бы поэкспериментировать, т.к. нужны права администратора)

Для получения разных данных нужны разные права, так что вместо некоторых файлов может появиться сообщение об ошибке в логе вида `An error occured while trying to load...`

Telegram API и используемая библиотека развиваются, поэтому скрипт нуждается в поддержке, например, скрипт перестал работать с появлением премиума и кастомных эмодзи (которые, к слову, не попадают в итоговые данные, потому что их невозможно сохранить как текст). Чтобы меньше ломаться на подобных вещах, скрипт пропускает все, в поддержке чего нет уверенности.


# Подготовка и настройка
Для запуска скрипта потребуется аккаунт в телеграме и зарегистрированный API ключ ([инструкция](https://core.telegram.org/api/obtaining_api_id#obtaining-api-id)).

После создания ключа понадобятся `api_id` и `api_hash` со [страницы разработчика](https://my.telegram.org/apps).

Сразу за импортами в `run.py` следуют настройки скрипта:
- `_LOGLEVEL: int = logging.INFO` – уровень логгирования
- `_API_ID: int = ...` – полученный `api_id`
- `_API_HASH: str = ...` – полученный `api_hash`
- `_PHONE: str = ...` – телефонный номер аккаунта, от имени которого будет происходить авторизация в телеграме
- `_OUTPUT_DIR: str = './data'` – путь к директории, куда будут сохранены результаты
- `_MESSAGES_AFTER: datetime.datetime` – загружены будут все сообщения **после** этого времени
- `_CHATS_BATCH_SIZE: int = 10` – сколько чатов показывать за раз при выборе чата

При запуске будет необходимо ввести проверочный код и код двухфакторной аутентификации, если он задан.

# Запуск
```shell
pip install -r requirements.txt
python run.py
```
