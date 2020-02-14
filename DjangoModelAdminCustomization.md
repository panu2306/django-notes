# DJANGO_ADMIN_CUSTOMIZATION:

## To change site_header, site_title & index_title: 
Go to `urls.py` file of main project folder & add following lines in that file: 
```
admin.site.site_header = "SITE_NAME"
admin.site.site_title = "SITE_NAME"
admin.site.index_title = "SITE_INDEX_NAME"
```

## To set correct/custom plural text for a model: 
While displaying model in index admin page, django just adds 's' for making it plural text. But, not every word becomes plural by adding 's'. In that case we need to provide custom plural name. To add plural text for model we just need to add `verbose_name_plural` in class Meta(inner class of actual model) of that particular class. The example is as shown below: 
```
class ModelA(models.Model):
	...
	...
	class Meta:
		verbose_name_plural = "name_you_want_to_provide" 

```



