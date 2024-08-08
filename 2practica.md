Семейное положение

Инструкция по использованию платформы



Вам необходио проверить влияние семейного положения (family_status) на средний доход клиентов avg_income (среднее по столбцуincome) и средний запрашиваемый кредит avg_credit(credit_amount) .

Подсчитайте количество записей для каждого значения в столбце family_status и назовите это количество cnt. Выведите в результате также средний доход клиентов avg_income и средний кредит avg_credit.

Отсортируйте по среднему доходу клиентов avg_income в порядке возрастания.
Используйте таблицу gb_shop.Clusters.


-- Вы работаете с PostgreSQL
-- Введите свой код ниже

-- Подсчитываем количество записей, средний доход и средний кредит для каждого семейного положения
SELECT 
    family_status,               -- Семейное положение
    COUNT(*) AS cnt,             -- Количество записей для каждого значения семейного положения
    AVG(income) AS avg_income,   -- Средний доход клиентов
    AVG(credit_amount) AS avg_credit  -- Средний запрашиваемый кредит
FROM 
    gb_shop.Clusters              -- Таблица с данными
GROUP BY 
    family_status                 -- Группируем по семейному положению
ORDER BY 
    avg_income ASC;               -- Сортируем по среднему доходу в порядке возрастания
###

Продукты категории "Мясо/птица"

Инструкция по использованию платформы



Напишите запрос SQL для подсчета количества продуктов в категории "Мясо/птица". Используйте таблицы gb_shop.Products и gb_shop.Categories.

SELECT count(*) 
FROM gb_shop.Products 
where categoryid in (
SELECT categoryid FROM gb_shop.Categories 
WHERE categoryname = 'Meat/Poultry')

****


Сумма количества товаров

Инструкция по использованию платформы



Выведите товары (название productname), в порядке убывания суммы количества (sum(Quantity), в котором их заказали. Используйте таблицы gb_shop.Products и gb_shop.OrderDetails.



SELECT productname 
FROM gb_shop.Products
WHERE productid IN (
    SELECT productid 
    FROM gb_shop.OrderDetails
    GROUP BY productid
    ORDER BY SUM(Quantity) DESC
);
