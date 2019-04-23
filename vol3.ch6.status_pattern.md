## Level 1 Status Pattern.

```mysql
CREATE TABLE IF NOT EXISTS order (
	id BINARY(16),
	description VARCHAR(255) NOT NULL DEFAULT '',
	received_datetime DATETIME NOT NULL DEFAULT '1000-01-01',
	entry_datetime DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
	confirmation_datetime DATETIME NOT NULL DEFAULT '1000-01-01',
	cancelled_datetime DATETIME NOT NULL DEFAULT '9999-12-31',
	opened_from_date DATETIME NOT NULL DEFAULT '1000-01-01',
	closed_thru_date DATETIME NOT NULL DEFAULT '9999-12-31'
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Level 2 Status Pattern

```mysql
CREATE TABLE IF NOT EXISTS status_type (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT ''
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS order (
	id BINARY(16),
	status_type_id INT UNSIGNED NOT NULL DEFAULT 0,
	status_datetime DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
	description VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id),
	FOREIGN KEY (status_type_id) REFERENCES status_type(id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Level 3 Status Pattern

```mysql

CREATE TABLE IF NOT EXISTS status_type (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT ''
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;


CREATE TABLE IF NOT EXISTS order_status (
	id BINARY(16),
	order_id BINARY(16) NOT NULL DEFAULT x'',
	status_type_id INT UNSIGNED NOT NULL DEFAULT 0,
	status_datetime DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP;
	status_from_date DATE NOT NULL DEFAULT '1000-01-01',
	status_thru_date DATE NOT NULL DEFAULT '9999-12-31',
	from_date DATE NOT NULL DEFAULT '1000-01-01',
	thru_date DATE NOT NULL DEFAULT '9999-12-31',
	PRIMARY KEY (id),
	FOREIGN KEY (order_id) REFERENCES order(id),
	FOREIGN KEY (status_type_id) REFERENCES status_type(id),
	UNIQUE (order_id, status_type_id, from_date)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS order (
	id BINARY(16),
	datetime DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
	description VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Level 4 Status Pattern

```mysql
CREATE TABLE IF NOT EXISTS status_type (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT ''
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS order (
	id BINARY(16),
	description VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS shipment (
	id BINARY(16),
	description VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS work_effort (
	id BINARY(16),
	NAME VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_application (
	id BINARY(16),
	order_id BINARY(16) NOT NULL DEFAULT x'',
	shipment_id BINARY(16) NOT NULL DEFAULT x'',
	work_effort_id BINARY(16) NOT NULL DEFAULT x'',
	status_type_id INT UNSIGNED NOT NULL DEFAULT 0,
	status_datetime DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP;
	status_from_date DATE NOT NULL DEFAULT '1000-01-01',
	status_thru_date DATE NOT NULL DEFAULT '9999-12-31',
	from_date DATE NOT NULL DEFAULT '1000-01-01',
	thru_date DATE NOT NULL DEFAULT '9999-12-31',
	PRIMARY KEY (id),
	FOREIGN KEY (order_id) REFERENCES order(id),
	FOREIGN KEY (shipment_id) REFERENCES shipment(id),
	FOREIGN KEY (work_effort_id) REFERENCES work_effort(id),
	FOREIGN KEY (status_type_id) REFERENCES status_type(id),
	UNIQUE (id, order_id, shipment_id, work_effort_id, status_type_id, from_date)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Status Category Pattern

```mysql
CREATE TABLE IF NOT EXISTS status_type (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_category (
	id INT UNSIGNED AUTO_INCREMENT,
	parent_status_type_category_id INT UNSIGNED NOT NULL DEFAULT 0,
	status_type_category_type_id INT UNSIGNED NOT NULL DEFAULT 0,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id),
	FOREIGN KEY (parent_status_type_category_id) REFERENCES status_type_category(id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_category_type (
	id INT UNSIGNED AUTO_INCREMENT,
	parent_status_type_category_type_id INT UNSIGNED NOT NULL DEFAULT 0,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id),
	FOREIGN KEY (parent_status_type_category_type_id) REFERENCES status_type_category_type(id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_category_application (
	id INT UNSIGNED AUTO_INCREMENT,
	status_type_id INT UNSIGNED NOT NULL DEFAULT 0,
	status_type_category_id INT UNSIGNED NOT NULL DEFAULT 0,
	from_date DATE NOT NULL DEFAULT '1000-01-01',
	thru_date DATE NOT NULL DEFAULT '9999-12-31',
	PRIMARY KEY (id),
	FOREIGN KEY (status_type_id) REFERENCES status_type(id),
	FOREIGN KEY (status_type_category_id) REFERENCES status_type_category(id),
	UNIQUE (status_type_id, status_type_category_id, from_date)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Status Type with Multi Rollup and Rules Pattern

```mysql
CREATE TABLE IF NOT EXISTS status_type (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_category (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_association_rule (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_association_type (
	id INT UNSIGNED AUTO_INCREMENT,
	name VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS status_type_association (
	id BINARY(16),
	from_status_type_id INT UNSIGNED NOT NULL,
	to_status_type_id INT UNSIGNED NOT NULL,
	status_type_association_rule_id INT UNSIGNED NOT NULL,
	status_type_association_type_id INT UNSIGNED NOT NULL,
	status_type_category_id INT UNSIGNED NOT NULL,
	from_date DATE NOT NULL DEFAULT '1000-01-01',
	thru_date DATE NOT NULL DEFAULT '9999-12-31',
	PRIMARY KEY (id),
	FOREIGN KEY (from_status_type_id) REFERENCES status_type(id),
	FOREIGN KEY (to_status_type_id) REFERENCES status_type(id),
	FOREIGN KEY (status_type_association_rule_id) REFERENCES status_type_association_rule(id),
	FOREIGN KEY (status_type_association_type_id) REFERENCES status_type_association_type(id),
	FOREIGN KEY (status_type_category_id) REFERENCES status_type_category(id),
	UNIQUE (from_status_type_id, to_status_type_id, status_type_association_rule_id, status_type_association_type_id, status_type_category_id, from_date)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

