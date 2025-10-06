# Design

You should not jump straight into coding.  From a good design we shall generate majority of the scaffolding of your application.

You can design you application approaching it either from the UI or from the database.  The UI is primary purpose is to capture and display data.  However, not  all data can be captured through an UI, for example external data integration.  Captured data shall be persisted in structures in a database that will be eventually queried.  Our methodology is to first defined the data model to capture all pertinent data that shall be displayed or quired.

## Database Design
We shall build upon [Grokking Relational Database Design](https://www.manning.com/books/grokking-relational-database-design) by adding our opinionated comments shaped by our experience without repeating what is written in the book.

## Grokking Relational Database Design
### Chapter 1
> [!IMPORTANT]
> Read it carefully.

### Chapter 2
> [!IMPORTANT]
> Read it carefully.

### Chapter 3
#### Data consistency and integrity
* We must strive to not pollute an object.  As the saying goes:
> [!WARNING]
> Garbage in, garbage out.
* Always defined your constraints.  They are part of your object documentation.
  * Domain values **must** be checked.
  * Foreign keys **must** be defined.
* You can temporarily disable the constraints to improve performance for bulk data loading.  After the load operation, you **must** validate the data.
* Sometimes the constraints are a pain in butt but it is worth it.  The amount of data cleaning due to data pollution is **not** worth your time.

#### Maintainability and ease of use
* We feel that this is the most important point for human cost is most expensive.  You code becomes legacy the moment is done coding.  Your code will out live your tenure of your role at your organization.
> [!IMPORTANT]
> Code is written for the next person or your future self to support.  Do the best job up front so that you don't be cursed out.
* Naming convention.
The following are the convention we shall use:
  * Use [Snake Case](https://en.wikipedia.org/wiki/Snake_case).
  * Italicized each word.  This makes it easier to target language that prefer mixed cases.
  * Length of entities and their attributes **must** be less than or equal to **30** characters.  In order to be generic and portable, we have to keep this rule to the lowest common denominator.  SEE: [Cross DBMS DDL spec](https://github.com/elau1004/Cross-DBMS-TabCol-Spec).
  * Due to the above limitation, good abbreviation should be used.  SEE: [CA-CST-SII approved abbreviation](https://github.com/CA-CST-SII/Software-Standards/blob/master/SQL%20Server%20Coding%20Standards.md) as starting point.  The following are augmentation to the above suggested standard.
    | Category       | Business Name  | Abbreviation | Comment |
    |----------------|----------------|--------------|---------|
    | Temporal       | Year to Date   | `ytd`        |         |
    |                | Quarter to Date| `qtd`        |         |
    |                | Month to Date  | `mtd`        |         |
    |                | Week to Date   | `wtd`        |         |
    |                | Day to Date    | `dtd`        |         |
    |                | Date time      | `dtm`        |         |
    | Reference      | Identification | `id`         |Internally generated unique integer.|
    |                | Code           | `code `      |Short alphanumeric string starting with a alphabet.|
    |                | Number         | `no `        |Externally unique reference.|
    | Measure        | Quantity       | `qty`        |         |
    |                | Price          | `prc`        |         |
    |                | Amount         | `amt`        |         |
    |                | Total          | `ttl`        |         |

#### Performance and optimization
* You only insert once, update a little, and query a lot.
* Your most populate queries access pattern need to be identified early.  Try to load up your database early to get a feel for your expected performance.
* Do **NOT** create single column indices except for the primary key.

#### Data security
* This must **not** be an after thought.  The following columns are an immediate call out to be addressed:
  * Human related:
    * Names
    * Dates
    * Externally issues IDs and account numbers
  * Financial related
* Keep your attack surface as small as possible.  Don't have all then sensitive data in one location.  Do **NOT** have simple `SELECT` query display many of your sensitive data to the screen.  Don't short-cut data privacy.  You need to build a decipline around protecting your data.

#### Scalability and flexibility
* We believe a well designed data model can yield tremendous performance **without** external caching.  It should **not** be the first go to solution before you have examine your design.

### Chapter 4
#### Entities and attributes
* We shall capture our design into an E-R diagrams.  We shall use **[ERD Concepts](https://www.erdconcepts.com/download.html)** as our tool of choice.  The data model format is stored in XML which makes it easier to parse by our code generator.
  * Do **NOT** use SQL reserved words.
  * We shall use **singular** form to name entities for we intent to comply with Object Oriented naming convention.
  * We shall use **crow's foot** notation.
  * We shall use the **pre-configured** template to start all design exercises.

#### Keys
* All entities **must** have a surrogate primary key that is an integer.
* You may have an addition candidate key for uniqueness.
* Do **not** predefined keys that you may think you need unless your have the benchmark to prove it.
  * Super keys may help with report performance for these kind of query retrieve a lot of data.
  * Some of the columns are for filtering.

#### Data types
* Use the smallest data type for your attributes.  The data types are part of your documentation.  For example, there are 195 recognize independent sovereign countries and we should use a `tinyint`, single byte, as its `ID` instead of `INT` data type which is **4** bytes and can store a maximum **2,147,483,647** positive values.  This does **not** convey that we only have a small dataset.
* Numeric data types are preferred over string data types.  Depending on the target DBMS, do **NOT** store lookup codes as strings just to make your your SQL easier to write.
* If you have to mix currencies with and without decimals, use `int` and deal with the decimal via a currency code qualifier.
* We suggest to store the `Date` and `Time` component separately instead of `Timestamp` to provide the flexibility to index just the data portion.
* Do not use `GUID` or `UUID`.  There are to store client side generate unique IDs.  Chances are you do not have a use case to have a globally distributed database.  A centralized generated `ID` is sufficient.  However, if you truly have a use case for it and you need less than nine gloablly distributed databases, you can use a sequence object to generate IDs incremented by 10.
* Always setup your database in **UTF-8** character set with the **UTC** timezone.

### Chapter 5
#### Cardinality

### Chapter 6
#### There are no multivalued columns
* Don't store array in your column even though the DBMS may have an `ARRAY` data type.  It is hard to check for value array value.  It is less portable to another DBMS.

#### NOT NULL
* We disagree that allowing `NULL` values for columns may lead to unexpected behaviors in SQL.  This can arises for a lack of understanding the behavior of the `NULL` value.
* Any column to store data to be collected in the future should be `NULL`able.
* Do **NOT** use sentinel value in place of `NULL` value. e.g. Do use `2999-12-31` for `Decease_Date`.

#### Referential actions
* If every entity primary key is a surrogate key, there are less issues dealing with changing the primary key value in the parent table.
  * 
* One characteristic of "Enterprise" systems is the auditability of DML operations.  This can be implemented using soft deletes implemented using a flag to denoting a row as deleted.  Furthermore, the timestamp of the operation is also captured.

#### Default
* Don't depend on the consistency of the engineers to do the correct thing.  If the system can determine the value, let the system do it.

#### Check
* Constrain the domain for you column. e.g. If there are no negative values for your primary key, constraint it to be values above zero.  Remember these are part of your data model documentation.


### Chapter 7
#### Confidentiality
* Generally the following are types of users:
  1. Admin - these users have access to manage the database but not able to view data.
  1. Agents - these the system automation.
  1. Basic users - these users have minimum access
  1. Managers - these users have more privileges over basic users.
* You can further sub-divide each of the above category into more specific task to be performed.
* Do setup DBMS roles and assign the above users to it.

#### Encryption
* Do encrypt sensitive data with a salt.  Do consider bulk import of your data.  Calling individual API for a single encryption operation is not scalable.

#### Denormalization
* Denormalization requires additional aggregation processing with can add more complexity into your system.
* Many decades ago joining tables is very slow.  Since then, DBMS vendors have invested tremendously in improving their join algorithm.  Today, join are a lot faster and not to be considered a boogie man.
* Most big query operation is to filter out the rows you don't need.  You should consider in your design a very high filterable index.

### Chapter 8


### Tools
The following are design tools considered.
* [dbDesigner4](https://fabforce.net/dbdesigner4/).  Pretty good but not stable.  Metadata is stored in XML format.
* [PlantUML](https://plantuml.com/).  Currently, it is hard to parse the metadata file.
  * The Python [Lark parser](https://github.com/lark-parser/lark) is available.
  * This [Python PlantUML parser](https://github.com/pjcuadra/plantuml-parser) is still under development.
  * SEE: [PlantUML Reference](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://deepu.js.org/svg-seq-diagram/Reference_Guide.pdf).
* [DBeaver](https://dbeaver.com/docs/dbeaver/ER-Diagrams/#structure-adjustment).  Need a database.
* [pgModeler](https://pgmodeler.io/).  Demo version does not save work.
* [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-data-modeling.html).  Need to try it out.
* [ERD Concepts](https://www.erdconcepts.com/download.html).  Very similar to `dbDesigner4`.

Ideally, the tool should be an open source cross platform that store it's metadata format that is easy to parse.  We should be able to annotate entities and attributes using punctuation characters.  We should be able to specify initial seed data to populate the objects.
