# Checking time series for stationarity using the example of cryptocurrency quotes data for different time periods..
# Проверка временных рядов на стационарность на примере данных котировок криптовалют за разные периоды времени.

Определение стационарности:
Стационарный временной ряд - это временной ряд, статистические свойства (среднее, дисперсия) которого не меняются со временем. Таким образом, временные ряды с трендами или с сезонностью не являются стационарными - тренд и сезонность будут влиять на значение временного ряда в разные моменты времени. С другой стороны, временной ряд, являющийся белым шумом, является стационарным, поскольку он будет выглядеть примерно одинаково в любой момент времени.
Тренд-стационарный процесс - это стохастический процесс , из которого можно удалить базовый тренд (функция только времени), оставив стационарный процесс . Тенденция не обязательно должна быть линейной.

Вначале в ноутбуке зададем базовые значения для дальнейших расчетов:

Интересующий нас символ криптовалюты(к примеру 'DOGE' или 'BTC')
FSYM = 'DOGE'

Символ валюты в которую конвертируется(к примеру 'RUB' или 'USD')
TSYM = 'RUB'

Период графика(к примеру день - 'D' или час - 'H')
PERIOD = 'H'   # 'D'  'H'  

Время с которого начинается загрузка котировок.
Например -  datetime.datetime(2014,9,1)
если нужна загрузка с самого начала существующих данных,
то пишем - 0
FROMTS = datetime(2021,2,15)

Время до которого качаем котировки,
Например -  datetime(2022,3,19)
если нужна загрузка до текущего момента,
то пишем - datetime.now()
TOTS = datetime(2021,3,20)  #      datetime.now()     

Зададим биржу с которой будут браться данные
Например - 'Kraken'
Если же хотим взять агрегированный индекс объединяющий данные 
о транзакциях из более чем 70 бирж, т.е. индекс CCCAGG,
то пишем - 'CCCAGG'
EXCHANGE = 'CCCAGG'

Значение по которому будем анализировать 
Например по цене открытия свечи - 'open'
или же по цене закрытия свечи - 'close'
CANDLE_DATA = 'close'  # 'open','close','high','low'

Критическое значение с доверительным интервалом в процента,
которое будет использоваться в расчетах в тестах Дики-Фуллера
и тесте KPSS (Kwiatkowski-Phillips-Schmidt-Shin).
Обычно для оценки стационарности берется критическое значение 
с доверительным интервалом - 5%
CRITICAL_VALUE = '5%'

Запускаем ноутбук.

Получаем результаты проверки на стационарность по 
1) расширенному тесту Дики-Фуллера, Augmented Dickey–Fuller test ( ADF )
2) тесту KPSS (тест Квятковского-Филлипса-Шмидта-Шина)

Делается вывод на основе сочетания результатов этих тестов:

Различные типы стационарности и как интерпретировать результаты вышеупомянутых тестов:
Строго стационарный: строгий стационарный ряд удовлетворяет математическому определению стационарного процесса. Для строгого стационарного ряда среднее значение, дисперсия и ковариация не являются функцией времени. Цель состоит в том, чтобы преобразовать нестационарный ряд в строгий стационарный ряд для прогнозирования.
Стационарный тренд: ряд, который не имеет единичного корня, но демонстрирует тренд, называется стационарным рядом тренда. После удаления трендовой составляющей, результирующий ряд будет строго стационарным. Тест KPSS классифицирует ряд как стационарный при отсутствии единичного корня. Это означает, что ряд может быть строго стационарным или стационарным по тренду.
Разностно-стационарный: временной ряд, который можно сделать строгим стационарным с помощью разностей, попадает в категорию разностно-стационарных. Тест ADF также известен как тест разностной стационарности.
Всегда лучше применять оба теста, чтобы мы были уверены, что ряд действительно стационарный. Давайте посмотрим на возможные результаты применения этих тестов.
* Случай 1: Оба теста показывают, что ряд не является стационарным -> ряд не является стационарным.
* Случай 2: Оба теста показывают, что ряд является стационарным -> ряд является стационарным
* Случай 3: KPSS = стационарный и ADF = не стационарный -> стационарный тренд, удалить тренд, чтобы сделать ряд строго стационарным.
* Случай 4: KPSS = не стационарный и ADF = стационарный -> разностно-стационарный, используйте разности, чтобы сделать ряд стационарным(т.е. дифиренцируйте ряд).

Кроме этого выводятся графики автокорреляционной функции (ACF) и частной автокорреляционной функции (PACF)
по виду которых тоже можно сделать определенные выводы о стационарности ряда или его близости к этому.

Так же можно с помощью библиотеки statsmodels сделать автоматическую декомпозицию ряда на составляющие - 
Тренд, Сезонность, Остатки.
