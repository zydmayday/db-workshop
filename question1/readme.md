## prepare

```sql
CREATE TABLE `orders` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `order_date` DATE NOT NULL,
  `total_amount` INT NOT NULL,
  `price` DECIMAL NOT NULL,
  `user_id` INT NOT NULL,
  PRIMARY KEY (`id`));

-- by default, we have order_date index
CREATE INDEX ix_orders_date ON orders (order_date);
```

```sql
DELIMITER //
CREATE PROCEDURE generate_orders(IN num_rows INT)
BEGIN
  DECLARE i INT DEFAULT 1;
  
  WHILE i <= num_rows DO
    INSERT INTO `leetcode`.`orders` (`order_date`, `total_amount`, `price`, `user_id`)
    VALUES (
      DATE_ADD('2023-01-01', INTERVAL FLOOR(RAND() * 365) DAY),
      FLOOR(RAND() * 1000),
      FLOOR(RAND() * 100) + 50,
      FLOOR(RAND() * 100) + 1
    );
    SET i = i + 1;
  END WHILE;
END //

DELIMITER ;

CALL generate_orders(1000);
```

## requirements

Let's say you have a database that contains a table called orders with millions of rows. You need to retrieve the total sales for each day over the past month, grouped by date.
