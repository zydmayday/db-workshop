## prepare

```sql
-- create a cities table
CREATE TABLE cities (
    id INT NOT NULL AUTO_INCREMENT,
    city_name VARCHAR(10) NOT NULL,
    PRIMARY KEY (id)
);

INSERT INTO cities (city_name)
VALUES
('Tokyo'),
('Yokohama'),
('Osaka'),
('Nagoya'),
('Sapporo'),
('Kobe'),
('Kyoto'),
('Fukuoka'),
('Kawasaki'),
('Saitama'),
('Hiroshima'),
('Sendai'),
('Kitakyushu'),
('Chiba'),
('Sagamihara'),
('Shizuoka'),
('Kumamoto'),
('Okayama'),
('Hamamatsu'),
('Nishinomiya'),
('Kagoshima'),
('Kanazawa'),
('Hachioji'),
('Matsuyama'),
('Kashiwa'),
('Nara'),
('Gifu'),
('Toyonaka'),
('Ichikawa'),
('Otsu'),
('Ichinomiya'),
('Kurashiki'),
('Maebashi'),
('Toyama'),
('Oita'),
('Nagasaki'),
('Yokosuka'),
('Akita'),
('Tokushima'),
('Fukui'),
('Asahikawa'),
('Higashiosaka'),
('Fuji'),
('Iwaki'),
('Miyazaki'),
('Oyama'),
('Ebetsu'),
('Kure'),
('Fukaya'),
('Takamatsu'),
('Takasaki'),
('Kumagaya'),
('Suzuka'),
('Tottori'),
('Sasebo'),
('Ageo'),
('Kanoya'),
('Ise');
```

```sql
CREATE PROCEDURE generate_visits_data(IN num_records INT)
BEGIN
    DECLARE i INT DEFAULT 0;
    DECLARE city_count INT;
    DECLARE city_index INT;
    DECLARE city_name VARCHAR(10);
    DECLARE user_id INT;
    DECLARE visit_date DATE;

    SELECT COUNT(*) INTO city_count FROM cities;

    WHILE i < num_records DO
        SET i = i + 1;
        SET city_index = FLOOR(RAND() * city_count) + 1;

        SELECT city INTO city_name FROM cities WHERE id = city_index;
        SET user_id = FLOOR(RAND() * 1000) + 1;
        SET visit_date = DATE_ADD(CURDATE(), INTERVAL -FLOOR(RAND() * 365) DAY);

        INSERT INTO visits(user_id, city, visit_date)
        VALUES(user_id, city_name, visit_date);
    END WHILE;
END;
```

## requirement

we want to know the visits for each city, with a specific date range,
and also should be ordered by the sum of visit times,
note, a user can only be count as once during the period.
finally we will show top 10 in the page.

```sql
SELECT city, COUNT(DISTINCT user_id) as unique_visitors
FROM visits
WHERE visit_date BETWEEN '2022-01-01' AND '2022-12-31'
GROUP BY city
ORDER BY unique_visitors DESC
LIMIT 10;
```

please do sql tuning.
