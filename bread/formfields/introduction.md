# Formfields

Поля формы - очаг системы "Вояджерс" BREAD.  
Каждое поле формы представляет собой поле в вашей базе данных и один вход \(или выход\) в BREAD.  
Чтобы настроить ваши поля формы, вы можете вставить JSON опции, которые описаны на следующих страницах.

У всех полей есть несколько общих опций:

## Описание

Все типы могут включать в себя описание для того, чтобы помочь будущему самому себе или другим пользователям, используя вашу администраторскую панель Voyager, чтобы точно понять, для чего предназначено конкретное поле ввода BREAD, это может быть определено в поле ввода `Optional Details` JSON:

```php
{
    "description": "A helpful description text here for your future self."
}
```

## Опции отображения

Есть также несколько опций, которые вы можете включить, чтобы изменить способ отображения вашего BREAD. Вы можете добавить ключ `display` к вашему объекту json и изменить ширину конкретного поля и даже указать пользовательский идентификатор.

```php
{
    "display": {
        "width": "3",
        "id": "custom_id"
    }
}
```

Ширина отображается на 12-колонной системе сетки. Установка ширины 3 будет составлять 25% от ширины.

С помощью **id** вы можете указать пользовательскую обертку идентификатора вокруг вашего элемента. Например:

```markup
<div id="custom_id">
    <!-- Your field element -->
</div>
```

## Значение по умолчанию

Большинство полей форм позволяют определить значение по умолчанию при добавлении записи:

```php
{
    "default" : "Default text"
}
```

## Пустые значения

Возможно, Вы захотите сохранить поле ввода в БД в виде `null` вместо пустой строки.

Это достаточно просто, внутри BREAD вы можете включить _Optional Details_ для поля:

```php
{
    "null": ""
}
```

Это превратит пустую строку в `null` значение. Однако, Вы можете добавить в базу данных и пустую строку, и нулевое значение для этого поля. Однако, Вы должны выбрать замену для `null` значения, но это может быть все, что Вы захотите. Например, если Вы хотите изменить строку \(например, `Nothing`\) на `null` значение, Вы можете включить следующую _Optional Details_ для этого поля:

```php
{
    "null": "Nothing"
}
```

Теперь ввод `Nothing` в поле закончится как `null` значение в базе данных.

## Генерация Slugs

Используя конструктор BREAD, вы можете автоматически генерировать slugs определенного входа. Допустим, у вас есть несколько сообщений, которые имеют заголовок и slug. Если вы хотите автоматически генерировать slug из атрибута title, вы можете включить следующую _Optional Details_:

```php
{
    "slugify": {
        "origin": "title",
        "forceUpdate": true
    }
}
```

Это автоматически сгенерирует slug из входного поля `title`. Если slug уже существует, то он будет обновляться только в том случае, если установлен параметр `forceUpdate`, по умолчанию он отключен.

## Пользовательское представление

Вы можете указать пользовательское представление, которое будет использоваться для поля формы.  
Для этого Вы должны указать атрибут `view` для желаемого поля:

```text
{
    "view": "my_view"
}
```

Теперь вместо formfield будет загружено `my_view` из `resources/views`.

Вы получаете множество данных, передаваемых в вашем представлении для использования:

* `$view` может быть `browse`, `read`, `edit`, `add` or `order`
* `$content` содержимое этого поля
* `$dataType` тип данных \(DataType\)
* `$dataTypeContent` the whole model-instance
* `$row` the DataRow
* `$options` the DataRow details

{% hint style="info" %}
**Разработка индивидуального поля формы?**  
Если вы разрабатываете пользовательское поле формы и хотите настроить любое из представлений, вы можете сделать это, объединив `view` в `$options` в методе `createContent()` вашего поля формы.
{% endhint %}
