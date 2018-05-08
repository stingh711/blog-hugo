+++
menu = "main"
Categories = ["python"]
Tags = ["python", "django"]
Description = "Use django-filter to add filter for rest api"
date = "2018-05-01T13:13:00+08:00"
title = "Use django-filter to add filter for rest api"
+++

对一个 list api 来说，查询和排序都是基本的需求，使用 django-filter 可以在 DRF 中用很少的代码满足我们的需求。

比如说如下的 model:

```
class Product(models.Model):
  name = models.CharField(max_length=100)
  created_at = models.DatetimeField()
```

我们用 DRF 的 generic view 可以创建一个 list view：首先需要写一个 serializer:

```
class ProductSerializer(serializers.ModelSerializer):
  class Meta:
    fields = '__all__'
```

```
class ProductListView(generics.ListView):
  queryset = Product.objects.all()
  serializer_class = ProductSerializer
```

如果不使用 django-filter，可以覆盖 ProductListView 的 get_queryset 来实现 filter 功能，比如说使用 name 做 filter：

```
def get_queryset(self):
  queryset = Product.objects.all()

  name = self.request.query_params.get('name', None)
  if name is not None:
    queryset = queryset.filter(name=name)
  return queryset
```

使用 django-filter，可以使代码变得更简洁，而且还可以用配置性的代码实现更强大的搜索功能。首先需要安装：`pip install django-filter`，然后可以在 views 里添加一个 filter：

```
class ProductFilter(django_filters.FilterSet):
  sort = django_filters.OrderingFilter(fields=('created_at',))

  class Meta:
    model = Product
    fields = ['name',]
```

然后在 ProductListView 里面添加两行代码：

```
filter_backends = (DjangoFilterBackend,)
filter_class = ProductFilter
```

添加对应的 urls 之后，按 name 搜索，按 created_at 都可以实现了。这个 ordering filter 的名字 sort 就是 url 里面排序字段的名字。如果原来 list 的 url 是`/products/`，那么按 name 搜索就是`/products/?name=iphone`，按 created_at 降序就是`/products/?sort=-created_at`

django-filter 还可以实现更复杂的功能，比如说按范围等，可以查看[官方文档](https://github.com/carltongibson/django-filter)
