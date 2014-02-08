SQLBuilder For Delphi
=================

Generates Delphi code from your database and lets you build typesafe SQL queries through its fluent API.

SQLBuilder4Delphi is a little Delphi library for dynamically generating SQL statements. It's sweet spot is for applications that need to build up complicated queries with criteria that changes at runtime. Ordinarily it can be quite painful to figure out how to build this string. SQLBuilder4Delphi takes much of this pain away.

The code for SQLBuilder4Delphi is intentionally clean and simple. Rather than provide support for every thing you could ever do with SQL, it provides support for the most common situations and allows you to easily modify the source to suit your needs.

The SQLBuilder4Delphi API was developed and tested in Delphi XE5.


External Dependencies
=================
This library requires the following external dependencies that are already included in their sources:


- Delphi Spring Framework (https://bitbucket.org/sglienke/spring4d);

Features
========

- Concise and intuitive API;
- Simple code, so easy to customize;
- Small, lightweight, fast;
- Generates clean SQL designed that is very human readable;
- Supports SELECT, UPDATE, INSERT and DELETE statements;   
- Suports JOIN, UNION, SUB-SELECT, WHERE, GROUP BY, HAVING, ORDER BY;
- Combine criteria with AND, OR and NOT operators;

Examples
=========

SELECT

    TSQLBuilder
      .Select
      .Column('C_Code')
      .Column('C_Name')
      .Column('C_Doc')
      .From('Customers')
      .Join('Places', '(Customers.P_Code = Places.P_Code)')
      .LeftOuterJoin('Places', '(Customers.P_Code2 = Places.P_Code2)')
      .RightOuterJoin('Places', '(Customers.P_Code = Places.P_Code)')
	  .Where('C_Code').Criterion(opEqual, 1)
      .GroupBy.Column('C_Code').Column('C_Name')
      .Having.Aggregate('(C_Code > 0)')
      .OrderBy.Column('C_Code').Column('C_Doc')
      .ToString;
	
	//==Result==\\
    Select
      C_Code, C_Name, C_Doc
      From Customers
      Join Places On (Customers.P_Code = Places.P_Code)
      Left Outer Join Places On (Customers.P_Code2 = Places.P_Code2)
      Right Outer Join Places On (Customers.P_Code = Places.P_Code)
	  Where (C_Code = 1)
      Group By C_Code, C_Name
      Having ((C_Code > 0))
      Order By C_Code, C_Doc

DELETE

    TSQLBuilder
      .Delete
      .From('Customers')
      .Where('C_Code').Greater(1)
      ._And('C_Name').Different('Ejm')
      ._And(
         TSQLBuilder.Where.Column('C_Code').InList([1, 2, 3])
      ._Or('C_Code').Less(10)
      ).ToString;

    //==Result==\\
    Delete From Customers
      Where (C_Code > 1) And (C_Name <> ''Ejm'') And 
    	((C_Code In (1, 2, 3)) Or (C_Code < 10))

UPDATE

    TSQLBuilder
      .Update
      .Table('Customers')
      .ColumnSetValue('C_Code', 1)
      .ColumnSetValue('C_Name', 'Ejm')
      .Where('C_Code').Equal(1)
      ._And(
          TSQLBuilder.Where.Column('C_Name').Equal('Ejm')
      ._Or('C_Name').Different('Ejm')
      ).ToString;
      
    //==Result==\\
    Update Customers Set
      C_Code = 1,
      C_Name = 'Ejm'
      Where (C_Code = 1) And 
    	   ((C_Name = 'Ejm') Or 
    	   (C_Name <> 'Ejm'))

INSERT

    TSQLBuilder
      .Insert
      .Into('Customers')
      .ColumnValue('C_Code', 1)
      .ColumnValue('C_Name', 'Ejm')
      .ColumnValue('C_Doc', 58)
      .ToString;
      
     //==Result==\\
     Insert Into Customers
      (C_Code,
       C_Name,
       C_Doc)
     Values
       (1,
       'Ejm',
       58);

Using SQLBuilder4Delphi
========================

Using this library will is very simple, you simply add the Search Path of your IDE or your project the following directories:

- SQLBuilder4Delphi\dependencies\Spring4D\Source\Base
- SQLBuilder4Delphi\dependencies\Spring4D\Source\Base\Collections
- SQLBuilder4Delphi\dependencies\Spring4D\Source\Base\Reflection
- SQLBuilder4Delphi\dependencies\Spring4D\Source\Core\Container
- SQLBuilder4Delphi\dependencies\Spring4D\Source\Core\Services
- SQLBuilder4Delphi\src\

Then you should add to your DPR a Unit responsible for making the records to be used in Dependency Injection.: 

Add Uses of DPR, preferably at the end of the next row.: 

- SQLBuilder4D.Module.Register

Analyze the unit tests they will assist you.