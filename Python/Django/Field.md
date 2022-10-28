# [Field](https://docs.djangoproject.com/en/4.1/ref/models/fields/)

## Field 란?
> **Field** is an abstract class that represents a database table column. Django uses fields to create the database table ([**db_type()**](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.Field.db_type)), to map Python types to database ([**get_prep_value()**](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.Field.get_prep_value)) and vice-versa ([**from_db_value()**](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.Field.from_db_value)).
>
>> **Field** 란 데이터베이스 테이블의 열을 표현한 추상적 클래스이다. Django는 field를 이용하여 데이터베이스 테이블을 생성하고([**db_type()**](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.Field.db_type)), Python 타입을 데이터베이스에 매핑하고([**get_prep_value()**](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.Field.get_prep_value)), 역으로도 매핑한다([**from_db_value()**](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.Field.from_db_value)).

## Field의 종류

주로 사용하는 Field는 Bold 처리.

### 데이터 타입에 따른 Field

- 수
  - 0을 포함한 자연수
    - PositiveBigIntegerField

      0 이상의 64bit 정수 지원.(0 ~ 9223372036854775807)

    - **PositiveIntegerField**

      0 이상의 32bit 정수 지원.(0 ~ 2147483647)

    - PositiveSmallIntegerField

      0 이상의 16bit 정수 지원.(0 ~ 32767)

  - 정수
    - BigIntegerField

      IntegerField 64bit 정수까지 지원. (-9223372036854775808 ~ 9223372036854775807)

    - IntegerField

      32bit 정수 지원.(-2147483648 ~ 2147483647)

    - SmallIntegerField

      16bit 정수 지원. (-32768 ~ 32767)

  - 실수
    - DecimalField

      [Decimal](https://docs.python.org/ko/3/library/decimal.html#decimal.Decimal) 지원.

    - FloatField

      부동 소수점 값 지원.

  - 자동 생성
    - AutoField
      
      ID 등을 위해 자동으로 증가하는 Field. 32bit 정수 지원.(1 ~ 2147483647)

    - BigAutoField

      AutoField와 동일하나 64bit 정수 지원. (1 ~ 9223372036854775807)
      
    - SmallAutoField

      AutoField와 동일하나 16bit 정수 지원. (1 ~ 32767)

- 문자열
  - 기본
    - **CharField**

      문자열 지원. 긴 문자열을 지원해야한다면 TextField 권장.

    - **TextField**

      긴 문자열 지원.

  - 형식이 있는 문자열
    - EmailField

      [EmailValidator](https://docs.djangoproject.com/ko/4.1/ref/validators/#django.core.validators.EmailValidator)로 검증하는 이메일 형식 문자열 지원

    - FilePathField

      파일 경로를 포함한 파일 이름 형식의 문자열 지원.

      - FilePathField.path

        기본 절대 경로 문자열 혹은 Callable 객체. 필수 인자.

    - GenericIPAddressField

      IPv4 혹은 IPv6 형식의 문자열 지원.

    - SlugField

      문자, 숫자, `_`, `-`만으로 이루어진 문자열. URL의 마지막을 주로 사용

    - URLField

      [](https://docs.djangoproject.com/ko/4.1/ref/validators/#django.core.validators.URLValidator)로 검증된 URL 형식의 문자열 지원.

    - UUIDField

      [UUID](https://docs.python.org/3/library/uuid.html#uuid.UUID) 혹은 UUID 형식의 32자 문자열 지원.

- 시간
  - DateField

    날짜([datetime.date](https://docs.python.org/ko/3/library/datetime.html#date-objects)) 지원.

  - **DateTimeField**

    날짜와 시간([datetime.date](https://docs.python.org/ko/3/library/datetime.html#datetime-objects)) 지원.

    - DateField.auto_now

      오브젝트가 저장될 때 자동 업데이트. 수정된 시각을 저장할 때 유용.

    - DateField.auto_now_add

      오브젝트가 생성될 때 자동 생성. 생성된 시각을 저장할 때 유용.

  - DurationField

    기간([datetime.timedelta](https://docs.python.org/ko/3/library/datetime.html#datetime.timedelta)) 지원

  - TimeField

    시각([datetime.time](https://docs.python.org/ko/3/library/datetime.html#time-objects)) 지원.

- 파일
  - FileField

    파일 지원.

    - FileField.upload_to

      파일 저장경로 인자. 시각 포맷팅([strftime()](https://docs.python.org/3/library/time.html#time.strftime))을 지원.

  - ImageField

    이미지 형식의 파일 지원.

- 기타
  - BinaryField

    바이너리 데이터([bytes](https://docs.python.org/ko/3/library/stdtypes.html#bytes), [bytearray](https://docs.python.org/ko/3/library/stdtypes.html#bytearray), [memoryview](https://docs.python.org/ko/3/library/stdtypes.html#memoryview)) 지원.

  - **BooleanField**
    
    True/False 지원.

  - JSONField

    JSON 데이터 지원.
