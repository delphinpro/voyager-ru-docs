# Конфигурации

При установке Voyager вы найдете новый конфигурационный файл, расположенный по адресу `config/voyager.php`.  
В этом файле вы можете найти различные опции для изменения конфигурации вашей установки Voyager.

{% hint style="info" %}
Если вы кэшируете файлы конфигурации, пожалуйста, убедитесь, что запустили `php artisan config:clear` после любых ваших изменений.
{% endhint %}

Ниже мы глубоко погрузимся в конфигурационный файл и дадим детальное описание каждого набора конфигураций.

## Пользователи

```php
<?php

'user' => [
    'add_default_role_on_register' => true,
    'default_role'                 => 'user',
    'admin_permission'             => 'browse_admin',
    'namespace'                    => App\User::class,
    'redirect'                     => '/admin'
],
```

**add\_default\_role\_on\_register:** Specify whether you would like to add the default role to any new user that is created.  
**default\_role:** You can also specify what the **default\_role** is of the user.  
**admin\_permission:** Разрешение, необходимое для просмотра панели администрирования..  
**namespace:** The namespace of your apps User Class.  
**redirect:** Путь перенаправления после входа пользователя в систему.

## Контроллер

```php
<?php

'controllers' => [
    'namespace' => 'TCG\\Voyager\\Http\\Controllers',
],
```

Вы можете определить пространство имен по умолчанию для `controller` Voyager. Если Вы когда-нибудь захотите переопределить любую из основных функциональных возможностей Voyager, то можете сделать это, дублируя контроллеры Voyager и указывая расположение Ваших пользовательских контроллеров.

{% hint style="info" %}
**Переопределение одного контроллера**  
Если вы хотите переопределить только один контроллер, то можете рассмотреть возможность добавления следующего фрагмента кода в метод `register` вашего класса `AppServiceProvider`.  
`$this->app->bind(VoyagerBreadController::class, MyBreadController::class);`
{% endhint %}

## Модель

```php
<?php

'models' => [
    //'namespace' => 'App\\',
],
```

Вы можете указать пространство имён или месторасположение ваших моделей. Это используется при создании моделей из раздела базы данных Voyager. Если не указано, то будет использоваться пространство имён приложения по умолчанию.

## Ассеты

```php
<?php

'assets_path' => '/vendor/tcg/voyager/assets',
```

Возможно, вы захотите указать другой путь ассетов. Если Ваш сайт располагается в подпапке, Вам может понадобиться включить эту директорию в начало пути. Это также может быть использовано в том случае, если вы хотите продублировать опубликованные ассеты и настроить свои собственные.

{% hint style="info" %}
При обновлении до новой версии voyager ассеты, расположенные в каталоге `/vendor/tcg/voyager/assets` могут потребовать перезаписи, поэтому, если вы хотите настроить любые стили, вам нужно продублировать этот каталог и указать новое местоположение вашего каталога asset\_path.
{% endhint %}

## Хранилище \(Storage\)

```php
<?php

'storage' => [
    'disk' => 'public',
],
```

By default Voyager is going to use the `public` local storage. You can additionally use any driver inside of your `config/filesystems.php`. This means you can use S3, Google Cloud Storage, or any other file storage system you would like.

## База данных

```php
<?php

'database' => [
    'tables' => [
        'hidden' => ['migrations', 'data_rows', 'data_types', 'menu_items', 'password_resets', 'permission_role', 'settings'],
    ],
    'autoload_migrations' => true,
],
```

Возможно, вы захотите скрыть некоторые таблицы БД в разделе "База данных" Voyager. В настройках вы можете выбрать, какие таблицы скрыть.  
`autoload_migrations` позволяет исключить из загрузки миграционные файлы Voyager при запуске `php artisan migrate`.

## Многоязычность

```php
<?php

'multilingual' => [
    'enabled' => false,
    'default' => 'en',
    'locales' => [
        'en',
        //'pt',
    ],
],
```

Вы можете указать, хотите ли **включить \(enabled\)** многоязычность. Вы можете определить язык **по умолчанию \(default\)** и все поддерживаемые языки \(**locales**\)

