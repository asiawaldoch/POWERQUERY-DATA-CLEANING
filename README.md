<h1 align="center">Project Overview</h1>

## Tools: PowerQuery, PowerBI
This project demonstrates a complete data cleaning and transformation process performed in Power Query.

The source dataset contained:
1. Inconsistent date formats
2. Combined fields
3. Incorrect text formatting
4. Duplicated records
5. Invalid characters
6. Data quality issues

The objective was to transform raw transactional data into an analysis-ready dataset suitable for reporting in Power BI.

## <h1 align="center">DATA CLEANING</h1>

## <h1 align="center">1. Date Column Standardization</h1>

“DataSprzedazy” column contained inconsistent date formats and unwanted characters (x) which could lead to incorrect date recognition and inaccurate time-based analysis.

## Transformation Steps
## 1. Remove Invalid Characters <br>
Some records contained an additional x character before the date value. I used Replace Values transformation to remove these characters and standardize the raw data.

<p align="center">
  <img src="images/date-beforeafter.png" alt="date-beforeafter">
</p>

## 1. Standardize Date Format <br>
After cleaning the values, all dates were converted to a consistent date format and assigned the correct data type.
## Result
•	Consistent date formatting
•	Proper chronological sorting
•	Reliable time-series analysis

<p align="center">
  <img src="images/date-afterformat.png" alt="date-afterformat">
</p>

## <h1 align="center">2. Customer Data Transformation</h1>

“KlientSurowy” column contained multiple pieces of information stored in a single field and inconsistent text formatting. This structure made customer segmentation difficult.

<p align="center">
  <img src="images/klientsurowy-before.png" alt="klientsurowy-before">
</p>

## Transformation Steps
## 1. Split Customer Information
The original “KlientSurowy” field contained multiple product attributes stored in a single column, which could make further progress difficult.
I used a Split column formula in order to distinct two values:

<p align="center">
  <img src="images/klientsurowy-splitc.png" alt="klientsurowy-splitc">
</p>

The original value: <br>

Klient 0107 ; VIP

was split into two separate columns:

ClientID     | ClientType

K0107        | VIP

<p align="center">
  <img src="images/klientsurowy-after.png" alt="klientsurowy-after">
</p>

## 2. Rename Columns

Generated column names were replaced with meaningful business names:

•	ClientID

•	ClientType

<p align="center">
  <img src="images/klientsurowy-splitc2.png" alt="klientsurowy-splitc2">
</p>

<p align="center">
  <img src="images/klientsurowy-splitcafter.png" alt="klientsurowy-splitcafter">
</p>

## 3. Standardize Client IDs

•	Converted text to lowercase

•	Removed unnecessary text

•	Added the K prefix

•	Standardized IDs across all records

## 4. Standardize Client Types

Customer segments contained inconsistent capitalization. I used UPPERCASE formula to standaralize.

<p align="center">
  <img src="images/clienttype-before.png" alt="clienttype-before">
</p>

Result
•	Unique and consistent customer identifiers
•	Clean customer segmentation
•	Better data quality for future reporting

<p align="center">
  <img src="images/clienttype-after.png" alt="clienttype-after">
</p>

Final columns extracted from "“KlientSurowy”:

<p align="center">
  <img src="images/klient-after.png" alt="klient-after">
</p>

## <h1 align="center">3. Product Data Extraction</h1>
“ProduktSurowy” column contained multiple attributes combined into a single field.

<p align="center">
  <img src="images/produktsurowy-before.png" alt="produktsurowy-before">
</p>

## Transformation Steps
The data neseccary fot futher analysis is "ProductID" because the rest of data can be fount in "Produkty" table.
In this case I used formula Extract -> Text beofre delimiter to extract only ProductID and eliminate unnecessary data from this table.

The product dimension became analysis-ready and suitable for creating a star schema

<p align="center">
  <img src="images/produktsurowy-after.png" alt="produktsurowy-after">
</p>

## <h1 align="center">4. Region Standardization</h1>

“Region” column contained inconsistent values caused by additional characters such as "/", "Polnoc".

<p align="center">
  <img src="images/region-efore.png" alt="region-efore">
</p>

## Transformation Steps

•	Removed unnecessary special characters with Replace Values

•	Standardized region names, by changing Połnoc -> Północ

Only valid region values remained:

<p align="center">
  <img src="images/region-after.png" alt="region-after">
</p>

## <h1 align="center">5. Order Status Standardization</h1>

„StatusZamowienia” column contained inconsistent capitalization.

<p align="center">
  <img src="images/statuszamowienia-before.png" alt="statuszamowienia-before">
</p>

## Transformation Steps
I applied text standardization techniques such as Replace Values  to ensure consistent values throughout the dataset.

Result

The same business status is now represented by a single standardized value.

<p align="center">
  <img src="images/statuszamowienia-after.png" alt="statuszamowienia-after">
</p>

## <h1 align="center">6. Data Quality Validation</h1>

