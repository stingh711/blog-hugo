+++
menu = "main"
categories = ["python"]
tags = ["python", "django"]
description = "Use django-filter to add advanced filter for rest api"
date = "2018-05-02T13:13:00+08:00"
title = "Use django-filter to add advanced filter for rest api"
+++

在上一篇文章中，我们介绍了 django-filter 的基本功能。实际上 django-filter 还可以实现更高级的搜索，比如说文本的模糊查询，日期的范围查询。

继续我们的例子：

```
class Product(models.Model):
  name = models.CharField(max_length=100)
  created_at = models.DatetimeField()

class ProductSerializer(serializers.ModelSerializer):
  class Meta:
    fields = '__all__'

class ProductFilter(django_filters.FilterSet):
  sort = django_filters.OrderingFilter(fields=('created_at',))

  class Meta:
    model = Product
    fields = ['name',]

class ProductListView(generics.ListView):
  queryset = Product.objects.all()
  serializer_class = ProductSerializer
  filter_backends = (DjangoFilterBackend,)
  filter_class = ProductFilter
```

首先要实现按 name 的模糊查询。上面 ProductFilter 里面的 fields，如果是 list，默认就是按=来匹配。它还支持 dict 的格式，可以选择更多的匹配方式。按 name 的模糊匹配，只需把 fields 改成如下即可：

```
fields = {
  'name': ['icontains']
}
```

使用 API 查询是原来的 url 是`/products/`, 模糊查询的 url 就变成`/products/?name__icontains=xxx`。

接下来实现按 created_at 按日期范围来搜索。一个方法是直接在 fields 里面配置：

```
fields = {
  'name': ['icontains'],
  'created_at': ['date__gte', 'date__lte']
}
```

然后就可以用下面的 url 来进行日期范围搜索：`/products/?created_at__date__gte=2018-01-01&created_at__date__lte=2018-04-01`

如果你觉得这个 param 名字太长，还可以自定义 filter 字段：

```
class ProductFilter(django_filters.FilterSet):
  sort = django_filters.OrderingFilter(fields=('created_at',))
  min_date = django_filters.DateFilter(name='created_at__date', lookup_expr='gte')
  max_date = django_filters.DateFilter(name='created_at__date', lookup_expr='lte')

  class Meta:
    model = Product
    fields = {
      'name': ['icontains'],
}
```

然后搜索的 url 变成这样：`/products/?min_date=2018-01-01&max_date=2018-04-01`

想一想如果用 java 写需要多少代码吧...
