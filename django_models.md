# Model Notes

Model is the representation of table in the database. It contains fields & behaviour of the data you're storing. 
Example of model defining Employee with name & company_name fields: 
```
from django.db import models

class Employee(models.Model):
    name = models.CharField(max_length=64)
    company_name = models.CharField(max_length=64)
```
Here, this above Person model will create a table in database with name `appname_person` with two columns with names `name` & `company_name`. The table name is given by django automatically to save user's time but if we want to replace table name with our suggested name, we can do it by overriding the database table name, using the db_table parameter in class Meta. 

Once we defined our models, we need to inform django that we're going to use this model. To do that we need to edit `settings.py` file in main projects directory. We have to add our application's name into the `INSTALLED_APPS` list so that django will be able to access the models. 
For example let's consider our app name is `myapp`,
```
INSTALLED_APPS = [
    ..,
    ..,
    'myapp',
    ..,
    ..,
]
```

After adding this app name to the `settings.py` file run the following command to create a migration file:  
```
python manage.py makemigrations
```
After creating migration file to view the sql of this migration file run the following command: 
```
python manage.py sqlmigrate app_name file_code 

# file_code is the number with which name of file is started. 
```
After all the verifications to reflect these changes into actual database run the following command: 
```
python manage.py migrate
```
To list down all the migrations we performed run the following command:
```
python manage.py showmigrations
```

## Fields in the Model:
Fields are most important & necessary part of each model. Each field is reference to the each column of the database table. Each column in database table has datatype like varchar, int etc. likewise each field of model also has type which maps to the datatype of database. 
Also, each field takes certain specific number of arguments. For example, `CharField` takes `max_length` argument which specifies the size of `VARCHAR` database field. Some of most commonly used field options are mentioned as follows: 
- null: If True, Django will store empty values as NULL in the database. Default is False.

- blank: If True, the field is allowed to be blank. Default is False.

- choices: A sequence of 2-tuples to use as choices for this field. If this is given, the default form widget will be a select box instead of the standard text field and will limit choices to the choices given. see [this](https://docs.djangoproject.com/en/3.0/ref/models/fields/#django.db.models.Field.choices) for more information & example. 

## Some properties of the field names: 
- default: Displays default value for a field. 
- help_text: Displays extra help with its form widget. 
- primary_key: Returns True if the field is primary key otherwise False. 
- unique: Returns True if the fiels is unique field otherwise False. 

``` 
NOTE: If you do not mention primary key to any of the field of that model then django will automatically creates primary key which is an id with autoincrement functionality. 
```

## Relationships: 
One the main goal of relational model is to have relation between different tables in database. Django offers ways to define the three most common types of database relationships: many-to-one, many-to-many and one-to-one.

1. Many-to-one relationships: To define many-to-one relationship, we use `django.db.models.ForeignKey`. `ForeignKey` field requires two arguments, one is `model name` to which its going to relate & another is `on_delete` option which to mention what to do after deleting one the values of that field. 
Let's take an example to show many-to-one relationship: 
```
from django.db import models

class ClassName(models.Model):
    ....
    ....
    pass

class Student(models.Model):
    class_name = models.ForeignKey(ClassName, on_delete=CASCADE)

``` 
2. Many-to-many relationships: To define many-to-one relationship, we use `django.db.models.ManyToManyField`. `ManyToManyField` requires one argument: the class to which the model is related.
```
from django.db import models

class Language(models.Model):
    ....
    ....
    pass

class Question(models.Model):
    ....
    ....
    language = models.ManyToManyField(Language)
```
3. One-to-one relationships: To define many-to-one relationship, we use `django.db.models.OneToOneField`. `OneToOneField` requires one argument: the class to which the model is related.
```
from django.db import models

class Address(models.Model):
    ....
    ....
    pass

class Person(models.Model):
    ....
    ....
    address = models.OneToOneField(Address)
```