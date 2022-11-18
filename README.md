## Сборник полезных расширений.

[ajax](#ajax) - расширение для работы с AJAX-запросами;

[cursor](#cursor) - расширение для работы с курсором в текстовом поле.

### Установка

Вы можете установить этот плагин в свое приложение CakePHP с помощью [composer](https://getcomposer.org).

Рекомендуемый способ установки пакетов composer - это:

```
composer require valeriy-brunov/extensions
composer dumpautoload
bin/cake plugin load Extensions
```

### <a name="ajax"></a>ajax.js

Выполняет AJAX-запрос по указанному адресу и обрабатывает ответ от сервера.

Для подключения библиотеки используйте:

```js
import Ajax from '../../../../../Extensions/webroot/js/components/ext/ajax.js';
```

Выполнить GET-запрос с минимальным количеством параметров:

```js
Ajax.connect({
    url: 'http://localhost/...',
    success: function( html ) {
        alert('Ответ успешно получен! Полученный код Html: ' + html);
    },
});
```

Перечислим все параметры запроса:

- _url_    - адрес запроса;
- _method_ - метод запроса GET или POST, по умолчанию GET;
- _async_  - как производить запрос асинхронно или синхронно, по умолчанию асинхронно (true);

Типы **передаваемых данных**:

Если ваш запрос не передаёт никакие данные на сервер, то тип данных указывать не требуется (простой запрос).
Если же возникает необходимость передать на сервер данные, то это можно сделать, указав следующие данные:

- _data_ -  данные передоваемые в запросе (строка или объект, обычно содержит данные формы)
            [пример](#d);
- _json_ -  JSON-данные
            [пример](#js);
- _file_ -  выгрузка файла на сервер (по умолчанию false). Для передачи файла(ов) необходимо указать
            объект формы загрузки
            [пример](#f).

Типы **возвращаемых данных**:

- _typeReturn_ - тип возвращаемых данных, содержащихся в ответе сервера:
    html (по умолчанию), json, document (XML).

Параметры, прикреплённые к событиям:

- _beforeSend: function() {}_              - функция срабатывает перед AJAX-запросом;
- _success: function( html ) {}_             - ответ от сервера успешно получен;
- _progress: function( loaded, total ) {}_                - отслеживает загрузку;
- _errorConnect: function() {}_            - срабатывает, если произошла ошибка соединения;
- _error: function( status, statusText ) {}_ - сработывает, если произошла ошибка запроса.

В коде, который отрабатывается (тестируется) и не готов к публикации, параметры "error" и "errorConnect"
указывать не нужно. Это даёт возможность отслеживать ошибки через консоль браузера. В рабочем коде вместо
параметров "error" и "errorConnect" необходимо установить функции-заглушки, как показано в коде ниже:

```js
Ajax.connect({
    url: 'http://localhost/...',
    success: function(html) {
        alert('Ответ успешно получен! Полученный код Html: ' + html);
    },
    error: function( status, statusText ) {},
    errorConnect: function() {},
});
```

<a name="d"></a>Пример для передачи DATA-данных из формы.

```html
<form name="person">
    <input name="name" value="Петя">
    <input name="surname" value="Васечкин">
</form>

<script>
    // Заполним FormData данными из формы:
    let formData = new FormData( document.forms.person );
    // Добавим ещё одно поле:
    formData.append( "middle", "Иванович" );
    Ajax.connect({
        url: 'http://localhost/...',
        data: formData,
        success: function(html) {
            alert('Ответ успешно получен! Полученный код Html: ' + html);
        },
        error: function( status, statusText ) {},
        errorConnect: function() {},
    });
</script>
```

<a name="js"></a>Пример для передачи JSON-данных.

```js
Ajax.connect({
    url: 'http://localhost/...',
    json: {
        name: "Вася",
        surname: "Петров"
    },
    success: function(html) {
        alert('Ответ успешно получен! Полученный код Html: ' + html);
    },
    error: function( status, statusText ) {},
    errorConnect: function() {},
});
```

<a name="f"></a>Пример для передачи файла на сервер.

```html
<input type="file" id="upload-file">

<script>
let f = document.getElementById('upload-file');
f.addEventListener('change', (e) => {
    Ajax.connect({
        url: 'http://localhost/...',
        file: f.files[0],
        progress: fuction( loaded, total ) {
            alert('Загружено байт {$loaded} из {$total}');
        },
        success: function() {
            alert('Файл успешно загружен!');
        },
        error: function( status, statusText ) {},
        errorConnect: function() {},
    });
});
</script>
```

Пример для приёма JSON-данных.

```js
Ajax.connect({
    url: 'http://localhost/...',
    complete: function( request ) {
        // Готово, получили ${request.length} байт.
        // Для ответа в виде JSON: request.response.имя_литерала
    },
    error: function( status, statusText ) {},
    errorConnect: function() {},
});
```

### <a name="cursor"></a>cursor.js





