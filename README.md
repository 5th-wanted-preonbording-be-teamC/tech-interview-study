# tech-interview-study

면접 스터디

## select_related()

select_related()는 foreign-key 관계를 QuerySet으로 반환합니다.<br />
이는 단일 쿼리를 더욱 복잡하게 만들지만 나중에 외래키 관계를 사용할 경우 성능을 향상시킬 수 있습니다.<br />
select_related()는 one-to-one 관계와 단일 값 관계의 foreign key로 제한됩니다.

```py
# 데이터베이스 참조가 발생한다.
e = Entry.objects.get(id=5)

# 다시한번 데이터베이스 참조가 발생한다.
b = e.blog
```

select_related를 사용한다면:

```py
# 데이터베이스 참조가 발생한다.
e = Entry.objects.selected_related('blog').get(id=5)

# 데이터베이스 참조가 발생하지 않는다.
b = e.blog
```

참고링크:

- https://docs.djangoproject.com/en/4.1/ref/models/querysets/#select-related
- https://betterprogramming.pub/django-select-related-and-prefetch-related-f23043fd635d

## prefetch_related()

prefetch_related()는 select_related()와 비슷한 목적을 가지고 있습니다.<br />
prefetch_related()는 many-to-many, many-to-one의 관계의 개체를 미리 가져올 수 있습니다.<br />

예시 모델:

```py
from django.db import models

class Topping(models.Model):
    name = models.CharField(max_length=30)

class Pizza(models.Model):
    name = models.CharField(max_length=50)
    toppings = models.ManyToManyField(Topping)

    def __str__(self):
        return "%s (%s)" % (
            self.name,
            ", ".join(topping.name for topping in self.toppings.all()),
        )
```

실행:

```py
Pizza.objects.all()
```

`__str__()`는 `self.toppings.all()`로 인해 매번 `Toppings` 테이블에 대한 쿼리를 실행합니다.

```
Pizza.objects.prefetch_related('toppings')
```

`prefetch_related`로 호출하게 된다면 prefetch에 필요한 모든 IDs를 수집하고, SQL의 `WHERE IN(IDs)`의 쿼리를 실행합니다. <br />
self.toppings.all()`이 호출될 때마다 데이터베이스로 호출하는 대신 미리 fetch된 QuerySet cache에서 항목을 찾습니다.

참고링크:

- https://docs.djangoproject.com/en/4.1/ref/models/querysets/#prefetch-related
- https://betterprogramming.pub/django-select-related-and-prefetch-related-f23043fd635d
