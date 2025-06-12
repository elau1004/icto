# Design

You should not jump straight into coding.  From agood design we shall generate majority of the scaffolding of your application.

## Database Design
You can design you application approaching it either from the UI or from the database.  We believe that the designed should be captured in the data model.  Everything in the UI MUST not be hard coded and should be retrieved from persisted storage.  The UI is primary purpose is to capture and display data.

It is much easier to change the tools and the presented UI than to migrate off your data to a different platform.

SEE: [Grokking Relational Database Design](https://www.manning.com/books/grokking-relational-database-design)

### Consistency
Consistency helps with the future maintenance of your application.  Your code will out live your tenure of your role at your organization.

> Profit is in the operation.

Good convention starts with coding standards.

The following are the convention we shall use:
1. Use [Snake Case](https://en.wikipedia.org/wiki/Snake_case)
1. Italicized each word.  Go talk to Java.
1. Length of entities and their attributes must be less than or equal to **30** characters.  In order to be generic and portable, we have to keep this rule to the lowest common denominator.  Go talk to Oracle.  SEE: [Cross DBMS DDL spec](https://github.com/elau1004/Cross-DBMS-TabCol-Spec).
1. Due to the above limitation, good abbreviation should be used.  SEE: [CA-CST-SII approved abbreviation](https://github.com/CA-CST-SII/Software-Standards/blob/master/SQL%20Server%20Coding%20Standards.md).


#### Augmented suggestion
The following are augmentation to the above suggested standard.
| Category       | Business Name  | Abbreviation | Comment |
|----------------|----------------|--------------|---------|
| Temporal       | Year to Date   | `ytd`        |         |
|                | Quarter to Date| `qtd`        |         |
|                | Month to Date  | `mtd`        |         |
|                | Week to Date   | `wtd`        |         |
|                | Day to Date    | `dtd`        |         |
|                | Date time      | `dtm`        |         |
| Reference      | Identification | `id`         |Internal system generated unique reference.|
|                | Number         | `no `        |External unique reference.
| Measure        | Quantity       | `qty`        |         |
|                | Price          | `prc`        |         |
|                | Amount         | `amt`        |         |
|                | Total          | `ttl`        |         |

### Object design.
We must strive to not pollute an object.  As the saying goes:
> Garbage in, garbage out.

Use the smallest data type for your attributes.  The data types are part of your documentation.  For example, there are 195 recognize independent sovereign countries and we should use a `tinyint`, single byte, as its `ID` instead of `INT` data type which is **4** bytes and can store a maximum **2,147,483,647** positive values.  This does **not** convey that we only have a small dataset.

### Tools
The following are design tools considered.
* [dbDesigner4](https://fabforce.net/dbdesigner4/).  Pretty good but not stable.  Metadata is stored in XML format.
* [PlantUML](https://plantuml.com/).  Currently, it is hard to parse the metadata file.
  * The Python [Lark parser](https://github.com/lark-parser/lark) is available.
  * This [Python PlantUML parser](https://github.com/pjcuadra/plantuml-parser) is still under development.
  * SEE: [PlantUML Reference](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://deepu.js.org/svg-seq-diagram/Reference_Guide.pdf).
* [DBeaver](https://dbeaver.com/docs/dbeaver/ER-Diagrams/#structure-adjustment).  Need to test it out.
* [pgModeler](https://pgmodeler.io/).  Need to test it out.
* [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-data-modeling.html).  Need to try it out.

Ideally, the tool should be an open source cross platform that store it's metadata format that is easy to parse.  We should be able to annotate entities and attributes using punctuation characters.  We should be able to specify initial seed data to populate the objects.
