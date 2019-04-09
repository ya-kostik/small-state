# small-state (Черновик)
Маленькое состояние для вашего приложения

Я всегда хотел от состояния несколько вещей:
1. Структуры и простоты — как в Backbone
2. Скорости работы — как с обычными объектами
3. Скорости разработки (мало кода, много дела) — без лишних действий
4. Легкости интеграции в любой проект и фреймворк — например, через декораторы или коннекторы
5. Расширяемости — если чего-то не хватает, добавить это не должно быть проблемой

## Установка
```sh
yarn add small-state
```
или
```sh
npm install small-state
```

## Как использовать?
```js
// Создаем экземпляр хранилища
const user = new State({
  name: 'Петя',
  age: 18,
  email: 'petya@example.com',
  status: 'active'
});

// Подписываемся на изменения
user.on('change:name', (name, oldName) => {
  console.info(name, oldName);
});

// Вносим изменения
user.name = 'Вася';
// Вася, Петя
```

Если у объекта есть вложенные поля, они автоматически обернуться в State.
На дочерние поля можно подписываться через точку:
```js
user.on('payload.activateToken', (token, oldToken) => {
  console.info(name, token, oldToken);
});
```
Всплытие событий немного замедляет работу хранилища, по этому лучше разделять свои модели на несколько слабозависимых узлов.

## Как исплользовать продвинуто?
Для состояния можно задать схему, тогда она будет приводить изменения к нужному типу и валидировать состояние:
```js
const user = new State({
  name: 'Петя',
  age: 18,
  email: 'petya@example.com'
}, {
  name: {
    type: String,
    required: true
  },
  age: Number,
  email: String,
  status: {
    type: String,
    default: 'active'
  }
});
```

### Пример подключения к React
```js
import connect from 'small-state/react';
import user from '../state/user';

function UserName(props) {
  return (
    <h2 className='user__name'>{props.name}</h2>
  )
}

connect(user, (name) => {
  return { name }
}, 'change:name')(UserName)
```

