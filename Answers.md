## 1
Создать индекс по полям cnt и name:
```sql
KEY example_index(cnt, name)
```
## 2
Указать в конфигурационном файле php, в error_log название файла.
## 3
<pre>
Во-первых, нет проверки, существует ли такой ключ как id в массиве $_GET;
Во-вторых, нет проверки, действительно ли то, что находится в $_GET['id'] имеет нужный тип данных;
В-третьих, нужно использовать плейсхолдер.
Таким образом, можно переписать этот sql-запрос следующим образом:
```php
$id = $_GET['id'] ?? NULL;

if (!$id) {
	throw new IDNotSentException('Не передан ID');
}

if (!is_int($id)) {
	throw new IDMustBeNumberException('ID должен быть числом');
}

$stmt = $DB->pdo->prepare("SELECT * FROM abc WHERE id=:id");
$stmt->bindValue(':id', $id);
$stmt->execute();
```
</pre>
## 4
<pre>
SELECT customer_name, SUM(order_price), MONTHNAME(date) FROM orders GROUP BY MONTHNAME(date), customer_name ORDER BY MONTHNAME(date);
SELECT customer_name FROM orders GROUP BY customer_name HAVING SUM(order_price)>10000 AND MIN(order_price)>=500;
</pre>
## 5
<pre>
Эти функции объявлены двумя разными способами.
Первая - Function Declaration.
Вторая - Function Expression.
Разница между ними:
Инструкция с вызовом первой функции может идти раньше, чем инструкция с объявлением этой функции.
В случае же со второй функцией, до тех пор пока интерпретатор не дойдет до инструкции создания этой функции, вызывать ее нельзя.
</pre>
## 6
<pre>
INNER JOIN: выбирает все строки, которые соот. условию объединения таблиц.
LEFT JOIN: выбирает все строки левой таблицы и строки из правой, если они соот. условию.
LEFT JOIN потенциально может дать больше результатов, потому что в выборку попадают не только те строки, которые соот. условию, но и вообще
все строки левой таблицы.
</pre>
## 7
В моем решении используется ubuntu, xampp, cron, curl (которые должны быть предварительно установлены)
Создать php-скрипт в папке, из которой раздаются файлы сервера (в моем случае это папка /opt/lampp/htdocs/dashboard):
```php
<?php
echo 'Hello';
?>
```
Создать cron-задание:
Первый вариант:
```
* * * * * /usr/bin/php /opt/lampp/htdocs/dasboard/hello.php >> /tmp/date.log
* * * * * sleep 15; /usr/bin/php /opt/lampp/htdocs/dasboard/hello.php >> /tmp/date.log
* * * * * sleep 30; /usr/bin/php /opt/lampp/htdocs/dasboard/hello.php >> /tmp/date.log
* * * * * sleep 45; /usr/bin/php /opt/lampp/htdocs/dasboard/hello.php >> /tmp/date.log
```
Пояснение: каждые 15 секунд cron запускает скрипт hello.php и записывает вывод скрипта в /tmp/date.log
Второй вариант:
```
* * * * * curl http://localhost/dashboard/hello.php
* * * * * sleep 15; curl http://localhost/dashboard/hello.php
* * * * * sleep 30; curl http://localhost/dashboard/hello.php
* * * * * sleep 45; curl http://localhost/dashboard/hello.php
```
Пояснение: каждые 15 секунд curl выполняет HTTP-запрос, запрашивая ресурс по URL-адресу http://localhost/dashboard/hello.php, т.к. обращение к php-скрипту через URL приводит к его выполнению, то теоретически, надпись 'Hello' выводится каждые 15с.
## 8
```php
$discount = 1000;
$prices = [500, 1000];

function distribute_discount(int $discount, array $prices): array
{
    $sumOfPrices = array_sum($prices);
    for ($i = 0; $i < count($prices); $i++) {
            $percent = round($prices[$i] * 100 / $sumOfPrices);
            $currentDiscount = $discount * $percent / 100;
            $prices[$i] = $prices[$i] - $currentDiscount;
    }
    return $prices;
}

var_dump(distribute_discount($discount, $prices));
```
