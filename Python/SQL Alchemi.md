## Models

#### Listing 3 db.py: Create a declarative base class

```python
import os
from dotenv import load_dotenv
from sqlalchemy import create_engine
from sqlalchemy.orm import DeclarativeBase

class Model(DeclarativeBase):
	pass
	
load_dotenv()
engine = create_engine(os.environ['DATABASE_URL'])
```

#### Listing 4 models.py: Product model class
```python
from sqlalchemy import String
from sqlalchemy.orm import Mapped, mapped_column
from db import Model

class Product(Model):
	__tablename__ = 'products'
	id: Mapped[int] = mapped_column(primary_key=True)
	name: Mapped[str] = mapped_column(String(64))
	manufacturer: Mapped[str] = mapped_column(String(64))
	year: Mapped[int]
	country: Mapped[str] = mapped_column(String(32))
	cpu: Mapped[str] = mapped_column(String(32))
	
	def __repr__(self):
		return f'Product({self.id}, "{self.name}")'
```

#### Listing 5 db.py: Configure naming conventions for indexes and constraints
```python
import os
from dotenv import load_dotenv
from sqlalchemy import create_engine, MetaData
from sqlalchemy.orm import DeclarativeBase

class Model(DeclarativeBase):
	metadata = MetaData(naming_convention={
		"ix": "ix_%(column_0_label)s",
		"uq": "uq_%(table_name)s_%(column_0_name)s",
		"ck": "ck_%(table_name)s_%(constraint_name)s",
		"fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",
		"pk": "pk_%(table_name)s",
	})
	
load_dotenv()
engine = create_engine(os.environ['DATABASE_URL'])
```
### Query Definition
```python
>>> from sqlalchemy import select
>>> q = select(Product)
```
### Query Execution
```python
>>> r = session.execute(q)
>>> list(r)
[(Product(1, "Acorn Atom"),), (Product(2, "BBC Micro"),), ..., (Product(149, "GEM 1000"),)]
```
```python
>>> session.execute(q).all()
[(Product(1, "Acorn Atom"),), (Product(2, "BBC Micro"),), ..., (Product(149, "GEM 1000"),)]
```
In addition to all(), the results object has other methods that retrieve the
first result of a query, which is a very common need:
- first() returns the first result row, or None if there are no results. If
there are any more rows in the result set, they are discarded.
- one() returns the first and only result. If there are zero or more than
one result rows, an exception is raised.
- one_or_none() returns the first and only result, or None if there are no
results. If there are two or more result rows, an exception is raised.

If you know that you will be receiving a single value per row, then you can
use the scalars() convenience method to execute the query:
```python
>>> session.scalars(q).all()
[Product(1, "Acorn Atom"), Product(2, "BBC Micro"), ..., Product(149, "GEM 1000")
```
Also as a convenience, there are some additional query execution methods
that combine scalars() with first(), one() and one_or_none(). Given a
query q, these are the additional methods:
- scalar(q) is the same as scalars(q).first() and returns the first
value of the first result row, or None if the query has no results.
- scalar_one(q) is the same as scalars(q).one() and returns the first
value of the only result row, or raises an exception if there are zero or
more than one result.
- scalar_one_or_none(q) is the same as scalars(q).one_or_none()
and returns the first value of the only result row or None if there are no
results. It raises an exception if there are two or more results.
The next example shows a compact way to get the first value of the first
```python
>>> r = session.scalar(q)
>>> r
Product(1, "Acorn Atom")
```

## Filters

```python
>>> q = (select(Product)
.where(Product.manufacturer == 'Commodore')
.where(Product.year == 1980))

>>> q = select(Product).where(Product.manufacturer == 'Commodore', Product.year == 1980)
```
Combining multiple filters as shown above effectively applies the AND
logical operator to them. Sometimes a query may need to combine filters
with the OR operator, which SQLAlchemy offers with the `ory_()` function.
The next example returns products built before 1970 or after 1990:
```python
>>> from sqlalchemy import or_
>>> q = select(Product).where(or_(Product.year < 1970, Product.year > 1990))
```
Even though it is implied when passing multiple arguments to the where()
clause, the AND logical operator can also be explicitly given using the
`and_()` function. The NOT unary operator is also available as the `not_()`
function.
Another very useful filter is the LIKE operator, which can be used to
implement a simple search function. The following example retrieves all
products that have the word Sinclair in their name:
```python
>>> q = select(Product).where(Product.name.like('%Sinclair%'))
```
Here are a few other example patterns for the `like()` filter:
- Sinclair% (items that start with Sinclair)
- %Sinclair (items that end with Sinclair)
- % Sinclair (items that end with a space, followed by Sinclair)
- R__% (items that start with the letter R followed by at least two more characters)
- _ (items that are one character long)
The like() function is case-sensitive. For case-insensitive searches, you can use the `ilike()` function instead.

