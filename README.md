# Relational-Database
CECS 323 - Project 2
## Project 2: Databasic Instinct
You may work on and submit this project with one other person.
 

### Overview
In this project, you will diagram and implement the canonical UML model from project 1 as a relational database in Postgres. Your final deliverables will consist of a ER diagram following the style shown in class, a series of CREATE TABLE statements generated by DataGrip (after you use it to create the tables in your design), and several SQL SELECT queries using your design.

### Enterprise Changes
There is one change to the enterprise that you will have to incorporate into your answer, because it is not shown in the UML. In addition to Rooms at a theater having many Formats they support, we will also require each Showing to choose the one (only) Format that is being presented at that Showing. For example, a Showing will now indicate its showtime ("Friday October 21 at 5pm"), its Room ("Screen 5 at AMC Marina Pacifica"), its Film ("Spider-man: No Way Home"), and now its Format ("IMAX").

### Diagramming the Design
Begin by translating the UML design to an entity-relationship diagram. Each class in the UML will become a table in your database, including the Rating enumeration class, and the attributes of the classes will become columns in the corresponding table. Follow these rules:

Tables are named in plural lowercase, e.g., "theaters".
Use "int", "float", "date", and "timestamp" for the simple primitive attributes. Use a varchar(100) for most string attributes, unless the string is always a particular length, like ZIP code. 
SQL does not support enumerations, so Rating becomes a regular class with a "name" attribute, and an association with Film.
You will have to decide on a primary key for each table:
You can choose one of the existing attributes, if you can guarantee that attribute is unique across all possible objects.
If no such natural attribute exists, you can introduce a serial surrogate key.
You will also need to introduce a surrogate for any table whose chosen primary key is larger than 8 bytes total, only if that table is the parent in a one-to-many association. (I.e., if another table is going to copy your table's PK as its own foreign key.)
Diagram the primary key column(s) as shown in lecture: in bold, above the horizontal line separating PKs from other fields, and with "PK" in the left column of the diagram.
Introduce junction tables needed for all many-to-many associations, as shown in lecture. 
Make sure you pay attention to the rules for primary keys. Some junction tables will have a large primary key, and that is OK if they are never the parent of an association.
Show associations between tables using "crow's foot" notation:
Add a copy of the parent table's PK to the child table, and mark it as "FK#" in the left column, replacing # with 1, 2, 3, etc.
Connect the FK column(s) to the PK column(s) and add appropriate crow's foot symbols at each end.
Add unique keys to columns that require uniqueness, but are not the primary key of a table. Number them in the left column as UK1, UK2, etc.

### Implementing the Design
Implement your finished design in Postgres, using DataGrip to design the tables. (You can write CREATE TABLE statements by hand if you wish, but I recommend using the DataGrip UI.)

Starting with a table that doesn't have any parents which haven't already been designed in DataGrip, make sure that you:

Add the columns of the table with the appropriate data types.
Use the Keys folder to add the table's primary key and any unique keys.
Use the Foreign Keys folder to add foreign keys for its associations, choosing the target table (the parent), your table's foreign key column, and the parent's primary key column.
Submit, then move on to the next table.

You will want to make slow and steady progress on this step. Don't leave it all to a single 2-hour session the night the project is due. You will make mistakes and you will get frustrated trying to fix them.

### Instantiating the Design
Using DataGrip's UI, or by writing INSERT statements by hand (recommended if you want to spend some extra time getting better at SQL), add rows to your tables to reflect the following enterprise objects:

A theatre named "Regal Edwards Long Beach", at 7501 E Carson St, Long Beach, CA 90808
One room at this theater, named "Screen 1".
This room supports two Formats: "Standard" and "IMAX". (You will need to create rows for these formats, then insert into the rooms/formats junction table.)
This room has five seats:
Seats 1, 2, 3, and 4 are labeled as "reclining".
Seat 5 has two seat labels: "accessible seating" and "non-reclining".
A film named "Wakanda Forever", release date November 10 2022, duration 161 minutes, rating PG-13. 
Genres: Action, Adventure, Superhero
The film has two showings, both in Screen 1 of the Regal Edwards Long Beach theater:
November 10 2022 at 3:45 PM in the Standard format.
November 10 2022 at 7:00 PM in the IMAX format.
Three tickets have been sold:
Ticket 1 for Seat 1 for the 3:45 PM showing, $18.
Ticket 2 for Seat 1 in the 7:00 PM showing, $22.
Ticket 3 for Seat 5 in the 7:00 PM showing, $15.
If you have implemented your design correctly, the following rows should be rejected by the database as violating one of the constraints, assuming you have inserted all the rows already mentioned:

A room named "Screen 2" at theater ID 9999, which does not exist. [If this is accepted, you did not set the FK correctly.]
Trying to associate Wakanda Forever with a Genre that does not exist in your database, like "Romance".
Ticket 4 for Seat 1 at the 3:45 PM showing, $20. [Should violate a primary or unique key.]

### Queries
Finally, you must write SQL SELECT statements to answer the following queries using your design. You will need to use aggregates and subqueries for most of them. You are welcome to use "SELECT ... FIRST X ROWS WITH TIES" when relevant.

Note: the data from the previous section won't be sufficient to test each of these queries. You can add your own objects if it helps.

Select the title of all films that have at least one showing in the IMAX format. [Be wary of duplicates!]
Select the name of all theaters that have no showings in the IMAX format.
Select the name of all rooms that do not have any seats with the "accessible seating" label.
Select the primary key of the showing that has brought in the most income: the sum of the price of every ticket sold for that showing.
Count the number of "short", "average", and "long" films. A short film is under 90 minutes; an average film is between 90 and 120 minutes; a long film is over 120 minutes.

### Deliverables
You must deliver to me, via Dropbox, a single PDF containing the following documents:

Title page (name of course, title of assignment, team member names, due date)
ER diagram for the entire system
CREATE TABLE statements for each table. 
After designing your tables in DataGrip, you can select them all in the Database Explorer side window, then right click -> SQL Scripts... -> Generate DDL to query console. Easy!
INSERT statements for the rows you created.
After inserting all the required rows, right-click the schema in Database Explorer, then select Export Data to Files. Choose the Extractor to be "SQL-Insert-Multirow", then choose an output directory. You will get one SQL file per table; copy them to your document.
The queries you wrote to solve the 5 questions. I do not need the output of those queries, just the statements themselves.
