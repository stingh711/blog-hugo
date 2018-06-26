+++
title = "Write a CustomPaginator for django rest framework"
date = 2018-06-23
tags = ["python", "django"]
type = "post"
draft = false
+++

Pagination is essential part for a REST api. Django rest framework provides an easy to way to add pagination to current API and a lot of useful built-in implementations. However, in order to work with a specified frontend component, a customized version is needed.

Customizing is easy in DRF, just extends a current one and override it get\_paginated\_response method. For example, the following one uses the page number pagination ,which means you can pass page=n in the URL to get the nth page. If pageSize is specified in request url, it will be used or the default page\_size will be used

```python
class CustomPagination(pagination.PageNumberPagination):
    page_size = 15
    page_size_query_param = 'pageSize'

    def get_paginated_response(self, data):
        return Response({
            'pagination': {
                'total': self.page.paginator.count,
                'pageSize': self.page.paginator.per_page,
                'current': self.page.number,
                },
                'list': data
            })
```

The response format is customized for antd's table component.

There are two ways to tell your view to use it.

-   Specify `pagination_class=CustomPagination` in your views.
-   Configure it in settings. Doc is [here](http://www.django-rest-framework.org/api-guide/pagination/).