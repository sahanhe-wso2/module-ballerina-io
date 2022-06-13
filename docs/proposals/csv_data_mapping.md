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


Testing
-------

Testing of data mapping with suitable tests as following is required. 
```
type Employee4 record{
    string id;
    int age;
}
@test:Config {}
isolated function testTableContent5() returns error? {
    string filePath = RESOURCES_BASE_PATH + "datafiles/io/records/sample11.csv";
    int expectedValue = 10;
    int total = 0;

    Employee4[] Result = check fileReadCSV(filePath);
    foreach Employee4 x in Result {
        total = total + x.age;
    }
    test:assertEquals(total, expectedValue);
    check csvChannel.close();
}

```