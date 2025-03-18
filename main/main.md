## init
```bash
<>

```

```sql
SELECT *
FROM POWERBI.TRANSACTIONS
WHERE SALES_AMOUNT < 0

SELECT *
FROM POWERBI.TRANSACTIONS
WHERE MARKET_CODE = 'Mark003'

SELECT *
FROM POWERBI.TRANSACTIONS TXN
INNER JOIN POWERBI.DATE DT
ON TXN.ORDER_DATE = DT.DATE

SELECT DT.YEAR, SUM(TXN.SALES_AMOUNT)
FROM POWERBI.TRANSACTIONS TXN
INNER JOIN POWERBI.DATE DT
ON TXN.ORDER_DATE = DT.DATE
	-- AND DT.YEAR = '2020'
GROUP BY DT.YEAR

SELECT DT.YEAR, SUM(TXN.SALES_AMOUNT)
FROM POWERBI.TRANSACTIONS TXN
INNER JOIN POWERBI.DATE DT
ON TXN.ORDER_DATE = DT.DATE
	-- AND DT.YEAR = '2020'
GROUP BY DT.YEAR



```

## Connect to powerbi

- Do the data modeling
```dax
= Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" then [sales_amount]*75 else [sales_amount])

= Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" or [currency] = "USD\r" then [sales_amount]*75 else [sales_amount])


```

