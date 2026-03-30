---
tags:
  - programming
  - c_programming
  - build_your_own
---

# SQLite

[Build your own SQLite - Codecrafters](https://app.codecrafters.io/courses/sqlite/overview)

#todo own github repo
[Github - t3snake/csqlite](github.com/t3snake/csqlite)

## dot commands

[Official Doc](https://www.sqlite.org/cli.html#special_commands_to_sqlite3_dot_commands)

Started with implementing dot command `.dbinfo`

```
sqlite3 sample.db .dbinfo
```

This outputs the metadata about database file:

```yaml
database page size:  4096
write format:        1
read format:         1
...
number of tables:    5
schema size:         330
data version:        1
```

## Schema Table

[Official Doc](https://www.sqlite.org/schematab.html)

This is a single internal table sqlite uses in each database.
The schema for a database is a description of all other tables, indexes, triggers and views contained in the database.

Schema tables has following columns/properties:
- **type**:
  Table, index, view or trigger
- **name**:
  Name of the object
- **tbl_name**:
  Name of the table or view.
  If type is table or view, then this is a copy of **name** property.
  For other types, it signals the original table.
- **rootpage**:
  Stores the page number of the root b-tree page for tables and indexes.
  For other types this is `0` or `NULL`
- **sql**:
  SQL text that can create the table the row represents.

## Database file

[Official Doc - File Format](https://www.sqlite.org/fileformat.html)

### Database header

[Official Doc - File Format - Database header](https://www.sqlite.org/fileformat.html#the_database_header)

The sqlite database file begins with database header.
The db page size is stored in the header, right after the magic string.
It is a 2-byte, big-endian value.
(Least significant bit is at the end/rightmost. How we would read numbers)

```
// Start of file
53 51 4c 69 74 65 20 66 6f 72 6d 61 74 20 33 00  
// Magic string: "SQLite format 3" + null terminator.
10 00                                            
/* Database page size, in bytes.
Here, the page size is 4096 bytes. */
...
```

The page size must be powers of 2 between `512` and `32768`.
Value `1` represents a page size of `65536`

#implementation
In code, we start reading file, skip 16 bytes and then read the next 2 bytes to get the page size.

### Pages

[Official Doc - File Format - Pages](https://www.sqlite.org/fileformat.html#pages)

The main database file consists of one or more pages.
The size of page is a power of 2 between 512 and 65536 inclusive.
All pages are same size in a database and equal to the 2-byte value at 16 offset in the header.

An empty database would have atleast a single page for the internal schema table.
Any additional tables (and other types) would add more pages.
In some cases each table can span multiple pages.

The first page always has 100 bytes reserved for database header.

`All tables are b-tree pages.`

Pages are numbered beginning with 1.
Max page number is `4294967294` or `2^32 - 2`.
Min size of SQLite Database is a single 512-byte page.
Max size of Database would be `4294967294` pages at `65536` bytes per page which is about `281 TB`.
Limits of underlying file system would typically be reached before reaching own internal size.

Each page has single use which is:
- A b-tree page
	- A table b-tree interior page
	- A table b-tree leaf page
	- An index b-tree interior page
	- An index b-tree leaf page
- A freelist page
	- A freelist trunk page
	- A freelist leaf page
- A payload overflow page
- A pointer map page
- The lock-byte page

### B-tree Pages

[Official Docs - File Format - B-tree Pages](https://www.sqlite.org/fileformat.html#b_tree_pages)

[SQLite File Format representation](https://torymur.github.io/sqlite-repr)

Great explanation walking from binary trees to B-trees:\
[B-Tree Youtube playlist](https://www.youtube.com/playlist?list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6)

In a nutshell, just like how binary trees split the range of numbers into two branches one side less, and the other side greater.
234 Trees and up split into 2, 3 or 4 branches based on number of keys.
In Binary Search Tree there is a single key that splits branch, in 234+ trees, there are multiple keys K which split into K+1 branches.
Each branch is between 2 keys (lesser than bigger key and bigger than smaller key)

SQLite uses B* Trees which means that all the data is stored in leaf nodes.

The very first page in the database contains the database header (100 bytes).
Subsequent pages start directly with B-tree page header

#implementation 
In code to get the number of tables, we look at the first B-Tree header right after 100 byte database header.
To get page size we already moved 16 bytes and then read 2 bytes so the seek has processed 18bytes.
At offset 3 in B-Tree page header, 2 byte int tells us the number of cells in the B-Tree page.
Since the very first B-Tree is the internal schema table, the number of cells gives us the number of tables. 
(Technically we would also get index other than tables, but we assume there are only tables at the moment.)

### Cell Pointer Array

The Cell pointer array comes right after B-Tree Page header.
They are array of 2-byte big-endian values that store the offsets, relative to start of the page.
The array size is equal to the number of cells on the page, which is already read from the page header.

#implementation 
For now I am just using the cell content start address, for multiple records, I might have to use the cell pointer array.

### Record