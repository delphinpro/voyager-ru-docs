# Установка

### Обычная установка

Voyager очень прост в установке. После создания нового приложения Laravel вы можете включить пакет Voyager следующей командой:

```bash
composer require tcg/voyager
```

Далее убедитесь, что вы создали новую базу данных и добавили учетные данные вашей базы данных в .env файл, а также добавьте URL вашего приложения в переменную `APP_URL`:

```text
APP_URL=http://localhost
DB_HOST=localhost
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

Наконец, мы можем установить Voyager. Вы можете выбрать установку Voyager с фиктивными данными или без них. В состав данных будет входить 1 аккаунт администратора  \(если нет уже существующих пользователей\), 1 демо-страница, 4 демо-записи, 2 категории и 7 настроек.

Чтобы установить Voyager без фиктивных данных просто запустите

```bash
php artisan voyager:install
```

Если вы предпочитаете установить его с фиктивными данными, выполните следующую команду:

```bash
php artisan voyager:install --with-dummy
```

{% hint style="danger" %}
**Ошибка Specified key was too long**  
Если вы видите это сообщение об ошибке у вас устаревшая версия MySQL, воспользуйтесь следующим решением: [https://laravel-news.com/laravel-5-4-key-too-long-error](https://laravel-news.com/laravel-5-4-key-too-long-error)
{% endhint %}

И всё готово!

Запустите локальный сервер разработки командой `php artisan serve` и откройте URL [http://localhost:8000/admin](http://localhost:8000/admin) в вашем браузере.

Если вы установили фальшивые данные, то для вас был создан пользователь со следующими учетными записями:

> **email:** `admin@admin.com`  
> **password:** `password`

{% hint style="info" %}
**Краткая справка**  
Пользователь создается **только**, если в вашей базе данных нет текущих пользователей.
{% endhint %}

Если фиктивный пользователь не был создан, вы можете назначить права администратора существующему пользователю. Это можно легко сделать, выполнив следующую команду:

```bash
php artisan voyager:admin your@email.com
```

Если вы хотите создать нового пользователя с правами администратора, вы можете передать флаг `--create`, как показано ниже.:

```bash
php artisan voyager:admin your@email.com --create
```

Вам будет предложено ввести имя пользователя и пароль.

### Ручная установка

Этот раздел предназначен для пользователей, которые устанавливают Voyager на уже существующую установку Laravel, или для пользователей, которые хотят выполнить установку вручную. Если это не ваш случай, вам следует вернуться к документации [Установка](installation.md) или пропустить этот раздел.

Первое, что вам нужно сделать, это опубликовать ассеты, которые поставляются с Voyager. Вы можете сделать это, выполнив следующие команды.:

```php
php artisan vendor:publish --provider="TCG\Voyager\VoyagerServiceProvider"
```

```bash
php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravelRecent"
```

Далее, выполните команду `php artisan migrate` чтобы создать все таблицы Voyager.

{% hint style="info" %}
Если вы хотите изменить миграции, например, использовать другую таблицу для пользователей, не запускайте миграции. Вместо этого скопируйте файлы миграций Voyager в `database/migrations`, внесите свои изменения, выключите опцию `database.autoload_migrations` в конфигурации и теперь запустите миграции.
{% endhint %}

Теперь откройте вашу User-Model \(обычно `app/User.php`\) и расширьте класс от `\TCG\Voyager\Models\User` вместо `Authenticatable`.

```php
<?php

class User extends \TCG\Voyager\Models\User
{
    // ...
}
```

Следующим шагом является добавление маршрутов Voyager в ваш файл `routes/web.php`:

```php
<?php

Route::group(['prefix' => 'admin'], function () {
    Voyager::routes();
});
```

Теперь запустите

`php artisan db:seed --class=VoyagerDatabaseSeeder`  
для сида необходимых данных в вашу базу данных,  
`php artisan hook:setup`  
для установки системы хуков, и  
`php artisan storage:link`  
чтобы создать сим-линк storage в public.

После этого запустите `composer dump-autoload` для завершения установки!

