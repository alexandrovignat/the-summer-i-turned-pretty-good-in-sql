-- Создаем базу
CREATE DATABASE IF NOT EXISTS performance_lab;
USE performance_lab;

-- Создаем таблицу логов (как будто это логи посещений сайта)
CREATE TABLE user_logs (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    action_type VARCHAR(50) NOT NULL,
    product_id INT,
    price DECIMAL(10,2),
    ip_address VARCHAR(45),
    event_date DATETIME NOT NULL,
    -- Индексов пока НЕТ, кроме PRIMARY KEY
    UNIQUE KEY (id) -- это просто для формальности, не считается
);

-- Генерация 1 миллиона строк (запусти и иди пей чай 3 минуты)
DELIMITER $$
CREATE PROCEDURE InsertMillionRows()
BEGIN
    DECLARE i INT DEFAULT 0;
    WHILE i < 1000000 DO
        INSERT INTO user_logs (user_id, action_type, product_id, price, ip_address, event_date)
        VALUES (
            FLOOR(1 + RAND() * 10000),  -- user_id (всего 10к юзеров)
            ELT(1 + FLOOR(RAND() * 4), 'view', 'click', 'purchase', 'add_to_cart'),
            FLOOR(1 + RAND() * 5000),
            ROUND(100 + RAND() * 9900, 2),
            CONCAT(FLOOR(1 + RAND()*255), '.', FLOOR(1 + RAND()*255), '.', FLOOR(1 + RAND()*255), '.', FLOOR(1 + RAND()*255)),
            DATE_SUB(NOW(), INTERVAL FLOOR(RAND() * 365) DAY)
        );
        SET i = i + 1;
    END WHILE;
END$$
DELIMITER ;

-- Запускаем генерацию
CALL InsertMillionRows();
