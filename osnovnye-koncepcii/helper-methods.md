# Helper methods

Voyager имеет несколько вспомогательных функций, готовых к использованию. Здесь вы можете найти список доступных функций, которые могут ускорить вашу разработку.

## Thumbnails URL

Voyager будет генерировать эскизы для типа поля изображения, когда вы укажете [дополнительные опции поля](../bread/introduction.md#additional-field-options).

После того, как вы сгенерировали эскизы, вы можете захотеть отобразить их на экране или получить URL эскиза. Для того, чтобы сделать это, вам нужно добавить трейт `Resizable` к вашей модели.

```php
use TCG\Voyager\Traits\Resizable;

class Post extends Model
{
    use Resizable;
}
```

### Отобразить одно изображение

```php
@foreach($posts as $post)
    <img src="{{Voyager::image($post->thumbnail('small'))}}" />
@endforeach
```

Или вы можете указать необязательное имя поля изображения \(attribute\), по умолчанию `image`.

```php
@foreach($posts as $post)
    <img src="{{Voyager::image($post->thumbnail('small', 'photo'))}}" />
@endforeach
```

### Отобразить несколько изображений

```php
@foreach($posts as $post)
    $images = json_decode($post->images);
    @foreach($images as $image)
        <img src="{{ Voyager::image($post->getThumbnail($image, 'small')) }}" />
    @endforeach
@endforeach
```

