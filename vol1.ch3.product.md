## Product

```mysql
CREATE TABLE IF NOT EXISTS product (
	id BINARY(16),
	subtype ENUM('good', 'service') NOT NULL,
	name VARCHAR(255) NOT NULL DEFAULT '',
	introduction_date DATE NOT NULL DEFAULT CURRENT_DATE,
	sales_discontinuation_date DATE NOT NULL DEFAULT '9999-12-31',
	support_discontinuation_date DATE NOT NULL DEFAULT '9999-12-31',
	comment VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS product_category_classification (
	product_id BINARY(16) NOT NULL,
	product_category_id VARCHAR(255) NOT NULL,
	from_date DATE NOT NULL DEFAULT CURRENT_DATE,
	thru_date DATE NOT NULL DEFAULT '9999-12-31',
	primary_flag tinyint(1) NOT NULL DEFAULT 0,
	comment VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (product_id, product_category_id, from_date),
	FOREIGN KEY (product_id) REFERENCES product(id),
	FOREIGN KEY (product_category_id) REFERENCES product_category(id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS product_category (
	id VARCHAR(255),
	description VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS product_category_rollup (
	product_category_id VARCHAR(255) NOT NULL,
	FOREIGN KEY (product_category_id) REFERENCES product_category(id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS market_interest (
	from_date DATE NOT NULL DEFAULT CURRENT_DATE,
	thru_date DATE NOT NULL DEFAULT '9999-12-31',
	product_category_id VARCHAR(255) NOT NULL,
	party_type_id VARCHAR(255) NOT NULL,
	PRIMARY KEY (from_date, product_category_id, party_type_id)
	FOREIGN KEY (party_type_id) REFERENCES party_type(id),
	FOREIGN KEY (product_category_id) REFERENCES product_category(id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS party_type (
	id VARCHAR(255) NOT NULL,
	description VARCHAR(255),
	PRIMARY KEY (id)
) ENGINE=InnoDB CHARACTER SET=utf8mb4 COLLATE=utf8mb4_general_ci;
```