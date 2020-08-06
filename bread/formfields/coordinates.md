# Coordinates

## Optional Details

Установите их в редактируемом интерфейсе BREAD.

### showLatLngInput / showAutocompleteInput

Установите любое из этих значений в `false`, чтобы не включать этот ввод. По умолчанию оба параметра `true`.

```json
{
  "showAutocompleteInput": false,
  "showLatLngInput": false
}
```

### onChange

```json
{
  "onChange": "myFunction"
}
```

Определяет функцию обратного вызова onChange, чтобы можно было выполнять действия (например, использовать адрес Autocomplete Place для заполнения другого поля) после того, как пользователь изменит любое из полей в этом поле формы.

onChange callback is debounced at 300ms.

Первый параметр — `eventType` ("mapDragged", "latLngChanged", или "placeChanged"). Второй параметр — объект`data`, содержащий свойства `lat`, `lng`, и `place`.

```javascript
function myFunction(eventType, data) {
  console.log('eventType', eventType);
  console.log('data.lat', data.lat);
  console.log('data.lng', data.lng);
  console.log('data.place', data.place);
}
```
