# MySQL
Лебёдкин Александр Владимирович
Группа № 5451
Решение домашнего задания

-- 1.	Создайте функцию, которая принимает кол-во сек и формат их в кол-во дней, часов, минут и секунд.

-	 Реализация (Основная идея решения основана на использовании функций DIV и MOD на 
-	множестве INT для определения остатков от деления исходного числа последовательно 
-	на 60, 3600 и 86400, позволяющих выделить соответственно секунды, минуты, часы и дни) 
DELIMITER //
CREATE PROCEDURE ConvertSecToDay (num INT)
BEGIN
	CASE
		WHEN num < 60 THEN
			SELECT CONCAT(num, ' ', 'seconds') AS Result;
        WHEN num >= 60 AND num < 3600 THEN
			SELECT CONCAT_WS(' ', num DIV 60, 'minutes', MOD(num, 60), 'seconds') AS Result;
        WHEN num >= 3600 AND num < 86400 THEN
			SELECT CONCAT_WS(' ', num DIV 3600, 'hours', MOD(num, 3600) DIV 60, 'minutes', MOD(MOD(num, 3600),60), 'seconds') AS Result;
        ELSE
			SELECT CONCAT_WS(' ', num DIV 86400, 'days', MOD(num, 86400) DIV 3600, 'hours', MOD(MOD(num, 86400),3600) DIV 60, 'minutes',
                             MOD(MOD(MOD(num, 86400),3600),60), 'seconds') AS Result;
    END CASE;
END//

DELIMITER ;
CALL ConvertSecToDay (123456);  

-- 2.	Выведите только четные числа от 1 до 10 включительно.
-- Реализация (Решение обобщено на поиск четных чисел из любого интервала на множестве INT, --- задаваемым пользователем, поэтому для выяснения позиций исходных чисел применена 
-- проверка индексов на чётность путём целочисленного деления на 2)
DELIMITER //
CREATE PROCEDURE GettingEvenNumbers (`start` INT, `end` INT)
BEGIN
	DECLARE i INT DEFAULT `start`;
    DECLARE Result TEXT DEFAULT NULL;
    WHILE  i<=`end` DO
        IF i%2 = 0 THEN
			IF Result IS NULL THEN
				SET Result = concat(i);
			ELSE
				SET Result concat(Result, ', ', i);
			END IF;
		END IF;
        SET i = i + 1;
    END WHILE;
	SELECT Result;
END //
DELIMITER ;
CALL GettingEvenNumbers (1, 10);
