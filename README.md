# Library-Management-import sqlite3

# Database connection
conn = sqlite3.connect('library.db')
cursor = conn.cursor()

# Create table
cursor.execute('''CREATE TABLE IF NOT EXISTS books (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    title TEXT NOT NULL,
                    author TEXT NOT NULL,
                    year INTEGER)''')

# Add book
def add_book(title, author, year):
    cursor.execute("INSERT INTO books (title, author, year) VALUES (?, ?, ?)", (title, author, year))
    conn.commit()
    print("Book added successfully!")

# View all books
def view_books():
    cursor.execute("SELECT * FROM books")
    for row in cursor.fetchall():
        print(row)

# Search book
def search_book(title):
    cursor.execute("SELECT * FROM books WHERE title LIKE ?", ('%' + title + '%',))
    results = cursor.fetchall()
    if results:
        for row in results:
            print(row)
    else:
        print("No book found!")

# Delete book
def delete_book(book_id):
    cursor.execute("DELETE FROM books WHERE id=?", (book_id,))
    conn.commit()
    print("Book deleted successfully!")

# Example usage
add_book("Python Basics", "John Doe", 2021)
add_book("Data Structures", "Jane Smith", 2020)

print("\nAll Books:")
view_books()

print("\nSearch Result:")
search_book("Python")

delete_book(1)

print("\nAfter Deletion:")
view_books()

conn.close()
