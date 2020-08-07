# Флажки \(Checkbox\)

## Checkbox

```php
{
    "on" : "On Text",
    "off" : "Off Text",
    "checked" : true
}
```

В Voyager флажок преобразуется в тумблер, и, как видно выше, клавиша `on` будет содержать значение, когда тумблер включен, и `off` будет содержать значение, которое устанавливается, когда переключатель выключен. Если `checked` установлен в значение _true_, флажок будет включен; в противном случае по умолчанию он будет выключен.

## Multiple Checkbox

```php
{
    "checked" : true,
    "options": {
        "checkbox1": "Checkbox 1 Text",
        "checkbox2": "Checkbox 2 Text"
    }
}
```

Вы можете создать столько флажков, сколько захотите.

## Radio Button

```php
{
    "default" : "radio1",
    "options" : {
        "radio1": "Radio Button 1 Text",
        "radio2": "Radio Button 2 Text"
    }
}
```

Radio button точно такая же, как и dropdown. Вы можете указать `default`, если она не установлена, а в объекте `options` вы укажете _значение_ опции **слева** и _текст_, который будет отображаться **справа**.

