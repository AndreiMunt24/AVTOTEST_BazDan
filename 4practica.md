
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



NOVIE AVTOTESTI


Лайки

Инструкция по использованию платформы



Имеется база данных – социальная сеть.

База данных содержит сущности:
users – пользователи;
messages – сообщения;
friend_requests – заявки на дружбу;
communities – сообщества;
users_communities – пользователи сообществ;
media_types – типы медиа;
media – медиа;
likes – лайки;
profiles – профили пользователя.

У сущности «пользователи» имеются следующие поля(атрибуты):
id – идентификатор;
firstname – имя;
lastname - фамилия;
email - адрес электронной почты.

У сущности «профиль пользователя» имеются следующие поля(атрибуты):
user_id – идентификатор;
gender – пол;
birthday - дата рождения;
photo_id - аватарка;
hometown - город.

Атрибут «пол» сущности «профиль пользователя» может принимать следующие значения:
'f' - женский;
'm' - мужской.

У сущности «лайки» имеются следующие поля(атрибуты): id – идентификатор;
user_id – пользователь, который поставил лайк;
media_id - медиа, который лайкнули.

У сущности «медиа» имеются следующие поля(атрибуты):
id – идентификатор;
user_id – пользователь – владелец медиа;
body - содержимое;
filename – ссылка на файл;
created_at - дата создания;
updated_at - дата последнего обновления.

Найдите общее количество лайков, которые получили пользователи женского пола.



-- Вы работаете с MySQL
-- Введите свой код ниже

SELECT COUNT(l.id) AS total_likes
FROM likes l
JOIN media m ON l.media_id = m.id
JOIN profiles p ON m.user_id = p.user_id
WHERE p.gender = 'f';


**********************************************************




Лайки 2

Инструкция по использованию платформы



Имеется база данных – социальная сеть.

База данных содержит сущности:
users – пользователи;
messages – сообщения;
friend_requests – заявки на дружбу;
communities – сообщества;
users_communities – пользователи сообществ;
media_types – типы медиа;
media – медиа;
likes – лайки;
profiles – профили пользователя.

У сущности «пользователи» имеются следующие поля(атрибуты):
id – идентификатор;
firstname – имя;
lastname - фамилия;
email - адрес электронной почты.

У сущности «профиль пользователя» имеются следующие поля(атрибуты):
user_id – идентификатор;
gender – пол;
birthday - дата рождения;
photo_id - аватарка;
hometown - город.

Атрибут «пол» сущности «профиль пользователя» может принимать следующие значения:
'f' - женский;
'm' - мужской.

У сущности «лайки» имеются следующие поля(атрибуты):
id – идентификатор;
user_id – пользователь, который поставил лайк;
media_id - медиа, который лайкнули.

Найдите количество лайков, которые поставили пользователи женского пола и мужского пола.
Выведите название пола (преобразовав значение атрибута f в женский, а значение ‘m` - мужской) и количество, поставленных лайков, применив к количеству лайков сортировку по убыванию.


-- Вы работаете с MySQL
-- Введите свой код ниже


SELECT 
    CASE 
        WHEN p.gender = 'f' THEN 'женский'
        WHEN p.gender = 'm' THEN 'мужской'
    END AS gender,
    COUNT(l.id) AS likes_count
FROM likes l
JOIN profiles p ON l.user_id = p.user_id
GROUP BY p.gender
ORDER BY likes_count DESC;


********************************************************


Кто не отправлял сообщения

Инструкция по использованию платформы



Имеется база данных – социальная сеть.

База данных содержит сущности:
users – пользователи;
messages – сообщения;
friend_requests – заявки на дружбу;
communities – сообщества;
users_communities – пользователи сообществ;
media_types – типы медиа;
media – медиа;
likes – лайки;
profiles – профили пользователя.
У сущности «пользователи» имеются следующие поля(атрибуты):
id – идентификатор;
firstname – имя;
lastname - фамилия;
email - адрес электронной почты.

У сущности «сообщения» имеются следующие поля(атрибуты):
id – идентификатор;
from_user_id – отправитель;
to_user_id – получатель;
body - содержимое;
created_at - дата отправки.

Выведите идентификаторы пользователей, которые не отправляли ни одного сообщения.

-- Вы работаете с MySQL
-- Введите свой код ниже

SELECT u.id
FROM users u
LEFT JOIN messages m ON u.id = m.from_user_id
WHERE m.from_user_id IS NULL;

********************************************************



Друзья

Инструкция по использованию платформы



Имеется база данных – социальная сеть.

База данных содержит сущности:
users – пользователи;
messages – сообщения;
friend_requests – заявки на дружбу;
communities – сообщества;
users_communities – пользователи сообществ;
media_types – типы медиа;
media – медиа;
likes – лайки;
profiles – профили пользователя.

У сущности «заявки на дружбу» имеются следующие поля(атрибуты):
initiator_user_id – инициатор;
target_user_id – получатель;
status - статус;
requested_at - дата создания;
updated_at - дата последнего обновления.

У сущности «пользователи» имеются следующие поля(атрибуты):
id – идентификатор;
firstname – имя;
lastname - фамилия;
email - адрес электронной почты.

Друзья — это пользователи у которых имеется соответствующая запись (заявка) в сущности «заявки на дружбу» и в атрибуте status данной сущности указано значение 'approved'.

Найдите количество друзей у каждого пользователя. Выведите для каждого пользователя идентификатор пользователя, имя, фамилию и количество друзей cnt. Сортировка выводимых записей в порядке возрастания идентификатора пользователя.


-- Вы работаете с MySQL
-- Введите свой код ниже

SELECT 
    u.id AS user_id,
    u.firstname,
    u.lastname,
    COALESCE(COUNT(DISTINCT CASE 
        WHEN fr.initiator_user_id = u.id THEN fr.target_user_id
        WHEN fr.target_user_id = u.id THEN fr.initiator_user_id
    END), 0) AS cnt
FROM users u
LEFT JOIN friend_requests fr 
    ON (u.id = fr.initiator_user_id OR u.id = fr.target_user_id)
    AND fr.status = 'approved'
GROUP BY u.id, u.firstname, u.lastname
ORDER BY u.id;


