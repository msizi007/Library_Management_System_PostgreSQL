## Library_Management_System_PostgreSQL
- The Library Management System is a streamlined PostgreSQL database designed to manage books, authors, and member records. It uses a relational schema to connect authors to their works while leveraging PostgreSQL arrays to efficiently track genres and borrowed items without complex join tables. Organized into "sprints," the project covers everything from table creation and data seeding to advanced CRUD operations and filtered queries.

![Library_Management_System_PostgreSQL](https://socialify.git.ci/msizi007/Library_Management_System_PostgreSQL/image?language=1&name=1&owner=1&stargazers=1&theme=Light)

## Getting Started
1. Clone the github project
```bash
git clone https://github.com/msizi007/Library_Management_System_PostgreSQL.git
```
2. Make sure you have installed pgAdmin 4. if you don't have it install check
```bash
https://www.pgadmin.org/download/pgadmin-4-windows/
```

3. Open pgAdmin 4 app go to Servers (1) > PostgreSQL 18
4. Right click and click create database. Give it a name LibraryDB.
5. Open databases and right click LibraryDB and click CREATE Script. You can now follow the sprints.

#### Sprint 1: Project Setup
- Create a new database LibraryDB.
```SQL
CREATE DATABASE LibraryDB;
```

- Create the required tables: Books, Authors, Patrons.

```SQL
CREATE TABLE IF NOT EXISTS Authors(
	id SERIAL PRIMARY KEY,
	name VARCHAR(255),
	nationality VARCHAR(255),
	birth_year INT,
	death_year INT
);


CREATE TABLE IF NOT EXISTS Books(
	id SERIAL PRIMARY KEY,
	title VARCHAR(255),
	genres TEXT[],
	published_year INT,
	available BOOL,
	author_id INT REFERENCES Authors(id)
);


CREATE TABLE IF NOT EXISTS Patrons(
	id SERIAL PRIMARY KEY,
	name VARCHAR(255),
	email VARCHAR(255),
	borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);
```

#### Sprint 2: Insert Data

```SQL
INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);

INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);

INSERT INTO patrons (id, name, email, borrowed_books) VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);
```

#### Sprint 3: Read Operations (Queries)

- Get all books.

```sql
SELECT * FROM Books;
```

- Get a book by title.

```sql
SELECT * FROM Books
WHERE title = 'Brave New World'
```

- Get all books by a specific author.

```sql
SELECT * FROM Books
WHERE author_id = 2
```

- Get all available books.

```sql
SELECT * FROM Books
WHERE available = true
```

#### Sprint 4: Update Operations

- Mark a book as borrowed (set available = false).

```sql
UPDATE Books
SET available = false
WHERE id = 2
```

- Add a new genre to an existing book.

```sql
UPDATE Books
SET genres = array_append(genres, 'Fiction')
WHERE id = 3
```

- Add a borrowed book to a patron’s record.

```sql
UPDATE Patrons
SET borrowed_books = array_append(borrowed_books, 2)
WHERE id = 1
```

#### Sprint 5: Delete Operations

- Delete a book by title.

```sql
DELETE FROM Books
WHERE title = '1984'
```

- Delete an author by ID.

```sql
DELETE FROM Authors
WHERE id = 2
```

#### Sprint 6: Advanced Queries

- Find books published after 1950.

```sql
SELECT * FROM Books
WHERE published_year > 1950
```

- Find all American authors.

```sql
SELECT * FROM Authors
WHERE nationality = 'American'
```

- Set all books as available.

```sql
UPDATE Books
SET available = true
```

- Find all books that are available AND published after 1950.

```sql
SELECT * FROM Books
WHERE available = true AND published_year > 1950
```

- Find authors whose names contain "George".

```sql
SELECT * FROM Authors
WHERE name ILIKE '%George%'
```

- Increment the published year 1869 by 1.

```sql
UPDATE Books
SET published_year = 1870
WHERE published_year = 1869
```
