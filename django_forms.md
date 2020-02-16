Whenever a site wants user to feed data into it forms come into handy. Forms are essential part of any site/web-app. Django helps to build the forms very easily. Django's Form Class helps to create forms easily. A Form class describes a form and determines how it works and appears. 

If you have worked with models in django then you will find similarities between django Form class & Model class. While django Model class represents database fields, Form class represents HTML form <input> elements. It is as simple as that. 

A form’s fields are themselves classes; they manage form data and perform validation when a form is submitted. A form field is represented to a user in the browser as an HTML “widget” - a piece of user interface machinery. Each field type has an appropriate default Widget class, but these can be overridden as required.

### Creating a form in django: 
#### 1. Using Pure HTML:
Suppose you want to create a form on website in order to sign up for your website. The HTML form might look like below: 
```
<form action="/sign-up/" method="post">
    <label for="your_name">Name: </label>
    <input id="your_name" type="text" name="your_name" value="{{ name }}">
    <label for="your_email">Email: </label>
    <input id="your_email" type="text" name="email_id" value="{{ email }}">
    <label for="password">Password: </label>
    <input id="password" type="text" name="password" value="{{ password }}">
    <label for="confirm_password">Confirm Password: </label>
    <input id="confirm_password" type="text" name="confirm_password" value="{{ confirm_password }}">
    <input type="submit" value="OK">
</form>
```
This tells the browser to return the form data to the URL /sign-up/, using the POST method. When the form is submitted, the POST request which is sent to the server will contain the form data. Now you’ll also need a view corresponding to that /sign-up/ URL which will find the appropriate key/value pairs in the request, and then process them.

#### 2. Using Django's Form Class: 
To build above same form but by using Django's specification we need to create a `forms.py` file inside our application's directory & in that we need to import forms as shown below: 
```
from django import forms 

class SignUpForm(forms.Form):
	name = forms.CharField(label="Name", max_length=64)
	email = forms.EmailField(label="Email", max_length=64)
	password = forms.CharField(label="Password", max_length=32, widget=forms.PasswordInput)
	confirm_password = forms.CharField(label="Confirm Password", max_length=32, widget=forms.PasswordInput)
```
This defines a form for Sign-Up functionality in your website. The CharField() & EmailField() are type of fields to represent type of data. For more information regarding form fields visit [here](https://docs.djangoproject.com/en/3.0/ref/forms/fields/#module-django.forms.fields). Also, label argument is to meto mention the human-friendly label for that particular field.

A Form instance has an is_valid() method, which runs validation routines for all its fields. When this method is called, if all fields contain valid data, it will:
 	- return True
	- place the form’s data in its cleaned_data attribute.

Now, the above class SignUpForm is just a representation of the fields to be shown in the form. To actually render the form we need to create a view function & a template to render the form. 
First we will see how view will look like:
```
from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import SignUpForm

def get_sign_up_form():
	# if this is a POST request we need to process the form data
    	if request.method == 'POST':
        	# create a form instance and populate it with data from the request:
        	form = SignUpForm(request.POST)
        	# check whether it's valid:
        	if form.is_valid():
            		# process the data in form.cleaned_data as require
            		# redirect to a new URL:
            		return HttpResponseRedirect('/submit/')

    	# if a GET (or any other method) we'll create a blank form
    	else:
        	form = SignUpForm()
    	return render(request, 'sign_up_form.html', {'form': form})
```
If we arrive at this view with a GET request, it will create an empty form instance and place it in the template context to be rendered. This is what we can expect to happen the first time we visit the URL.

If the form is submitted using a POST request, the view will once again create a form instance and populate it with data from the request: form = NameForm(request.POST) This is called “binding data to the form” (it is now a bound form).

We call the form’s is_valid() method; if it’s not True, we go back to the template with the form. This time the form is no longer empty (unbound) so the HTML form will be populated with the data previously submitted, where it can be edited and corrected as required.

If is_valid() is True, we’ll now be able to find all the validated form data in its cleaned_data attribute. We can use this data to update the database or do other processing before sending an HTTP redirect to the browser telling it where to go next.

Now, that we have written a view function to describe how form will be rendered & submitted. We are finally at a point of writing template for the form we have created. 
The template will look like as follow:
```
<form action="/submit/" method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Submit">
</form>
```
Here, few points need to understand regarding form template: 
1. The method should be POST method which is a HTTP method mainly used for adding data to server. 
2. When we use POST method everytime we need to specify `{% csrf_token %}` for securely transffer of our data. For more details visit [here](https://docs.djangoproject.com/en/3.0/ref/csrf/). 
3. `{{ form.as_p }}` will render the fields we have mentioned in `forms.py` file. `as_p` is just to display the field one below another(using line-breaks).

