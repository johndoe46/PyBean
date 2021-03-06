# PyBean

PyBean is intended as a proof-of-concept of a Python RedBeanPHP implementation 
(see http://www.redbeanphp.com/ for the original concept).

Beans are simple Python objects with a type that maps to the table name
and properties representing the cells of a row.

PyBean is used in development mode (AKA "frozen=False"), where tables and columns are
created on the fly, or in production mode (the default) where schema will not be altered.

The status of PyBean is alpha forever ;-)
Just because I use it in production doesn't mean you should.

## Quick example

    from pybean import Store, SQLiteWriter
    library = Store(SQLiteWriter(":memory:", frozen=False))
    book = library.new("book")
    book.title = "Boost development with pybean"
    book.author = "Charles Xavier"
    library.save(book)
    for book in library.find("book","author like ?",["Charles Xavier"]):
            print book.title
    library.delete(book)
    library.commit()

## Install Pybean

Pybean is available at PyPi. Just type the following:

    pip install pybean

Or if you want to upgrade:

    pip install --upgrade pybean

Pybean is tested under Python 2.7 but other versions may work.    

## Creating a Store

The store is used to save, delete and load beans. You must pass it's constructor a backend,
known as a query writer. Currently, only a SQLite writer is available:

    from pybean import Store, SQLiteWriter
    # "frozen=True" means the SQLiteWriter won't create tables and columns on the fly
    db = Store(SQLiteWriter("/path/to/your/sqlite/database.sqlite", frozen=False))

## Creating a new bean

Once you have a store, you can use it to create new beans:

     book = db.new("book")

Here, we just created a new bean of type "book". The table used to store this type of bean will
be named "book" too. Do not use reserved SQL words as this would break Pybean.

You may now start to assign values to fields:

    book.title = "Tom Sawyer"
    book.author = "Mark Twain"
 
## Saving a bean

Saving a bean is just a matter of using the store's save function:

    db.save(book)

## Finding beans

Use the store's find method:

    # find all books
    for book in db.find("book"):
        print book.title

    # find books where author is Mark Twain
    for book in db.find("book","author like ?",["Mark Twain"]):
        print book.author

find method returns an iterator. If all you want is a single instance, you could use the find_one method:

    my_only_book = db.find("book")

find_one takes the same exact arguments as find and returns either your instance or None    

## Counting beans

This is similar to the find method:

    number_of_books = db.count("book")
    number_of_mark_twain_books = db.count("book", "author like ?", ["Mark Twain"])

## Deleting beans

Use the delete method:

    db.delete(book)

## Welcomed pull request

Things I'm willing to pull if you have it:

 * MySQL writer
 * PostgreSQL writer

