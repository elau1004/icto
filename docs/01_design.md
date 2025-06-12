# Design

You should not jump straight into coding.  From agood design we shall generate majority of the scaffolding of your application.

## Database Design
You can design you application approaching it either from the UI or from the database.  We believe that the designed should be captured in the data model.  Everything in the UI MUST not be hard coded and should be retrieved from persisted storage.  The UI is primary purpose is to capture and display data.

It is much easier to change the tools and the presented UI than to migrate off your data to a different platform.

### Consistency
Consistency helps with the future maintenance of your application.  Your code will out live your tenure of your role at your organization.

> Profit is in the operation.

Good convention starts with coding standards.

The following are the convention we shall use:
1. Use [Snake Case](https://en.wikipedia.org/wiki/Snake_case)
1. Italized each word.  Go talk to Java.
1. Length of entities and their attributes must be less than or equal to **30** characters.  In order to be generic and portable, we have to keep this rule to the lowest common denominator.  Go talk to Oracle.  SEE: [Cross DBMS DDL spec](https://github.com/elau1004/Cross-DBMS-TabCol-Spec).
1. Due to the above limitation, good abbreviation should be used.  SEE: Suggested [approved abbreviation](https://github.com/CA-CST-SII/Software-Standards/blob/master/SQL%20Server%20Coding%20Standards.md).


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

