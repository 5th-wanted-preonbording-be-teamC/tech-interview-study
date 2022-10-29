<img src="https://www.django-rest-framework.org/img/logo.png" />

# Serializer(직렬화)
> 직렬화(Serializer)란? 쿼리셋, 모델 인스턴스 등의 복잡한 데이터들를 json/xml등의 형태로 데이터를 변화해주는 어뎁터라고 생가가하면 된다. 반대로 json/xaml형태의 데이터를 원데이터 형태로 복원하는 형태를 역직렬화(Deserializer)라고 한다.

## Django Rest Framework Serializer 클래스 종류
1. Serializer
2. ModelSerializer
3. HyperlinkedModelSerializer
4. ListSerializer
5. BaseSerilizer


### 1. Serializer
* 직렬화/역직렬화 되어야하는 모델의 필드들이 정의된다.
* 모델의 모든 필드를 정의할 수도 있고, 일부필드만 정의할 수도 있다.
* create(), update() 메소드를 정의 해줘야한다.
```python
class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()

    def create(self, validated_data):
        return Comment(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        return instance
```

### 2. ModelSerializer
* Serializer 클래스를 더욱 편하게 정의할 수 있도록 리팩토링을 해준것.
* 직렬화/역직렬화  되어야 하는 모델 필드들을 자동으로 정의할 모델의 필드명만 명시해주면 그것들의 타입에 맞게 시리얼라이저의 필드들이 자동으로 정의가 된다.
* create(), update() 메소드를 가장 기본적인 형태로 이미 구현해 놓았다. 따라서 특별한 세부 구현이 필요하지 않은 경우에는 굳이 두 메소드를 정의해줄 필요가 없다.
* 세부구현이 필요한 경우에는 오버라이딩을 한다.
* 시리얼라이저의 필드 정보 확인하는 방법 print(repr(serializer))
```python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ['id', 'account_name', 'users', 'created']
```

### 3. HyperlinkedModelSerializer
* 기본 키가 아닌 하이퍼링크를 사용하여 관계를 나타내는 것을 제외하고는 ModelSerializer 클래스와 유사하다.
* 기본키 대신 URL 필드 사용
* 필드 옵션에 기본 키를 추가하여 명시적으로 기본 키를 포함할 수 있다.
```python
class AccountSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Account
        fields = ['account_url', 'account_name', 'users', 'created']
        extra_kwargs = {
            'url': {'view_name': 'accounts', 'lookup_field': 'account_name'},
            'users': {'lookup_field': 'username'}
        }
```

### 4. ListSerializer
* 한 번에 여러 개체를 직렬화하고 유효성을 검사하기 위한 동작을 제공한다.
```python
class BookListSerializer(serializers.ListSerializer):
    def create(self, validated_data):
        books = [Book(**item) for item in validated_data]
        return Book.objects.bulk_create(books)

class BookSerializer(serializers.Serializer):
    ...
    class Meta:
        list_serializer_class = BookListSerializer
```

### 5. BaseSerializer
* Serializer 클래스와 동일한 기본 API를 구현한다.
* BaseSerialzier 클래스가 탐색 가능한 API에서 HTML 양식을 생성하지 않는다. 이는 그들이 반환하는 데이터에 각필드를 적절한 HTML 입력으로 렌더링할 수 있는 모든 필드 정보가 포함되어 있지 않기 때문이다.
* BaseSerializer 클래스는 특정 직렬화 스타일을 처리하거나 대체 저장소 백엔드와 통합하기 위해 새로운 일반 직렬 변환기 클래스를 구현하려는 경우에도 유용하다.
```python
class HighScoreSerializer(serializers.BaseSerializer):
    def to_representation(self, instance):
        return {
            'score': instance.score,
            'player_name': instance.player_name
        }
```

> 주로 Serialisr, ModelSerializer 클래스를 사용한다.


참고
* https://www.django-rest-framework.org/api-guide/serializers/#serializers
* https://it-eldorado.tistory.com/70