“IDTransakcji” validation -  a uniqueness check was performed on Transaction IDs. - 14,999 distinct, 14,988 unique

<p align="center">
  <img src="images/idtransakcji-before.png" alt="idtransakcji-before">
</p>

One duplicated transaction ID (T000321) was identified. Since the records contained different transaction details, they were retained as a potential business data quality issue for further investigation.

<p align="center">
  <img src="images/idtransakcji-excel.png" alt="idtransakcji-excel">
</p>

Rather than automatically removing duplicates, the records were investigated first to determine whether they represented a true duplicate or a legitimate business scenario.


## <h1 align="center">7. Format standardization</h1>

<p align="center">
  <img src="images/column-type-before.png" alt="column-type-before">
</p>

Several numerical columns such as:

- UnitPrice
  
- Discount

- UnitCost
were converted to appropriate numeric data types.

This ensured accurate calculations, aggregations and KPI reporting in Power BI.

<p align="center">
  <img src="images/column-type-after.png" alt="column-type-after">
</p>

## <h1 align="center">8. ETL Architecture</h1>

Instead of cleaning data directly inside reporting tables, a dedicated table named “sprzedaż_source” was created as the master data layer.

<p align="center">
  <img src="images/sprzedaz_source-diagram.png" alt="sprzedaz_source-diagram">
</p>

This architecture follows ETL best practices by separating the raw data layer from reporting and analytical layers.

All subsequent dimension tables will have a reference to sprzedaż_source – cleaned and prepared tables in order to increase efficiency of our project.

Benefits

•	Single source of truth

•	Reusable transformations

•	Easier maintenance

•	Better Power BI performance

•	Scalable ETL workflow

“Sprzedaz_source” structure:

```
let
   #"Źródło" = Csv.Document(File.Contents(#"path" & #"nazwa pliku sprzedaz"), [Delimiter = ",", Columns = 16, Encoding = 65001, QuoteStyle = QuoteStyle.None]),
   #"Nagłówki o podwyższonym poziomie 1" = Table.PromoteHeaders(Źródło, [PromoteAllScalars = true]),
  #"Zamieniono wartość" = Table.ReplaceValue(#"Nagłówki o podwyższonym poziomie 1", "x", "", Replacer.ReplaceText, {"DataSprzedazy"}),
  #"Zmieniono typ kolumny" = Table.TransformColumnTypes(#"Zamieniono wartość", {{"IDTransakcji", type text}, {"DataSprzedazy", type date}, {"DataWysylki", type date}, {"IDKanalu", type text}, {"Ilosc", Int64.Type}, {"CenaJednostkowa", type text}, {"Rabat", type text}, {"KosztJednostkowy", type text}, {"ProduktSurowy", type text}, {"KlientSurowy", type text}, {"Region", type text}, {"StatusZamowienia", type text}, {"NotatkaTechniczna", type text}, {"Kanal", type text}, {"TypKanalu", type text}, {"", type text}}, "pl"),
  #"Usunięto kolumny" = Table.RemoveColumns(#"Zmieniono typ kolumny", {"", "NotatkaTechniczna"}),
  #"Split column by delimiter" = Table.SplitColumn(#"Usunięto kolumny", "KlientSurowy", Splitter.SplitTextByDelimiter("  ;  "), {"ClinetID", "ClientType"}),
  #"Lowercased text" = Table.TransformColumns(#"Split column by delimiter", {{"ClinetID", each Text.Lower(_), type nullable text}}),
  #"Replaced value" = Table.ReplaceValue(#"Lowercased text", "klient", "", Replacer.ReplaceText, {"ClinetID"}),
  #"Trimmed text" = Table.TransformColumns(#"Replaced value", {{"ProduktSurowy", each Text.Trim(_), type nullable text}, {"ClinetID", each Text.Trim(_), type nullable text}, {"Region", each Text.Trim(_), type nullable text}}),
  #"Added prefix" = Table.TransformColumns(#"Trimmed text", {{"ClinetID", each "K" & _, type text}}),
  #"Uppercased text" = Table.TransformColumns(#"Added prefix", {{"ClientType", each Text.Upper(_), type nullable text}}),
  #"Extracted text before delimiter" = Table.TransformColumns(#"Uppercased text", {{"ProduktSurowy", each Text.BeforeDelimiter(_, "|", 0), type text}}),
  #"Renamed columns" = Table.RenameColumns(#"Extracted text before delimiter", {{"ProduktSurowy", "ProductID"}}),
  #"Replaced value 1" = Table.ReplaceValue(#"Renamed columns", ".", "", Replacer.ReplaceText, {"Region"}),
  #"Replaced value 2" = Table.ReplaceValue(#"Replaced value 1", "/", "", Replacer.ReplaceText, {"Region"}),
  #"Replaced value 3" = Table.ReplaceValue(#"Replaced value 2", "Polnoc", "Północ", Replacer.ReplaceText, {"Region"}),
  #"Replaced value 4" = Table.ReplaceValue(#"Replaced value 3", "zrealizowane", "Zrealizowane", Replacer.ReplaceText, {"StatusZamowienia"}),
  #"Trimmed text 1" = Table.TransformColumns(#"Replaced value 4", {{"Region", each Text.Trim(_), type nullable text}}),
  #"Trimmed text 2" = Table.TransformColumns(#"Trimmed text 1", {{"ClientType", each Text.Trim(_), type nullable text}}),
  #"Replaced value 5" = Table.ReplaceValue(#"Trimmed text 2", "realiz.", "Zrealizowane", Replacer.ReplaceText, {"StatusZamowienia"}),
  #"Replaced value 6" = Table.ReplaceValue(#"Replaced value 5", "", "0", Replacer.ReplaceValue, {"Rabat"}),
  #"Changed column type with locale" = Table.TransformColumnTypes(#"Replaced value 6", {{"CenaJednostkowa", type number}}, "en-US"),
  #"Changed column type with locale 1" = Table.TransformColumnTypes(#"Changed column type with locale", {{"Rabat", type number}}, "en-US")
in
  #"Changed column type with locale 1"
```

