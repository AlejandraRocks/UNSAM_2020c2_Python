[Contenidos](../Contenidos.md) \| [Anterior (2 El módulo *main* (principal))](02_305Main_module.md) \| [Próximo (4 Contratos: Especificación y Documentación)](04_Especificación.md)

# 6.3 Temas de diseño

In this section we reconsider a design decision made earlier.

### Filenames versus Iterables

Compare these two programs that return the same output.

```python
# Provide a nombre_archivo
def read_data(nombre_archivo):
    records = []
    with open(nombre_archivo) as f:
        for line in f:
            ...
            records.append(r)
    return records

d = read_data('file.csv')
```

```python
# Provide lines
def read_data(lines):
    records = []
    for line in lines:
        ...
        records.append(r)
    return records

with open('file.csv') as f:
    d = read_data(f)
```

* Which of these functions do you prefer? Why?
* Which of these functions is more flexible?

### Deep Idea: "Duck Typing"

[Duck Typing](https://en.wikipedia.org/wiki/Duck_typing) is a computer
programming concept to determine whether an object can be used for a
particular purpose.  It is an application of the [duck
test](https://en.wikipedia.org/wiki/Duck_test).

> If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck.

In the second version of `read_data()` above, the function expects any
iterable object. Not just the lines of a file.

```python
def read_data(lines):
    records = []
    for line in lines:
        ...
        records.append(r)
    return records
```

This means that we can use it with other *lines*.

```python
# A CSV file
lines = open('data.csv')
data = read_data(lines)

# A zipped file
lines = gzip.open('data.csv.gz','rt')
data = read_data(lines)

# The Standard Input
lines = sys.stdin
data = read_data(lines)

# A list of strings
lines = ['ACME,50,91.1','Naranja,75,123.45', ... ]
data = read_data(lines)
```

There is considerable flexibility with this design.

*Question: Should we embrace or fight this flexibility?*

### Library Design Best Practices

Las bibliotecas de código suelen ser más útiles si son flexibles. No restrinjas las opciones. La más flexibilidad ganás más potencia.

## Ejercicio

### Ejercicio 6.6: From nombre_archivos to file-like objects
You've now created a file `fileparse.py` that contained a
function `parse_csv()`.  The function worked like this:

```python
>>> import fileparse
>>> camion = fileparse.parse_csv('Data/camion.csv', types=[str,int,float])
>>>
```

Right now, the function expects to be passed a nombre_archivo.  However, you
can make the code more flexible.  Modify the function so that it works
with any file-like/iterable object.  For example:

```
>>> import fileparse
>>> import gzip
>>> with gzip.open('Data/camion.csv.gz', 'rt') as file:
...      port = fileparse.parse_csv(file, types=[str,int,float])
...
>>> lines = ['name,cajones,precio', 'Lima,100,34.23', 'Naranja,50,91.1', 'HPE,75,45.1']
>>> port = fileparse.parse_csv(lines, types=[str,int,float])
>>>
```

In this new code, what happens if you pass a nombre_archivo as before?

```
>>> port = fileparse.parse_csv('Data/camion.csv', types=[str,int,float])
>>> port
... look at output (it should be crazy) ...
>>>
```

Yes, you'll need to be careful.   Could you add a safety check to avoid this?

### Ejercicio 6.7: Fixing existing functions
Fix the `leer_camion()` and `read_precios()` functions in the
`informe.py` file so that they work with the modified version of
`parse_csv()`.  This should only involve a minor modification.
Afterwards, your `informe.py` and `costo_camion.py` programs should work
the same way they always did.


[Contenidos](../Contenidos.md) \| [Anterior (2 El módulo *main* (principal))](02_305Main_module.md) \| [Próximo (4 Contratos: Especificación y Documentación)](04_Especificación.md)