A range of items can be requested with a where() clause defining two
conditions for the lower and upper bounds respectively, but it is more clear
to use the between() method exposed by column attributes of model
classes. The example below returns products that were made in the 1970s:
```python
>>> q = select(Product).where(Product.year.between(1970, 1979))
```

### Order of Results

The next example retrieves products alphabetically ordered by their names:
```python
>>> q = select(Product).order_by(Product.name)
>>> session.scalars(q).all()
[Product(10, "464 Plus"), Product(11, "6128 Plus"), ..., Product(127, "ZX Spectrum")]

>>> q = select(Product).order_by(Product.year.desc())
>>> session.scalars(q).all()
[Product(6, "A7000"), Product(33, "Falcon"), ..., Product(74, "Honeywell 316")]

>>> q = select(Product).order_by(Product.year.desc(), Product.name.asc())
```

### Access to Individual Columns
For example, an application may only need to retrieve an individual
column. In the next query, only the names of the products are retrieved:
```python
>>> q = select(Product.name)
>>> session.scalars(q).all()
['Acorn Atom', 'BBC Micro', ..., 'GEM 1000']

>>> q = select(Product.name, Product.manufacturer)
>>> session.execute(q).all()
[('Acorn Atom', 'Acorn Computers Ltd'), ('BBC Micro', 'Acorn Computers Ltd'), ...]
```

### Aggregation Functions
The next example finds out how many products are stored in the
database:
```python
>>> from sqlalchemy import func
>>> q = select(func.count(Product.id))
>>> r = session.scalar(q)
>>> r
149
```
```python
>>> q = select(func.count()).select_from(Product)
>>> r = session.scalar(q)
>>> r
149
```
Another pair of useful SQL functions are min() and max(). The example
that follows returns the first and last years in which products in the database
were manufactured:
```python
>>> q = select(func.min(Product.year), func.max(Product.year))
>>> r = session.execute(q)
>>> r.first()
(1969, 1995)
```
### Result Grouping
Whenever a database query can return duplicated results, the distinct()
clause added to it tells the database to combine identical results:
```python
>>> q = select(Product.manufacturer).order_by(Product.manufacturer).distinct()
>>> session.scalars(q).all()
['Acorn Computers Ltd', 'AGAT', ..., 'West Computer AS']
```
 When you need to count unique results, there is a distinct() method that can be called on the item being counted, inside the count() function:
```python
>>> q = select(func.count(Product.manufacturer.distinct()))
>>> session.scalar(q)
76
```
The group_by() clause offers a result grouping solution that has more
flexibility. The query above that returns the list of manufacturers can also
be created with group_by() as follows:
```python
>>> q = (select(Product.manufacturer)
.group_by(Product.manufacturer)
.order_by(Product.manufacturer))
```
 Similar to where(), the having() clause is used to filter the
grouped and aggregated results. Below is a query that gets a list of
manufacturers that have five or more models, along with the actual number:
```python
>>> q = (select(
Product.manufacturer,
func.count()
)
.group_by(Product.manufacturer)
.having(func.count() >= 5)
.order_by(Product.manufacturer))
>>> session.execute(q).all()
[('Acorn Computers Ltd', 6), ('Amstrad', 7), ..., ('Timex Sinclair', 6)]
```
To ensure that the count of products per manufacturer is written only once,
the label() method can be used to associate a label to the calculation, and
then the label can be used in the two locations it is needed:
```python
>>> num_products = func.count().label(None)
>>> q = (select(
Product.manufacturer,
num_products
)
.group_by(Product.manufacturer)
.having(num_products >= 5)
.order_by(Product.manufacturer))
```
### Pagination