```SQL> use custom_schema;```

```SQL> show tables;```


| Tables_in_custom_schema               |
|---------------------------------------|
| EntityA                               |
| ManyAToManyBEntity                    |
| EntityB                               |


```SQL> desc ManyAToManyBEntity;```


| Field         | Type    | Null | Key | Default | Extra          |
|---------------|---------|------|-----|---------|----------------|
| entity_a_id   | int(11) | NO   | MUL | NULL    |                |
| entity_b_id   | int(11) | NO   | MUL | NULL    |                |


```
	SQL> SELECT constraint_schema, table_schema, table_name, column_name, referenced_table_schema, referenced_table_name, referenced_column_name 
		FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE 
		WHERE REFERENCED_TABLE_SCHEMA = "custom_schema" AND TABLE_NAME ="ManyAToManyBEntity";

```	

| constraint_schema | table_schema  | table_name         | column_name   | referenced_table_schema | referenced_table_name | referenced_column_name |
|-------------------|---------------|--------------------|---------------|-------------------------|-----------------------|------------------------|
| custom_schema     | custom_schema | ManyAToManyBEntity | entity_a_id   | custom_schema           | EntityA               | id                     |
| custom_schema     | custom_schema | ManyAToManyBEntity | entity_b_id   | custom_schema           | EntityB               | id                     |


![UML](https://github.com/meyacine/mariadb-reverse-engineering/raw/master/jdl.png "UML Diagram")


```
	entity Schema {
		name String
	}
	entity Entity {
		name String
	}
	entity Field {
		name String
		type TypeEnum
		nullable Boolean
		key KeyEnum
		default String
		extras ExtraEnum
		referenced ReferenceField
	}
	enum TypeEnum {
		TINYINT, //Tiny integer, -128 to 127 signed
		BOOLEAN, //Synonym for TINYINT(1)
		SMALLINT, //Small integer from -32768 to 32767 signed
		MEDIUMINT, //Medium integer from -8388608 to 8388607 signed
		INT, //Integer from -2147483648 to 2147483647 signed
		INTEGER, //Synonym for INT
		BIGINT, // Large integer
		DECIMAL, // A packed "exact" fixed-point number
		DEC, NUMERIC, FIXED, // Synonyms for DECIMAL
		FLOAT, // Single-precision floating-point number
		DOUBLE, // Normal-size (double-precision) floating-point number
		BIT, // Bit field type
		CHAR, // Fixed-length string 1	
		VARCHAR, // Variable-length string
		BINARY, // Fixed-length binary byte string
		BYTE, // Alias for BINARY
		VARBINARY, // Variable-length binary byte string
		TINYBLOBCHAR, // Tiny binary large object up to 255 bytes
		BLOBCHAR, // Binary large object up to 65,535 bytes
		BLOB, //Binary large object data types and the corresponding TEXT types
		MEDIUMBLOB, // Medium binary large object up to 16,777,215 bytes
		LONGBLOB, // Long BLOB holding up to 4GB
		TINYTEXT, // A TEXT column with a maximum length of 255 characters
		TEXT, // A TEXT column with a maximum length of 65,535 characters
		MEDIUMTEXT, // A TEXT column with a maximum length of 16,777,215 characters
		LONGTEXT, // A TEXT column with a maximum length of 4,294,967,295 characters
		ENUM, // Enumeration, or string object that can have one value chosen from a list of values
		SET,
		DATE, // The date type YYYY-MM-DD
		TIME, // Time format HH:MM:SS.ssssss
		DATETIME, // Date and time combination displayed as YYYY-MM-DD HH:MM:SS
		TIMESTAMP, // YYYY-MM-DD HH:MM:DD
		YEAR // A four-digit year
	}
	enum KeyEnum {
		PRI,
		MUL,
		NOT
	}
	enum ExtraEnum {
		AUTO_INCREMENT
	}
	relationship OneToMany {
		Schema{entities} to Entity
	}
	relationship OneToOne {
		Entity{schemaName} to Schema
	}
	relationship OneToMany {
		Entity{fields} to Field
	}
	relationship OneToOne {
		Field{entityName} to Entity
	}
	relationship OneToMany{
		Field{referencingFields} to Field
	}
```
