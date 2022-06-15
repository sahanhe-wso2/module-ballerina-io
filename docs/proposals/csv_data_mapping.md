# Proposal: Support Data Mapping in CSV read/write operations

_Owners_: @sahanhe, @daneshk
_Reviewers_:  @daneshk    
_Created_: 
_Updated_: 
_Issue_: 


Summary
------
Current CSV read/write APIs supports reading and writing as string[][] values. In this proposal we propose to add data mapping to return an array of particular record defined by the user.

Goals
-----

Provide an option to map the data to array of particular records defined by the user. 

> Adding data mapping feature to CSV READ/Write operations to return an array of particular record defined by the user.

> Change ```fileReadCsv, fileWriteCsv``` to support data mapping.

> Carry out the data mapping by infering the Return type automatically without passing a function parameter

Motivation
----------

Until now, user doesn't have a direct data mapping method to get an array of user defined records from CSVs.  

Description
-----------
```
//user defined record
type foo record{
    string foo_1;
    int foo_2;
}
```

```
//Return String[][]
string[][] result_1 = check fileReadCsv(filePath);
```

```
//Return array of records after data mapping
foo[] result_2 = check fileReadCsv(filePath);
```

```
//Takes String[][] as input
check fileWriteCsv(filePath, result_1);
```

```
//Takes array of records as input
check fileWriteCsv(filePath, result_2);
```


Example
-------

Example of a data mapping in to array of records.

CSV file
```
id, name, salary
User1, WSO2, 10000.50
User2, WSO2, 20000.50
User3, WSO2, 30000.00
```

Code

```
type Employee record {
    string id;
    string name;
    float salary;
};


@test:Config {}
isolated function testTableContent5() returns error? {
    float expectedValue = 60001.00;
    float total = 0;

    Employee[] Result = check fileReadCSV(filePath);
    foreach Employee x in Result {
        total = total + x.age;
    }
    test:assertEquals(total, expectedValue);
    check csvChannel
    check csvChannel.close();
    fileWriteCsv(filePath, Result);
}

```

Risks and Assumptions
--------

1. Headers of the record is provided in the first row of CSV, and they are equal to the headers in record type.
2. Number of fields in defined record and the number of headers in the CSV file are equal.