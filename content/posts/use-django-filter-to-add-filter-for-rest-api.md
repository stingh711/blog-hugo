+++
title = "Use django-filter to add filter for REST api"
date = 2018-06-23
tags = ["python", "django"]
type = "post"
draft = false
+++

For a REST list api, filtering and sorting is the basic requirements. Using django-filter, we can add these functions with only a few lines of codes.

For example, if we have a model as follows,

```python
class Product(models.Model):
    name = models.CharField(max_length=100)
    created_at = models.DatetimeField()
```

We will write relative serializer and view.

```python
class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        fields = '__all__'

class ProductListView(generics.ListView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```

If we don't use django-filter, we can override ProductListview's get\_queryset to implement filter. For example, to filter by name:

```python
def get_queryset(self):
    queryset = Product.objects.all()
    name = self.request.query_params.get('name', None)
    if name is not None:
        queryset = queryset.filter(name=name)
    return queryset
```

With django-filter, the code can be even simpler. We can just code like configuration to implement more advanced filters.
First, we install it with `pip install django-filter`.
Then, we add a filter class in our views.

```python
class ProductFilter(django_filters.FilterSet):
    sort = django_filters.OrderingFilter(fields=('created_at',))

    class Meta:
        model = Product
        fields = ['name',]
```

Then modify our ProductListView by adding following two lines

```python
filter_backends = (DjangoFilterBackend,)
filter_class = ProductFilter
```

Now filter by name and sorting by created\_at are both implemented. If the original list url is `/products/`, to search by name, the url is `/products/?name=iPhone`; to order by created\_at desc, the url is `/products/?sort=-created_at`.

Django-filter can implement more advanced features, e.g, filter by range, fuzzy search. Please refer to [Official doc](https://github.com/carltongibson/django-filter).