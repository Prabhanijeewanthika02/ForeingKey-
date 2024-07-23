# ForeignKey
In MySQL, foreign keys are used to link one table to another. They help maintain the referential integrity of your database by ensuring that the value in a column (or a combination of columns) matches values in a column of another table. Here's a comprehensive guide on how to add foreign keys in MySQL, including creating, altering, and dropping foreign keys.

### 1. Creating Tables with Foreign Keys

When creating tables, you can define foreign keys using the `FOREIGN KEY` constraint. Here’s an example:

```sql
CREATE TABLE parent (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

CREATE TABLE child (
    id INT PRIMARY KEY,
    parent_id INT,
    name VARCHAR(50) NOT NULL,
    FOREIGN KEY (parent_id) REFERENCES parent(id)
);
```

In this example, the `child` table has a foreign key (`parent_id`) that references the `id` column in the `parent` table.

### 2. Adding a Foreign Key to an Existing Table

If you already have tables created and want to add a foreign key constraint, you can use the `ALTER TABLE` statement:

```sql
ALTER TABLE child
ADD CONSTRAINT fk_parent
FOREIGN KEY (parent_id) REFERENCES parent(id);
```

### 3. Dropping a Foreign Key

To drop a foreign key constraint, you need to know its name. You can then use the `ALTER TABLE` statement to drop it:

```sql
ALTER TABLE child
DROP FOREIGN KEY fk_parent;
```

### 4. Listing Foreign Keys

To list all foreign keys in your database, you can query the `information_schema`:

```sql
SELECT table_name, constraint_name, column_name, referenced_table_name, referenced_column_name
FROM information_schema.key_column_usage
WHERE referenced_table_schema = 'your_database_name';
```

Replace `'your_database_name'` with the name of your database.

### 5. Updating Foreign Key Constraints

Updating a foreign key constraint usually involves dropping the existing foreign key and then adding a new one. For example, to change the referenced column:

```sql
ALTER TABLE child
DROP FOREIGN KEY fk_parent;

ALTER TABLE child
ADD CONSTRAINT fk_parent
FOREIGN KEY (parent_id) REFERENCES parent(new_id_column);
```

### 6. Enforcing and Disabling Foreign Key Checks

Sometimes, you may need to temporarily disable foreign key checks (e.g., during bulk inserts). You can do this using:

```sql
SET FOREIGN_KEY_CHECKS = 0;
```

To enable the checks again:

```sql
SET FOREIGN_KEY_CHECKS = 1;
```

### 7. Handling Foreign Key Constraints

- **ON DELETE CASCADE**: Automatically delete rows in the child table when the corresponding row in the parent table is deleted.
- **ON UPDATE CASCADE**: Automatically update the value in the child table when the value in the parent table is updated.
- **ON DELETE SET NULL**: Set the foreign key column in the child table to `NULL` when the corresponding row in the parent table is deleted.
- **ON UPDATE SET NULL**: Set the foreign key column in the child table to `NULL` when the value in the parent table is updated.

Example:

```sql
CREATE TABLE child (
    id INT PRIMARY KEY,
    parent_id INT,
    name VARCHAR(50) NOT NULL,
    FOREIGN KEY (parent_id) REFERENCES parent(id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

### Full Example

Here's a full example demonstrating the creation, modification, and deletion of foreign keys:

```sql
-- Create parent table
CREATE TABLE parent (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

-- Create child table with foreign key
CREATE TABLE child (
    id INT PRIMARY KEY,
    parent_id INT,
    name VARCHAR(50) NOT NULL,
    FOREIGN KEY (parent_id) REFERENCES parent(id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);

-- Add a foreign key to an existing table
ALTER TABLE child
ADD CONSTRAINT fk_parent
FOREIGN KEY (parent_id) REFERENCES parent(id);

-- Drop a foreign key
ALTER TABLE child
DROP FOREIGN KEY fk_parent;

-- Disable foreign key checks
SET FOREIGN_KEY_CHECKS = 0;

-- Enable foreign key checks
SET FOREIGN_KEY_CHECKS = 1;
```

By following these methods, you can effectively manage foreign key constraints in your MySQL database.
