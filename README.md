# SQL_XML_TABLE #

An object that allows you to easily create and work with tables, while also importing/exporting them from/to SQL databases or XML files, it can serve as a way to send data back and ford between XML files and SQL databases.



```
#!python

From sql_xml_table import Table
```


It defines an object called Table, it can be created with a name for identification an a pre-created connection object of a pep-246 compatible database interface.


```
#!python

table = Table('Animals') 
```

Then you need to add columns with the agregar_columna (add_column) method, with can take various key word arguments:

  - campo (field): the name of the field

  - tipo (type): the type of data stored, can be a things like 'varchar' and 'double' or name of python objects if you aren't interested in exporting to a data base latter.

  - defecto (default): set a default value for the column if there is none when you add a row

  - there are other 3 but are only there for database tings and not actually functional

like:


```
#!python
Table.agregar_columna(campo='Name', tipo='str')
Table.agregar_columna(campo='Year', tipo='date')

declaring it date, time, datetime or timestamp is important for being able to store it as a time object and not only as a number, But you can always put it as a int if you don't care for dates

Table.agregar_columna(campo='Priority', tipo='int')
```

Then you add the rows with the += operator (or + if you want to create a copy with an extra row)


```
#!python

Table += ('Cat', date(1998,1,1), 1)
Table += {'Year':date(1998,1,1), 'Priority':2, Name:'Fish'}
#â€¦
#The condition for adding is that is a container accessible with either the column name or the position of the column in the table
```

Then you can generate XML and write it to a file with exportar_XML (export_XML) and escribir_XML (write_XML):


```
#!python

file = os.path.abspath(os.path.join(os.path.dirname(__file__), 'Animals.xml'))
Table.exportar_xml()
Table.escribir_xml(file)
```


And then import it back with importar_XML (import_XML) with the file name and indication that you are using a file and not an string literal:


```
#!python

Table.importar_xml(file, tipo='archivo')
#archivo means file
```


# Advanced #

This are ways you can use a Table object in a SQL manner.

## UPDATE <Table> SET Name = CONCAT(Name,' ',Priority), Priority = NULL WHERE id = 2 ##

```
#!python

for row in Table:
    if row['id'] == 2:
        row['Name'] += ' ' + row['Priority']
        row['Priority'] = None
print(Table)
```


## DELETE FROM <Table> WHERE MOD(id,2) = 0 LIMIT 1 ##

```
#!python

n = 0
nmax = 1
for row in Table:
    if row['id'] % 2 == 0:
        del Table[row]
        n += 1
        if n >= nmax: break
print(Table)
```


this examples assume a column named 'id' but can be replaced width row.pos for your example.

```
#!python


if row.pos == 2:
```

