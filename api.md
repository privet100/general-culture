## API (Application programming interface, интерфейс прикладной программы)
* инструмент для взаимодействия, интерфейс для программы, который общается с ней на понятном языке
  + по аналогии, кнопка — пользовательский интерфейс
* набор правил, которые позволяют программам взаимодействовать 
* API может быть:
  + у языка программирования
    - чтобы функции взаимодействовали между собой
    - каждая функция выступает «приложением»
    - API = набором инструкций для правильного вызова этих функций
  + у ОС
    - чтобы программы могли извлекать из неё данные и изменять настройки ОС
    - при разработке приложения для Windows, Linux или Android нужно знать API этой системы, чтобы работать с файлами и графикой
  + у веб-сервисов
    - чтобы другие веб-сервисы и программы могли к ним подключаться и работать совместно
    - при разработке программ и веб-сервисов часто нужно обращаться в сторонние сервисы
    - веб-разработчики чаще всего сталкиваются именно с веб-API
* **RESTful API** = тип API, который следует принципам архитектуры передачи репрезентативного состояния
  + сам по себе веб-API можно составить как угодно
  + для удобства разработчиков существует REST API — определённый набор стандартов по составлению API с помощью протокола HTTP
    - позволяет унифицировать программные интерфейсы, сделать их понятными
* OpenAPI
  + спецификация
  + формализует написание API и позволяет автоматически генерировать API и интеграции для работы с большим количеством сторонних приложений
  + когда мы берём сторонний веб-API, его нужно интегрировать в нашу программу => OpenAPI автоматизирует 
* протокол gRCP
  + в основном сейчас для обмена запросами по веб-API используют XML/JSON поверх протокола HTTP 1.1.
  + проблема: она требует чётко фиксированных запросов, которые не очень удобно прописывать в коде
  + кроме того, запросы XML/JSON достаточно тяжёлые и нагружают сеть
* протокол gRCP (?) решает эти прблемы
  + обмениваться более лёгкими запросами
  + составлять для обмена более простые API
  + пока не стал стандартом
  + можно использовать, если подключить специальные библиотеки
* пользоваться API:
  + сторонний разработчик пишет приложение, функцию, операционную систему
  + он составляет API — набор правил, на основе которых другие разработчики могут использовать его разработку
  + API либо лежит в публичном доступе, либо открывается по запросу для клиентов
  + вы вызываете API внутри своего приложения и пользуетесь нужными функциями
  + вам не нужно знать, как работает API-функция, вы вызываете её и получаете результат
* создать API
  + составить набор инструкций, по которым другие приложения смогут обращаться к вашему
* используется, например, чтобы получить информацию, отправить её, изменить, удалить, например:
  + отправка вам курса валют
  + авторизация вас в CRM-системе с возможностью добавлять, удалять и изменять информацию
  + проведение платежа через банковскую систему
  + распознавание голоса и перевод его в текст
  + перевод текста
  + получение информации об актуальных остатках товара
* Ваше приложение отправляет запрос, API формирует ответ 
* позволяет не разрабатывать свои функции, а пользоваться уже разработанными
  + как с библиотеками: вы можете не писать свою функцию сортировки, а подключить библиотеку 
  + например, у вас есть внутренняя CRM-система и вы хотите добавить к ней учёт рабочего времени, можно найти сервис учёта, взять его API и подключить к своей CRM
  + использование API делает приложение безопаснее. Если ваша компания не специализируется на информационной безопасности, вам сложно будет создать безопасный инструмент для платежей или схему авторизации, а разработчики API как раз заняты в первую очередь безопасностью
* вызов API обычно прописан в документации к нему
  + как правило, для вызова нужно отправить данные на сервер, адрес которого задан в виде URL
  + Например, добавить к себе на сайт Яндекс Карту
    - подключаете карту в заголовок вашей страницы `<script src="https://api-maps.yandex.ru/2.1/?apikey=ваш API-ключ&lang=ru_RU" type="text/javascript"></script>`
    - API-ключ нужно получать отдельно, так как API Яндекс Карт закрытый
    - создаёте контейнер, в котором будет карта `<div id="map" style="width: 600px; height: 400px"></div>`
    - добавляете код карты
```
<script type="text/javascript">
  ymaps.ready(init);
  function init() { var myMap = new ymaps.Map("map", {center: [55.76, 37.64], zoom: 7}); }
</script>
```

