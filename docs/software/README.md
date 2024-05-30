# Реалізація інформаційного та програмного забезпечення

В рамках проекту розробляється: 
- SQL-скрипт для створення на початкового наповнення бази даних
- RESTfull сервіс для управління даними

Ось нова версія коду, що зберігає таку саму функціональність, але має іншу структуру та назви об'єктів:

```sql
-- Створення бази даних і таблиць

CREATE DATABASE IF NOT EXISTS `new_db` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

USE `new_db`;

-- Таблиця історії

CREATE TABLE IF NOT EXISTS `History` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `event_name` VARCHAR(100) NULL,
    `event_time` DATETIME NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB;

-- Таблиця клієнтів

CREATE TABLE IF NOT EXISTS `Client` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `first_name` VARCHAR(50) NULL,
    `last_name` VARCHAR(50) NULL,
    `email` VARCHAR(100) NULL,
    `password_hash` VARCHAR(100) NULL,
    `role_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `email_UNIQUE` (`email`),
    INDEX `fk_Client_Role_idx` (`role_id` ASC),
    CONSTRAINT `fk_Client_Role`
        FOREIGN KEY (`role_id`)
        REFERENCES `Role` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;

-- Таблиця ролей

CREATE TABLE IF NOT EXISTS `Role` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `role_name` VARCHAR(50) NULL,
    `permissions` VARCHAR(100) NULL,
    `role_description` VARCHAR(100) NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB;

-- Таблиця запитів

CREATE TABLE IF NOT EXISTS `Request` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `request_name` VARCHAR(100) NULL,
    `request_time` TIME NULL,
    `request_description` VARCHAR(100) NULL,
    `history_id` INT NOT NULL,
    `client_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    INDEX `fk_Request_History_idx` (`history_id` ASC),
    INDEX `fk_Request_Client_idx` (`client_id` ASC),
    CONSTRAINT `fk_Request_History`
        FOREIGN KEY (`history_id`)
        REFERENCES `History` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
    CONSTRAINT `fk_Request_Client`
        FOREIGN KEY (`client_id`)
        REFERENCES `Client` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;

-- Таблиця повідомлень користувачів

CREATE TABLE IF NOT EXISTS `UserMessage` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `message_text` VARCHAR(250) NOT NULL,
    `message_time` DATETIME NOT NULL,
    `support_request_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    INDEX `fk_UserMessage_SupportRequest_idx` (`support_request_id` ASC),
    CONSTRAINT `fk_UserMessage_SupportRequest`
        FOREIGN KEY (`support_request_id`)
        REFERENCES `SupportRequest` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;

-- Таблиця запитів на підтримку

CREATE TABLE IF NOT EXISTS `SupportRequest` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `is_answered` BOOLEAN,
    `request_type` VARCHAR(50) NOT NULL,
    `client_id` INT NOT NULL,
    `history_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    INDEX `fk_SupportRequest_Client_idx` (`client_id` ASC),
    INDEX `fk_SupportRequest_History_idx` (`history_id` ASC),
    CONSTRAINT `fk_SupportRequest_Client`
        FOREIGN KEY (`client_id`)
        REFERENCES `Client` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
    CONSTRAINT `fk_SupportRequest_History`
        FOREIGN KEY (`history_id`)
        REFERENCES `History` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;

-- Таблиця відповідей адміністратора

CREATE TABLE IF NOT EXISTS `AdminAnswer` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `answer_text` VARCHAR(250) NOT NULL,
    `answer_time` DATETIME NOT NULL,
    `support_request_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    INDEX `fk_AdminAnswer_SupportRequest_idx` (`support_request_id` ASC),
    CONSTRAINT `fk_AdminAnswer_SupportRequest`
        FOREIGN KEY (`support_request_id`)
        REFERENCES `SupportRequest` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;

-- Таблиця завдань

CREATE TABLE IF NOT EXISTS `Task` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `task_name` VARCHAR(255) NOT NULL,
    `deadline` DATETIME NOT NULL,
    `client_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    INDEX `fk_Task_Client_idx` (`client_id` ASC),
    CONSTRAINT `fk_Task_Client`
        FOREIGN KEY (`client_id`)
        REFERENCES `Client` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;

-- Таблиця експорту

CREATE TABLE IF NOT EXISTS `Export` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `is_success` BOOLEAN,
    `export_time` DATETIME NOT NULL,
    `history_id` INT NOT NULL,
    PRIMARY KEY (`id`),
    INDEX `fk_Export_History_idx` (`history_id` ASC),
    CONSTRAINT `fk_Export_History`
        FOREIGN KEY (`history_id`)
        REFERENCES `History` (`id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
) ENGINE=InnoDB;
```



