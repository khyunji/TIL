# 200728 Error & Exception Handling


- input 값은 항상 `str` 형식으로 들어온다.

```python
num = input('값을 입력하세요: ')
#이 때 num이 str 형식이기 때문에 num = 20일 때
print(num * 2) #출력 결과는 40이 아닌 2020이 나오게 된다.
```

```python
num = input('값을 입력하세요: ')
print(int(num))
#만약 사용자가 '백'이라고 입력했다면? 사용자에게 에러 메시지를 띄울 순 없다. → 예외처리 하기
```



- 예외처리 `try` & `exception` 

```python
try:
    num = input('값을 입력하세요: ')
    print(int(num))
    
except:
    print('숫자 형태로 입력해주세요.')
```



- 에러 메시지 처리 `as`

```python
try:
    list = []
    print(list[0])
except IndexError as error:
    print(error)
#출력 결과: list index out of range
```



- 복수 예외 처리

```python
#input 값이 문자열일 때와 0일 때 오류 한 번에 처리하기
try:
    num = input('10으로 나눌 값을 입력하세요: ')
    print(10/int(num))
except (ValueError, ZeroDivisionError):
    print('무언가 잘못되었습니다.')
```

```python
#input 값이 문자열일 때와 0일 때 각각 다른 오류 출력하기
try:
    num = input('10으로 나눌 값을 입력하세요: ')
    print(10/int(num))
except ValueError:
    print('글자가 아닌 숫자를 입력해주세요.')
except ZeroDivisionError:
    print('0으로는 나눌 수 없습니다.')
```

```python
#에러는 순차적으로 수행되기 때문에 가장 작은 범주부터 시작해야 한다.
try:
    num = input('10으로 나눌 값을 입력하시오: ')
    print(10/int(num))
except Exception:
    print('에러가 났어요.')
except ValueError:
    print('글자가 아닌 숫자를 입력해주세요.')
except ZeroDivisionError:
    print('0으로는 나눌 수 없습니다.')
#위와 같은 경우, ValueError가 나든 ZeroDivisionError가 나든 동일하게 '에러가 났어요.'라는 메시지가 뜨게 된다.
```



- `else`

> 에러가 발생하지 않는 경우, 즉 try 코드 블럭이 예외를 일으키지 않았을 때 실행돼야 하는 코드에 사용한다. else는 except 코드 다음에 와야 한다. 

```python
try:
    numbers = [1, 2, 3]
    number = numbers[2]
except IndexError:
    print('오류 발생')
else:
    print(number)
```



- `finally`

> 어떤 경우에든 반드시 실행해야 하는 코드에 사용한다. 예외 발생 여부와 관계없이 항상 실행된다. 
> 
```python
try:
    languages = {'korean': 'good'}
    languages['chinese']
except KeyError as err:
    print(f'{err}는 딕셔너리에 없는 키입니다.')
finally:
    print('감사합니다.')
#출력 결과: 'chinese'는 딕셔너리에 없는 키입니다. 감사합니다.
```