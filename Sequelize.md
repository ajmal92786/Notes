## 🔍 What is `Op` in Sequelize?

`Op` stands for **Sequelize Operators** — it’s a set of **comparison and query operators** that Sequelize uses to build SQL queries more expressively and flexibly.

### 📦 How to use it:

You import it like this:

```js
const { Op } = require('sequelize');
```

Then you use it in your query conditions like:

```js
where: {
  rating: { [Op.gt]: 7 },             // rating > 7
  genre: { [Op.iLike]: '%action%' },  // genre ILIKE '%action%'
}
```

### 🧠 Why use `Op`?

Because Sequelize doesn't allow raw SQL like:

```js
where: "rating > 7"
```

Instead, it provides a safe and structured way using `Op`.

### ✅ Common Sequelize Operators (`Op`)

| Operator   | Description              | SQL Equivalent    |
| ---------- | ------------------------ | ----------------- |
| `Op.eq`    | Equal to                 | `=`               |
| `Op.ne`    | Not equal to             | `!=`              |
| `Op.gt`    | Greater than             | `>`               |
| `Op.gte`   | Greater than or equal    | `>=`              |
| `Op.lt`    | Less than                | `<`               |
| `Op.lte`   | Less than or equal       | `<=`              |
| `Op.like`  | Case-sensitive pattern   | `LIKE '%value%'`  |
| `Op.iLike` | Case-insensitive pattern | `ILIKE '%value%'` |
| `Op.in`    | Value in array           | `IN (...)`        |
| `Op.notIn` | Value not in array       | `NOT IN (...)`    |
| `Op.or`    | Logical OR               | `OR`              |
| `Op.and`   | Logical AND              | `AND`             |


### 📌 Example: Search for movies with genre including "action" and rating > 7

```js
const movies = await movieModel.findAll({
  where: {
    genre: { [Op.iLike]: '%action%' },
    rating: { [Op.gt]: 7 }
  }
});
```

---

