```SQL> use custom_schema;```

```SQL> show tables;```


| Tables_in_custom_schema               |
|---------------------------------------|
| EntityA                               |
| ManyAToManyBEntity                    |
| EntityB                               |


```
@Getter
@Setter
public class Schema {
	private List<Entity> entities;
}
```


```SQL> desc ManyAToManyBEntity;```


| Field         | Type    | Null | Key | Default | Extra          |
|---------------|---------|------|-----|---------|----------------|
| entity_a_id   | int(11) | NO   | MUL | NULL    |                |
| entity_b_id   | int(11) | NO   | MUL | NULL    |                |


```
@Getter
@Setter
public class Entity {
	private String schemaName;
	private List<Field> fields;
}
```

```
@Getter
@Setter
public class Field {
	private String name;
	private entityName;
	private TypeEnum type;
	private boolean isNull;
	private KeyEnum key;
	private String default;
	private ExtraEnum extras;
	private ReferenceField referenced;
}
```

```
public enum TypeEnum {
	INT,
	VARCHAR,
	CHAR,
	DATE,
	DECIMAL,
	DATETIME,
	...
}
```

```
public enum KeyEnum {
	PRI,
	MUL,
	NOT
}
```


```
public enum ExtraEnum {
	AUTO_INCREMENT,
	...
}
```


```
	SQL> SELECT constraint_schema, table_schema, table_name, column_name, referenced_table_schema, referenced_table_name, referenced_column_name 
		FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE 
		WHERE REFERENCED_TABLE_SCHEMA = "custom_schema" AND TABLE_NAME ="ManyAToManyBEntity";
```	

| constraint_schema | table_schema  | table_name         | column_name   | referenced_table_schema | referenced_table_name | referenced_column_name |
|-------------------|---------------|--------------------|---------------|-------------------------|-----------------------|------------------------|
| custom_schema     | custom_schema | ManyAToManyBEntity | entity_a_id   | custom_schema           | EntityA               | id                     |
| custom_schema     | custom_schema | ManyAToManyBEntity | entity_b_id   | custom_schema           | EntityB               | id                     |


```
@Getter
@Setter
public class ReferenceField {
	private String schemaName;
	private String entityName;
	private String fieldName;
}
```
