# Notes

---

# **🔥 Sequelize Associations 🔥**  

## **1️⃣ One-to-One Association (`hasOne` & `belongsTo`)**  
### **🛠 Define Association**  
```javascript
User.hasOne(Profile, { foreignKey: 'userId' }); 
Profile.belongsTo(User, { foreignKey: 'userId' }); 
```
- `User.hasOne(Profile)`: A **User** has **one** Profile.
- `Profile.belongsTo(User)`: A **Profile** belongs to **one** User.

### **🚀 Generated Methods**
| **Method**                | **Description** |
|--------------------------|---------------|
| `user.getProfile()` | Fetches the associated profile for a user. |
| `user.setProfile(profileInstance)` | Associates a profile with a user (replaces existing one if any). |
| `user.hasProfile()` | Checks if the user has an associated profile. |
| `user.createProfile(profileData)` | Creates and associates a new profile for the user. |
| `profile.getUser()` | Fetches the user associated with the profile. |
| `profile.setUser(userInstance)` | Associates a profile with a user. |

### **📌 Example Usage**
```javascript
const user = await User.findByPk(1);
const profile = await user.getProfile(); // Get User's Profile

if (!profile) {
  await user.createProfile({ bio: "Full Stack Developer", website: "example.com" });
}

const newProfile = await Profile.create({ bio: "Updated Bio", website: "new-site.com" });
await user.setProfile(newProfile); // Update User's Profile
```
---

## **2️⃣ One-to-Many Association (`hasMany` & `belongsTo`)**  
### **🛠 Define Association**  
```javascript
Author.hasMany(Book, { foreignKey: 'authorId' });
Book.belongsTo(Author, { foreignKey: 'authorId' });
```
- `Author.hasMany(Book)`: One **Author** can have **many** Books.
- `Book.belongsTo(Author)`: Each **Book** belongs to **one** Author.

### **🚀 Generated Methods**
| **Method**                | **Description** |
|--------------------------|---------------|
| `author.getBooks()` | Fetches all books written by the author. |
| `author.addBook(bookInstance)` | Associates a new book with the author **without removing existing books**. |
| `author.addBooks(booksArray)` | Associates multiple books with the author **without removing existing books**. |
| `author.hasBook(bookInstance)` | Checks if the author is associated with a specific book. |
| `author.hasBooks(booksArray)` | Checks if the author is associated with multiple books. |
| `author.countBooks()` | Counts the total number of books written by the author. |
| `author.removeBook(bookInstance)` | Removes the association of a book from the author. |
| `author.removeBooks(booksArray)` | Removes multiple books from the author. |
| `book.getAuthor()` | Fetches the author of a given book. |
| `book.setAuthor(authorInstance)` | Sets or updates the author for the book. |

### **📌 Example Usage**
```javascript
const author = await Author.create({ name: "J.K. Rowling", birthdate: "1965-07-31", email: "jkrowling@books.com" });

const book1 = await Book.create({ title: "Harry Potter", description: "Magic book", publicationYear: 1997 });
const book2 = await Book.create({ title: "Fantastic Beasts", description: "Magical creatures", publicationYear: 2001 });

await author.addBook(book1);
await author.addBooks([book1, book2]); // Multiple books

const books = await author.getBooks(); // Fetch all books of the author
console.log(books);
```
---

## **3️⃣ Many-to-Many Association (`belongsToMany`)**  
### **🛠 Define Association**  
```javascript
Book.belongsToMany(Genre, { through: 'BookGenres' });
Genre.belongsToMany(Book, { through: 'BookGenres' });
```
- **A Book can belong to multiple Genres** (like Fantasy, Adventure).
- **A Genre can have multiple Books** (Fantasy may include HP, LOTR, GOT).

### **🚀 Generated Methods**
| **Method**                 | **Description** |
|---------------------------|---------------|
| `book.getGenres()` | Fetches all genres associated with a book. |
| `book.setGenres([genre1, genre2])` | Book ke saare old genres remove karke naye genres set karega. |
| `book.addGenre(genreInstance)` | Adds a new genre to the book **without removing existing genres**. |
| `book.addGenres(genresArray)` | Adds multiple genres to the book **without removing existing genres**. |
| `book.hasGenre(genreInstance)` | Checks if the book is associated with a specific genre. |
| `book.hasGenres(genresArray)` | Checks if the book is associated with multiple genres. |
| `book.countGenres()` | Counts the total number of genres linked to a book. |
| `book.removeGenre(genreInstance)` | Removes the association of a specific genre from the book. |
| `book.removeGenres(genresArray)` | Removes multiple genres from the book. |
| `genre.getBooks()` | Fetches all books that belong to a specific genre. |
| `genre.addBook(bookInstance)` | Adds a new book to the genre **without removing existing books**. |
| `genre.addBooks(booksArray)` | Adds multiple books to the genre **without removing existing books**. |
| `genre.hasBook(bookInstance)` | Checks if a book is associated with a specific genre. |
| `genre.hasBooks(booksArray)` | Checks if multiple books are associated with a genre. |
| `genre.countBooks()` | Counts the total number of books in a specific genre. |
| `genre.removeBook(bookInstance)` | Removes the association of a specific book from the genre. |
| `genre.removeBooks(booksArray)` | Removes multiple books from the genre. |

### **📌 Example Usage**
```javascript
const book = await Book.create({ title: "Harry Potter", description: "Magic book", publicationYear: 1997 });
const genre1 = await Genre.create({ name: "Fantasy", description: "Magical and mythical stories." });
const genre2 = await Genre.create({ name: "Adventure", description: "Exciting and risky stories." });

await book.addGenre(genre1);
await book.addGenres([genre1, genre2]); // Multiple genres

const genres = await book.getGenres(); // Fetch all genres of the book
console.log(genres);
```

---

## **🎯 Summary**
✔ **One-to-One** → `User.hasOne(Profile)`  
✔ **One-to-Many** → `Author.hasMany(Book)`  
✔ **Many-to-Many** → `Book.belongsToMany(Genre, { through: 'BookGenres' })`  

---

# File removal from github
 - Agar koi file aapne galti se github pe upload kar di hai like **.env** or **.env.test**, to usko git hub se hatane ki commands niche di gayi hai:
 - Note: in commands ko chalane se pahle apna sara code github upload kar do

```
# 1. Remove it from the repository (not from your local)
git rm --cached .env.test

# 2. Commit the removal
git commit -m "Remove .env.test from version control"

# 3. Rewrite entire git history to remove it from all past commits
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch .env.test" \
  --prune-empty --tag-name-filter cat -- --all

# 4. Force push to GitHub
git push origin --force --all

```
