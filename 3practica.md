Расчет среднего чека для заказов

Инструкция по использованию платформы



Вам необходимо провести анализ среднего чека для заказов в вашем интернет-магазине.
В результате этого запроса будет один столбец average_receipt.
Используете таблицы gb_shop.OrderDetails и gb_shop.Products.


-- Вы работаете с PostgreSQL
-- Введите свой код ниже
-- Вычисление среднего чека с дополнительным фильтром для точности

WITH receipts AS (
    SELECT orderid, AVG(price * quantity) AS receipt_total
    FROM gb_shop.OrderDetails od
    JOIN gb_shop.Products p ON p.ProductID = od.ProductID
    GROUP BY orderid
)
SELECT AVG(receipt_total) AS average_receipt
FROM receipts;


**********************


Количество уникальных заказов

Инструкция по использованию платформы



Необходимо провести анализ количества уникальных заказов, доставленных каждым перевозчиком в разные месяцы и годы.
Отсортируйте по перевозчику, месяцу и году заказа.

В результате запроса, сформулированного вами, будут следующие столбцы:

shippername: наименование перевозчика,
month(orderdate): месяц даты заказа,
year(orderdate): год даты заказа,
cnt: количество уникальных заказов, доставленных каждым перевозчиком в каждом месяце и году.

Используемые таблицы gb_shop.Shippers и gb_shop.Orders.

EXTRACT: EXTRACT - это функция, используемая для извлечения части даты или времени (например, месяца, дня, года) из значения даты или времени. Например: SELECT EXTRACT(MONTH FROM '2023-07-04' AS DATE);


-- Вы работаете с PostgreSQL
-- Введите свой код ниже

SELECT 
    s.shippername, 
    EXTRACT(MONTH FROM CAST(o.orderdate AS TIMESTAMP)) AS month, 
    EXTRACT(YEAR FROM CAST(o.orderdate AS TIMESTAMP)) AS year, 
    COUNT(DISTINCT o.orderid) AS cnt
FROM 
    gb_shop.Orders o
JOIN 
    gb_shop.Shippers s ON o.shipperid = s.shipperid
GROUP BY 
    s.shippername, 
    EXTRACT(MONTH FROM CAST(o.orderdate AS TIMESTAMP)), 
    EXTRACT(YEAR FROM CAST(o.orderdate AS TIMESTAMP))
ORDER BY 
    s.shippername, 
    EXTRACT(YEAR FROM CAST(o.orderdate AS TIMESTAMP)) DESC, 
    EXTRACT(MONTH FROM CAST(o.orderdate AS TIMESTAMP));


*****************************************



LTV

Инструкция по использованию платформы



Необходимо вычислить среднюю жизненную ценность клиента (LTV) (сколько денег покупатели в среднем тратят в магазине за весь период) для всех клиентов на основе их заказов в интернет-магазине.

В результате этого запроса будет один столбецAVG(ltv).

Используемые таблицы gb_shop.OrderDetails, gb_shop.Orders, gb_shop.Products.

-- Вы работаете с PostgreSQL
-- Введите свой код ниже
WITH ltvs AS (SELECT customerid, AVG(price*quantity) AS ltv 
FROM gb_shop.OrderDetails od
    JOIN gb_shop.Orders o ON o.OrderID=od.OrderID
           JOIN gb_shop.Products p ON p.ProductID=od.ProductID
GROUP BY customerid)
SELECT AVG(ltv) 
FROM ltvs

***************