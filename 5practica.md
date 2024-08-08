Создайте хранимую процедуру с именем GetEmployeeOrders. который принимает идентификатор сотрудника в качестве параметра и возвращает все заказы, обработанные этим сотрудником, идентифицированным по EmployeeID. Пропишите запрос, который создаст требуемую процедуру.

-- Вы работаете с PostgreSQL
-- Введите свой код ниже

-- Создаем хранимую процедуру GetEmployeeOrders
CREATE OR REPLACE FUNCTION itresume10575057.GetEmployeeOrders(employee_id INT)
RETURNS TABLE (
    OrderID INT,
    OrderDate DATE,
    CustomerName TEXT
) AS $$
BEGIN
    RETURN QUERY
    SELECT 
        o.OrderID,                -- Идентификатор заказа
        o.OrderDate,              -- Дата заказа
        c.CustomerName            -- Имя клиента
    FROM 
        itresume10575057.gb_shop.Orders o   -- Таблица заказов с указанной схемой
        JOIN itresume10575057.gb_shop.Customers c -- Таблица клиентов с указанной схемой
        ON o.CustomerID = c.CustomerID
    WHERE 
        o.EmployeeID = employee_id; -- Фильтрация по идентификатору сотрудника
END;
$$ LANGUAGE plpgsql;

##

Создайте таблицу {student_schema_name}.EmployeeRoles, как на уроке.

Важно: Чтобы проверка прошла успешно, перед нажатием кнопки Проверить студент должен написать запрос и нажать кнопку Выполнить.

-- Вы работаете с PostgreSQL
-- Введите свой код ниже

-- Создаем таблицу EmployeeRoles в схеме itresume10575057
CREATE TABLE itresume10575057.EmployeeRoles (
    RoleID SERIAL PRIMARY KEY,     -- Уникальный идентификатор роли, автоматически увеличивается
    RoleName VARCHAR(255) NOT NULL -- Название роли, не может быть NULL
);
##

Удалите таблицу {student_schema_name}.EmployeeRoles, созданную в предыдущей задаче.

Пропишите запрос, который удалит нужную таблицу.

Важно: Чтобы проверка прошла успешно, перед нажатием кнопки Проверить студент должен написать запрос и нажать кнопку Выполнить.

-- Удаляем таблицу EmployeeRoles в схеме itresume10575057
DROP TABLE itresume10575057.EmployeeRoles;
##

Удалите все заказы со статусом Delivered из таблицы OrderStatus, которую создавали на семинаре.

Пропишите запрос, который удалит нужные строки в таблице.

Важно: Чтобы проверка прошла успешно, перед нажатием кнопки Проверить студент должен написать запрос и нажать кнопку Выполнить.


-- Вы работаете с PostgreSQL
-- Введите свой код ниже

DROP TABLE IF EXISTS  itresume10575057.orderstatus;
CREATE TABLE itresume10575057.orderstatus (
    OrderStatusID INT PRIMARY KEY,
    OrderID INT,
    Status VARCHAR(50)
);

-- Удаляем все заказы со статусом 'Delivered' из таблицы OrderStatus в схеме itresume10575057
DELETE FROM itresume10575057.OrderStatus
WHERE Status = 'Delivered';