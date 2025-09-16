---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')

---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}

small {
    font-size: 0.7em;
}
</style>

Серверна частина на JavaScript
=====

---

# Node.js

Node.js - це середовище виконання JavaScript, яке дозволяє запускати JavaScript на сервері й дозволяє створювати високопродуктивні серверні додатки.

Node.js використовує асинхронну, подієво-орієнтовану модель, що робить його ідеальним для обробки великої кількості одночасних з'єднань.

---

# npm

Також в роботі будемо використовувати npm - це менеджер пакетів для Node.js, який дозволяє встановлювати, оновлювати та керувати бібліотеками та модулями.

npm дозволяє легко додавати сторонні бібліотеки до вашого проекту, що значно спрощує розробку.

Створемо новий проект:

```bash
npm init
```

---

# Express.js

[Express.js](https://www.npmjs.com/package/express) - це популярний веб-фреймворк для Node.js, який спрощує створення веб-додатків і API.

Додамо Express до нашого проекту:

```bash
npm install "express@5"
```

Існує також бібліотека [Express Generator](https://expressjs.com/en/starter/generator.html), яка дозволяє швидко створити проект на express.js.

---

# Структура проекту

package.json - файл конфігурації проекту, який містить інформацію про залежності та скрипти.

src/ - це буде директорія з вихідним кодом проекту.

Для розробки серверу необхідно буде також змінити файл `package.json`, змінивши тип на `module`:

```json
{
  "type": "module",
  ...
}
```
---

# Приклад сервера на Express.js

Cтворимо мінімальний "Hello World!" сервер на Express.js (файл `src/server.js`):

```javascript
import express from 'express';
const app = express()

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(3000, () => {
  console.log(`Server listening on http://localhost:3000`)
})
```

---

Запустимо сервер:

```bash
node src/server.js
```

Тепер можна відкрити браузер і перейти за адресою [http://localhost:3000](http://localhost:3000), де ми побачимо повідомлення "Hello World!".

---
# Nodemon

Для зручності розробки можна використовувати [Nodemon](https://www.npmjs.com/package/nodemon) - це утиліта, яка автоматично перезапускає сервер при зміні файлів.

Додамо Nodemon до нашого проекту:

```bash
npm install --save-dev nodemon
```

І додамо скрипт для запуску сервера з Nodemon до `package.json`:

```json
"scripts": {
  "start": "nodemon src/server.js"
}
```

---
# Обробники маршрутів

Express.js дозволяє легко обробляти різні маршрути (URL) за допомогою методів `app.get()`, `app.post()`, `app.put()`, `app.delete()` та інших.

Наприклад:

```javascript
app.post('/', (req, res) => {
  res.send('Got a POST request')
})
```

---

# Статичні файли

Для обслуговування статичних файлів (HTML, CSS, JS) можна використовувати вбудований обробник `express.static`.

Наприклад, створимо папку `public` з файлами, ми можемо додати наступний код до нашого сервера перед обробкою маршрутів:

```javascript
app.use(express.static('public'));
``` 

---

# Шаблонізатори

Генерувати великі обсяги HTML-коду вручну може бути складно та неефективно. Тому часто використовують шаблонізатори, які дозволяють створювати HTML-шаблони з динамічними даними.

Шаблон — це файл або кілька файлів, написані на певній мові, які в подальшому наповнюються даними та перетворюються на веб сторінку (зазвичай це HTML-файл з різними маркерами, де необхідно вставити дані). 
А шаблонізатор це бібліотека, яка дозволяє на основі таких шаблонів та даних отримати на виході *HTML* сторінку.

---

# Популярні шаблонізатори

Деякі з популярних шаблонізаторів для Node.js:
- [EJS](https://www.npmjs.com/package/ejs) - простий і популярний шаблонізатор, який дозволяє вставляти JavaScript-код у HTML.
- [Pug](https://www.npmjs.com/package/pug) (раніше Jade) - шаблонізатор з чистим синтаксисом, який дозволяє писати HTML без закриваючих тегів.
- [Handlebars](https://www.npmjs.com/package/handlebars) - шаблонізатор, який дозволяє використовувати логіку в шаблонах і має підтримку частин (partials).
- [Nunjucks](https://www.npmjs.com/package/nunjucks) - шаблонізатор, який підтримує розширені можливості, такі як фільтри, макроси та асинхронні функції.

---

# EJS

[Embedded JavaScript (EJS)](https://www.npmjs.com/package/ejs) - це шаблонізатор для Node.js, який дозволить нам зручно генерувати HTML на сервері.

Додамо EJS до нашого проекту:

```bash
npm install ejs
```

І налаштуємо Express для використання EJS як шаблонізатора:

```javascript
app.set('view engine', 'ejs');
app.set('views', './views');
```

---

# Теги EJS

EJS дозволяє використовувати теги для вставки JavaScript-коду в HTML-шаблони. Основні теги:

- `<% %>` - виконує JavaScript-код
- `<%= %>` - виконує JavaScript-код і виводить результат
- `<%- %>` - виконує JavaScript-код і виводить результат без HTML-escaping

---
# Приклад обробки форми логіна

Створимо форму логіна з використанням EJS (файл `views/login.ejs`):

```html
  <%- include('header') -%>
  <form action="/login" method="post">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>
    <br/>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>
    <br/>
    <button type="submit">Login</button>
  </form>
  <%- include('footer') -%>