„Sprzedaz” structure – created in order to put filters or inplement changes that will not influence our source or other tables created basic on sprzedaz_source

```
let
    #"Źródło" = sprzedaz_source

in
  #"Źródło"
```

## <h1 align="center">8. New table - "kanały"</h1>
I created new table „kanały” that bases on "sprzedaz_source" and shows all sales channels used by the company. Creating this dimension table improves reporting performance and simplifies data visualization in Power BI. 

Structure:

```
let
  #"Źródło"= sprzedaz_source,
  #"Removed other columns" = Table.SelectColumns(Źródło, {"IDKanalu", "Kanal", "TypKanalu"}),
  #"Removed duplicates" = Table.Distinct(#"Removed other columns", {"TypKanalu", "Kanal", "IDKanalu"})

in
  #"Removed duplicates"
```
<p align="center">
  <img src="images/kanaly-table.png" alt="kanaly-table">
</p>

## <h1 align="center">10. Calendar Table</h1>

I created a nw, calendar table to prepare for visualisaton:

<p align="center">
  <img src="images/kalendarz.png" alt="kalendarz">
</p>

## Business Requirement

Time intelligence calculations require a dedicated calendar table.

## Transformation Steps

A calendar dimension was generated based on the minimum and maximum sales dates found in the transaction data.
Business Value

```
let
  Source = sprzedaz_source,
  #"Calculated latest" = List.Max(Source[DataSprzedazy]),
  #"Calculated earliest" = List.Min(#"Source"[DataSprzedazy]),
  LiczbaDni = Duration.Days(#"Calculated latest" - #"Calculated earliest") + 1,
  ListaDat = List.Dates(#"Calculated earliest" , LiczbaDni, #duration(1,0,0,0)),
  #"Converted to table" = Table.FromList(ListaDat, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
  #"Renamed columns" = Table.RenameColumns(#"Converted to table", {{"Column1", "Data"}}),
  #"Changed column type" = Table.TransformColumnTypes(#"Renamed columns", {{"Data", type date}}),
  #"Inserted year" = Table.AddColumn(#"Changed column type", "Rok", each Date.Year([Data]), type nullable number),
  #"Inserted month" = Table.AddColumn(#"Inserted year", "MiesiacNumer", each Date.Month([Data]), type nullable number),
  #"Inserted day name" = Table.AddColumn(#"Inserted month", "NazwaDzien", each Date.DayOfWeekName([Data]), type nullable text),
  #"Added Conditional Column" = Table.AddColumn(#"Inserted day name", "DzienTygodnia", each if [NazwaDzien] = "środa" then 3 else if [NazwaDzien] = "czwartek" then 4 else if [NazwaDzien] = "piątek" then 5 else if [NazwaDzien] = "sobota" then 6 else if [NazwaDzien] = "niedziela" then 7 else if [NazwaDzien] = "poniedziałek" then 1 else if [NazwaDzien] = "wtorek" then 2 else null, type number),
  #"Inserted month name" = Table.AddColumn(#"Added Conditional Column", "NazwaMiesiac", each Date.MonthName([Data]), type nullable text),
  #"Capitalized each word" = Table.TransformColumns(#"Inserted month name", {{"NazwaMiesiac", each Text.Proper(_), type nullable text}})
in
  #"Capitalized each word"
```

The calendar table enables:
•	Year-over-Year analysis
•	Monthly trends
•	Quarterly reporting
•	Time Intelligence functions in Power BI

## <h1 align="center">10. Query grupping</h1>

To have clean work I grupped queries into sections:

Before:
<p align="center">
  <img src="images/grupowanie-before.png" alt="grupowanie-before">
</p>

After:

<p align="center">
  <img src="images/grupowanie-after.png" alt="grupowanie-after">
</p>

