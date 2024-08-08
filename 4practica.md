
Суммы кредитов

Инструкция по использованию платформы



Сопоставьте совокупную сумму сумм кредита (CumulativeSum) для каждого кластера, упорядоченную по месяцам, и сумму кредита в порядке возрастания.

В результате этого запроса должны быть столбцы:

month: месяц,
cluster: кластер (или категория),
credit_amount: сумма кредита,
CumulativeSum: накопленная сумма кредита для каждого кластера, рассчитанная по месяцам и сумме кредита, отсортированным в порядке возрастания.
Используйте таблицуgb_shop.Clusters.


-- Вы работаете с PostgreSQL
-- Введите свой код ниже

SELECT
    month,
    cluster,
    credit_amount,
    SUM(credit_amount) OVER (PARTITION BY cluster ORDER BY month, credit_amount) AS CumulativeSum
FROM gb_shop.Clusters;

****************


Cредняя сумма кредита

Инструкция по использованию платформы



Рассчитайте среднюю сумму кредита (AvgCreditAmount) для каждого кластера в каждом месяце, учитывая общую среднюю сумму кредита за соответствующий месяц(OverallAvgCreditAmount).

В результате этого запроса будет четыре столбца:

month: месяц,
cluster: кластер (или категория),
AvgCreditAmount: средняя сумма кредита для каждого кластера в каждом месяце,
OverallAvgCreditAmount: общая средняя сумма кредита для каждого месяца.
Используйте таблицу gb_shop.Clusters.



-- Вы работаете с PostgreSQL
-- Введите свой код ниже

SELECT
    month,
    cluster,
    AVG(gb_shop.Clusters.credit_amount) AS AvgCreditAmount,
    AVG(gb_shop.Clusters.credit_amount) OVER (PARTITION BY month) AS OverallAvgCreditAmount
FROM gb_shop.Clusters
GROUP BY month, cluster, gb_shop.Clusters.credit_amount;


*********************************




Ранжируйте продукты

Инструкция по использованию платформы



Ранжируйте продукты (по ProductRank) в каждой категории на основе их общего объема продаж (TotalSales).

Ваша задача — ранжировать товары в каждой категории на основе их общего объема продаж, сгруппированного по категориям.

Набор данных включает в себя информацию из gb_shop.OrderDetails таблицы, которая содержит сведения о каждом продукте в заказе, и таблицы gb_shop.Products, которая предоставляет дополнительную информацию о каждом продукте.

PARTITION BY od.ProductID гарантирует, что сумма рассчитывается отдельно для каждого продукта.
PARTITION BY CategoryID гарантирует, что рейтинг будет составляться отдельно для каждой категории.

В результате этого запроса будут столбцы:

CategoryID: идентификатор категории продукта,
ProductID: идентификатор продукта,
ProductName: название продукта,
ProductRank: ранг продукта внутри своей категории на основе общего объема продаж в порядке убывания.
Используйте таблицы gb_shop.OrderDetails и gb_shop.Products.


-- Вы работаете с PostgreSQL
-- Введите свой код ниже
SELECT 
    CategoryID,
    ProductID,
    ProductName,
    RANK() OVER (PARTITION BY CategoryID ORDER BY TotalSales DESC) AS ProductRank
FROM (
    SELECT 
        p.CategoryID,
        od.ProductID,
        p.ProductName,
        SUM(od.Quantity * p.Price) OVER (PARTITION BY od.ProductID) AS TotalSales
    FROM gb_shop.OrderDetails od
    JOIN gb_shop.Products p ON od.ProductID = p.ProductID
) AS ProductSales;



****************************