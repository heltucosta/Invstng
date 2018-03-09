# Securities Master Databases
* Usuallym the most focused part of a trading system is the system that takes cares of generating signals, called the **_alpha model_**.
* However, it will only be as good as the data it was fed with. 
* It is crucial that accurate, timely data is used to feed the alpha model, otherwise results will be poor and lead to large losses.
* It is important to discuss the issues surrounding the acquisition and provision of timely accurate data for algorithmic trading strategy backtesting and execution system.
* **Securities Master Database** is an organisation-wide database that stores, _fundamental_, _pricing_ and _transactional_ data for financial instruments across all assets.
* Provides informations in a consisten manner that can be used by multiple departments, like risk management, clearing/settlement and proprietary trading.
* Some of the instruments that might be of interest:
  * Equities;
  * Equity Options;
  * Indices;
  * Foreign Exchange;
  * Interest Rates;
  * Futures;
  * Commodities;
  * Bonds
  * Derivatives (caps, floors, swaps).

## Which datasets are used
* It is important to disclaim that the datasets used by retail investors, when compared to big investment funds, are pretty different.
* For retail investors, it will mostly use end-of-day (EOD) and intra-day pricing for equities, indices, futures and forex.
* There are a number of services that provide EOD data:
  * Yahoo Finance
  * Google Finance
  * QuantQuote
  * EODData
* When working with small portfolio, it is perfectly fine to download the datasets mannualy. When more assets are being traded and much time is being lost downloading datasets, it can be really usefull to automate the update of datasets.
* Another important aspect to take note is the **look-back period**that will be used. This will greatly depend on the strategy built, but there are some problemns that expand all strategies.
* **Regime Change** is the most common of those problemns, and is characterized by changes in the regularoty environment, with periods of high/low volatility or longer-term trending markets.
* There are obstacles that should be noted when working with big amounts of data, but it is generally better to have the most amount of data available to train a trading strategy.

## What is used to store data
* Three main ways: Flat-file storage, NoSQL Databases (Document Store), Relational Database Management Systems.
* Each have its access level, performance and structural capabilities.

### 1. Flat-File Storage
* The simplest way to store financial data;
* Most likely the way you'll receive data from vendors;
* Often a CSV file (Comma-Separated Variables);
* For EOD data, each row represents a trading day, with values beeing shown in OHLC paradigm (Open-High-Low-Close)
* This type of storage advantage is it's simplicity in use, as-well-as the compression that can be applied when for archiving and downloading. Their disadvantages are low-performance and inability to perform queries.

### 2. Document Stores/NoSQL
* Old concept that gained a lot of force recently;
* Differ considerably from Relational Databases, since there's no concept of tables;
* Data is stored in collections and documents;
* Popular environments are MongoDB, Cassandra and CouchDB;
* When regarding financial data, these type of data storage is more suited for fundamental data (corporate actions, earnins statements, SEC fillings, etc);
* Not well designed for working with time-series such as high-resolution pricing.

### 3. Relational Database Management Systems (RDBMS)
* Makes use of relational models to store data;
* Well suited for working with financial data since different 'objects' (like exchanges, data sources, prices, etc) can be stored in its own table and they can relate to one another;
* Makes use of _Structured Query Language_ to perform complex data queries;
* Most common environments are: Oracle, MySQL, SQLServer and PostgreSQL;
* Main advantages are:
  * ease of installation;
  * platform-independency;
  * ease of querying;
  * ease of integration with backtest software;
  * high-performance capabilities at large scale;

* Main disadvantages are:
  * Complexity of customization;
  * Learning curve;
  * achieving high-performance can be hard for unseasoned developers;
  * semi-rigid schemas, requiring data to be modified in order to fit the table.

## How is historical data structured 
* This can be designed in a infinite number of ways, we will only demonstrate the most common ones;
* It is important to understand when and how to modify which data you're using to create the strategy;
* The first task is to identify the _entities_ you'll want to map from the financial data to your tables;
* Some of the basic ones you should take note are:
  1. **Exchanges**: the source of the data;
  2. **Vendor**: who provided the current data point;
  3. **Instrument/Ticker**: symbol for index, along with corporate information;
  4. **Price**: The actual price of an asset on a particular day;
  5. **Corporate Actions**: All stock splits or dividend adjustments, necessary for adjusting the pricing data;
  6. **National Holidays**: Usefull for avoiding miss-classifying trading holidays with missing data errors;
* It is important to note that tickers can change as time passes by, either by a company being bankrupt, or by being acquired by another company, and even create more stock classes. It is important to match tickers from differentes sources when they may have different tickers.

## Data accuracy
* Data provided by vendors can come with many forms of errors:
  1. **Corporate Actions**: incorrect handling of stock splits and dividend adjustments.
  2. **Spikes**: Pricing points that greatly exceed historical volatility levels. Can occur when not taking into account stock splits. Can be mitigated utilizing _Spike filter scripts_.
  3. **OHLC Aggregation**: free vendor data are prone to having 'bad tick aggregation', which can occur when small exchanges process transactions in price levels that differ from the main exchange price. It's not an error, but something to be mindfull of.
  4. **Missing Data**: can be caused by many reasons, like lack of trades in a certain period, trading holidays or simply an error om the exchange system. This problem can be mitigated by various processes, like _padding_(filled with previous value), _interpolation_(linearly or otherwise) or ignoring the missing data.
* Many of this errors rely on manual decision on how to proceed, although it is possible to automate, it can sometimes be very hard.
* What is usually more accepted is automate the process of identifying possible errors, so a manual decision can be made.
* Strategies must be designed so that a single data point cannot skew the results and performance.

## How to automate the process
* Re-ocurring tasks (scripts) can be executed by using **Crontab** in UNIX systems, and **TaskScheduler** on windows.
* Automates the download of EOD data when available.
* Run missing data and spike filters, creating alerts for the trader(e-mail, SMS,etc).
* Make new data available for backtesting scripts. 
* Dependeing on whether your system is local or over at a remote server, how you get data to your backtesting system can be time-consuming.
* It is better to minimize Input/Output processes, performing a selective query before you send the data to the backtesting script.
* MySQL and PostgreSQL can be used with open-source softwares, meanwhile SQLServer is created to easily integrates with MS .NET, C# and ASP.NET technologies.

## Benefits of a Local Securities Master Database
1. **Speed**:with the databsae locally, any data application can rapidly access it. When it is online, you would need to perdorm multiple Input/Output (I/O) actions on a latent network.
2. **Mutiple Sources**: Allows the storage of multiple data sources for the same ticker. Custom error correction code and audit trails to correct data.
3. **Downtime**: Internet connections can go down, both on your side as well as the vendor's side. A local database followed by a replication system will always be online.
4. **Meta-data**: Allows us to store fundamental data on a ticker, helping us to minimise data source errors.
5. **Transactions**: with time, the database can grow to include our own transactional data history. This can help us with data analysis on the data we have created.

