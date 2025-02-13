* Секретные чаты
  + сквозное шифрование (end-to-end encryption)
  + не являются полностью P2P
  + сообщения передаются через серверы Telegram, однако остаются зашифрованными, серверы не могут прочитать содержимое
    - обеспечение доставки сообщений, даже если участники не в сети одновременно
    - сохранение стабильного соединения через NAT (сетевой адрес трансляции) и файрволлы
* Голосовые и видеозвонки
  + могут использовать P2P-соединение, если это возможно, передача данных происходит напрямую между устройствами участников
  + если прямое соединение невозможно (например, из-за сетевых ограничений), звонки будут проходить через серверы Telegram в зашифрованном виде
* Синхронизация с облаком
  + Если вы используете мессенджеры с локальным шифрованием (например, Signal или WhatsApp), можно отправить важные сообщения из Telegram в другие мессенджеры через пересылку.
* Не рекомендую заходить в свой телеграмм с чужих устройств, так как он иногда после выхода не запрашивает никакой код повторно, а сразу заходит обратно
* создание и управление ботами в Telegram с визуальной настройкой меню
  + BotFather + Telebot / python-telegram-bot + Flowchart Tools
    - схемы меню в виде блок-схем
    - с помощью draw.io* или Lucidchart вы можете визуализировать структуру меню как дерево, его можно использовать для автоматической генерации кода бота, например, через Google Sheets
  + Chatfuel или ManyChat (для простых ботов), визуальные конструкторы для создания чат-ботов с меню и ветвлением
* Flow XO
  + визуальный конструктор для создания ботов
  + позволяет строить сложные меню и логику взаимодействия с пользователем
  + для создания автоматизированных процессов и интеграции с внешними сервисами, в том числе с Google Sheets
* Botpress
  + нужно больше контроля и гибкости
  + имеет визуальный интерфейс для построения диалогов и меню
  + для создания более сложных ботов
  + можно интегрировать различные источники данных, включая Google Sheets
* Tars Landbot
  + платформы для создания ботов с помощью визуальных блоков и конструкторов
* визуализация структуры меню на стороне Google Sheets с помощью скриптов или добавить сторонние инструменты для более удобного представления данных