Узнать больше о мультиязычности [здесь](../core-concepts/multilanguage.md).

## Панель управления \(Dashboard\)

```php
<?php

'dashboard' => [
    'navbar_items' => [
        'Profile' => [
            'route'         => 'voyager.profile',
            'classes'       => 'class-full-of-rum',
            'icon_class'    => 'voyager-person',
        ],
        'Home' => [
            'route'         => '/',
            'icon_class'    => 'voyager-home',
            'target_blank'  => true,
        ],
        'Logout' => [
            'route'      => 'voyager.logout',
            'icon_class' => 'voyager-power',
        ],
    ],
    'widgets' => [
        'TCG\\Voyager\\Widgets\\UserDimmer',
        'TCG\\Voyager\\Widgets\\PostDimmer',
        'TCG\\Voyager\\Widgets\\PageDimmer',
    ],
],
```

В настройках панели управления вы можете добавить **navbar\_items**, сделать **data\_tables** отзывчивыми, и управлять вашими **виджетами \(widgets\)**.

**navbar\_items** Включает новый маршрут в выпадающую панель навигации основного пользователя и содержит 'route', 'icon\_class', and 'target\_blank'.

**data\_tables** Если вы установите 'responsive' в true, то datatables станут отзывчивыми.

**widgets** Здесь вы можете управлять виджетами, которые находятся в панели управления. Пример пример класса виджетов можно найти в `tcg/voyager/src/Widgets`.

## Основной цвет

```php
<?php

'primary_color' => '#22A7F0',
```

Основным цветом по умолчанию для панели администратора Voyager является светло-голубой цвет. Вы можете изменить этот основной цвет, изменив значение этой конфигурации.

## Показать советы разработчиков

```php
<?php

'show_dev_tips' => true,
```

У администратора Voyager есть советы или уведомления от разработчиков, которые покажут вам, как ссылаться на определенные значения из Voyager. Вы можете скрыть эти уведомления, установив значение конфигурации в false.

## Дополнительные файлы стилей

```php
<?php

'additional_css' => [
    //'css/custom.css',
],
```

Вы можете добавить свои собственные таблицы стилей, которые будут включены в панель администратора Voyager. Это означает, что вы можете технически создать совершенно новую тему для Voyager, добавив свою собственную таблицу стилей.

Подробнее [здесь](../customization/additional-css-js.md).

{% hint style="info" %}
The path will be passed to Laravels [asset](https://laravel.com/docs/helpers#method-asset) function.
{% endhint %}

## Дополнительные файлы javascript

```php
<?php

'additional_js' => [
    //'js/custom.js',
],
```

То же самое касается и этой конфигурации. Вы можете добавить свой собственный javascript, который будет выполняться в панели администрирования Voyager. Добавьте столько javascript-файлов, сколько необходимо.

Подробнее [здесь](../customization/additional-css-js.md).

## Google Maps

```php
<?php

'googlemaps' => [
    'key'    => env('GOOGLE_MAPS_KEY', ''),
    'center' => [
        'lat' => env('GOOGLE_MAPS_DEFAULT_CENTER_LAT', '32.715738'),
        'lng' => env('GOOGLE_MAPS_DEFAULT_CENTER_LNG', '-117.161084'),
    ],
    'zoom' => env('GOOGLE_MAPS_DEFAULT_ZOOM', 11),
],
```

Существует новый тип данных под названием **coordinates**, который позволяет добавлять карту Google в качестве типа данных. Пользователь может перетащить пин в картах Google, чтобы сохранить значение долготы и широты в базе данных.

In this config you can set the default Google Maps Keys and center location. This can also be added to your .env file.

## Разрешенные Mime-типы

Чтобы разрешить/запретить загрузку файлов с mime-типами в медиа-менеджере можно определить массив `allowed_mimetypes`:

```php
<?php

'allowed_mimetypes' => [
     'image/jpeg',
     'image/png',
     'image/gif',
     'image/bmp',
     'video/mp4',
],
```

Пользователь может загружать файлы только с заданными миметипами. Если вы хотите разрешить загрузку всех типов, вы можете использовать следующее:

```php
<?php

'allowed_mimetypes' => '*',
```