```

---

# Обробка POST-запиту

Для початку потрібно додати middleware для обробки POST-запитів з форм:

```javascript
app.use(express.urlencoded({ extended: true }));
app.use(express.json()); // на майбутнє, коли будемо працювати з JSON
```

Тепер ми можемо обробити POST-запит на `/login`:

```javascript
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  // Тут можна додати логіку для перевірки логіна і пароля
  res.send(`Logged in as ${username}`);
});
```

---

# Додаткові обробники

Якщо необхідно буде завантажувати файли, можна використовувати [Multer](https://www.npmjs.com/package/multer) - middleware для обробки multipart/form-data, який використовується для завантаження файлів. Або ж [Express Fileupload](https://www.npmjs.com/package/express-fileupload).

Також існує [Body Parser](https://www.npmjs.com/package/body-parser) - що містить додаткові формати та налаштування.

---

# Express valitor

[Express Validator](https://express-validator.github.io/docs) - це набір middleware для валідації та санітизації даних у запитах Express.js.

Додамо Express Validator до нашого проекту:

```bash
npm install express-validator
```

---

Приклад використання Express Validator для валідації даних з форми логіна:

```javascript
import { body, validationResult } from 'express-validator';

const loginValidators = [
  body('username').isLength({ min: 3 }).withMessage('Username must be at least 3 characters long'),
  body('password').isLength({ min: 5 }).withMessage('Password must be at least 5 characters long')
];

app.post('/login', loginValidators, (req, res) => {
  const errors = validationResult(req);
  
  if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });
  
  const { username, password } = req.body;  
  res.send(`Logged in as ${username}`);
});
```

---

# Відображення помилок

Попередній варіант видає помилки в форматі JSON, що не дуже зручно для користувача. Кращим варіантом було б відобразити помилки на сторінці логіна. Для чого ми можемо передати помилки в шаблон EJS:

```javascript
app.post('/login', loginValidators, (req, res) => {
  const errors = validationResult(req);

  if (!errors.isEmpty()) {
    return res.render('login', { errors: errors.array() });
  }
  ...
});
```

---

Тепер для відображення помилок валідації можна використовувати EJS:

```html
<% if (locals.errors && locals.errors.length > 0) { %>
  <div class="errors">
    <ul>
      <% locals.errors.forEach(error => { %>
        <li><%= error.msg %></li>
      <% }) %>
    </ul>
  </div>
<% } %>
```

---

# Передача даних в шаблон

Таким чином при відображенні шаблоку ми можемо передававати будь-які об'єкти, наприклад, помилки валідації,
обʼєкти отримані з бази даних чи будь-які інші динамічні дані, які необхідно відобразити на сторінці:

```javascript
res.render('shop', { 
  items: [ 
    { id: 1, name: 'Phone', price: 500 },
    { id: 2, name: 'Tablet', price: 650 }
  ],
});
```
