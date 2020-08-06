# Многоязычность

Voyager поддерживает несколько языков для ваших моделей. Для начала вам необходимо настроить некоторые вещи.

## Настройка

Сначала Вам нужно определить некоторые `locales` в Вашем файле `config/voyager.php` и `enable` мультиязычность:

```php
'multilingual' => [
        'enabled' => true,
        'default' => 'en',
        'locales' => [
            'en',
            'da',
        ],
    ],
```

После этого Вам необходимо включить в модель трейт `Translatable` и определить переводимые атрибуты:

```php
use TCG\Voyager\Traits\Translatable;
class Post extends Model
{
    use Translatable;
    protected $translatable = ['title', 'body'];
}
```

Теперь вы увидите выбор языка в ваших страницах BREAD.

## Использование

### Переводы с большей загрузкой

```php
// Loads all translations
$posts = Post::with('translations')->get();

// Loads all translations
$posts = Post::all();
$posts->load('translations');

// Loads all translations
$posts = Post::withTranslations()->get();

// Loads specific locales translations
$posts = Post::withTranslations(['en', 'da'])->get();

// Loads specific locale translations
$posts = Post::withTranslation('da')->get();

// Loads current locale translations
$posts = Post::withTranslation('da')->get();
```

### Получить значение языка по умолчанию

```php
echo $post->title;
```

### Получить переведенное значение

```php
echo $post->getTranslatedAttribute('title', 'locale', 'fallbackLocale');
```

Если вы не определите локаль, будет использована текущая локаль приложения. Вы можете передать в свою собственную локаль как строку. Если вы не определите fallbackLocale, то будет использована текущая локаль приложения. Вы можете передать свою собственную локаль как строку. Если вы хотите отключить запасную локаль, передайте false. Если для модели не найдено значений для определенного атрибута, либо для локали, либо для запасной локали, то этот атрибут будет равен null.

### Перевести всю модель

```php
$post = $post->translate('locale', 'fallbackLocale');
echo $post->title;
echo $post->body;

// You can also run the `translate` method on the Eloquent collection
// to translate all models in the collection.
$posts = $posts->translate('locale', 'fallbackLocale');
echo $posts[0]->title;
```

Если вы не определите локаль, будет использована текущая локаль приложения. Вы можете передать в свою собственную локаль как строку. Если вы не определите fallbackLocale, то будет использована текущая локаль приложения. Вы можете передать в вашей собственной локали как строку. Если вы хотите отключить запасную локаль, передайте false. Если для модели не найдено значений для конкретного атрибута, либо для локали, либо для fallback, то этот атрибут будет равен null.

### Проверка, что модель является переводимой

```php
// with string
if (Voyager::translatable(Post::class)) {
    // it's translatable
}

// with object of Model or Collection
if (Voyager::translatable($post)) {
    // it's translatable
}
```

### Установить атрибут перевод

```php
$post = $post->translate('da');
$post->title = 'foobar';
$post->save();
```

Это позволит обновить или создать перевод для заголовка сообщения с помощью locale da. Обратите внимание, что если изменённый атрибут не подлежит переводу, то он будет вносить изменения непосредственно в саму модель. Это означает, что атрибут будет перезаписан в языковом наборе по умолчанию.

### Query translatable Models

Для поиска переведенного значения Вы можете использовать метод `whereTranslation`.  
Например, для поиска slug в посте, вы бы использовали

```php
$page = Page::whereTranslation('slug', 'my-translated-slug');
// Is the same as
$page = Page::whereTranslation('slug', '=', 'my-translated-slug');
// Search only locale en, de and the default locale
$page = Page::whereTranslation('slug', '=', 'my-translated-slug', ['en', 'de']);
// Search only locale en and de
$page = Page::whereTranslation('slug', '=', 'my-translated-slug', ['en', 'de'], false);
```

`whereTranslation` принимает следующие параметры:

* `field` поле, в котором вы хотите искать
* `operator` оператор. По умолчани `=`. Также может быть значение \(То же самое, что и у [where](https://laravel.com/docs/queries#where-clauses)\)
* `value` значение, которое вы хотите найти
* `locales` локали, в которых вы хотите искать в виде массива. Оставьте `null`, если вы хотите искать во всех локалях.
* `default` также выполняйте поиск в значении по умолчанию/локале. Значение по умолчанию равно true.

