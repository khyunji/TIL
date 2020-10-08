# 201007 Django TEST

> https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Testing



### Why is `Django TEST` necessary? 

```markdown
- As websites grow they become harder to test manually
- A small change in one area can impact other areas
- More changes will be required to ensure everything keeps working and errors are not introduced as more changes are made
```

:point_right: One way to mitigate these problems is to write `automated tests`, which can easily and reliably be run every time you make a change.

:point_right: Automate `unit testing` of one's website using Django's test framework.



### `TDD` and `BDD`

- TDD: **[Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development)**

  - a [software development process](https://en.wikipedia.org/wiki/Software_development_process) that relies on the repetition of a **very short development cycle**

  - Test-driven development **reverses traditional development and testing**. So, instead of writing your code first and then retroactively fitting a test to validate the piece of code you just wrote, test-driven development dictates that you **write the test first** and then implement code changes **until your code passes the test** you already wrote. The code you produce when you use this testing methodology is cleaner and **less prone to breaking in the long run**. 

    - :capital_abcd:*be prone to: To be prone to something, usually something bad, means to have a tendency to be affected by it or to do it.*  

  - **[5 steps of test-driven development](https://developer.ibm.com/articles/5-steps-of-test-driven-development/)**

    ![](TIL/image_files/TDD_cycle.png)

    ![](TIL/image_files/TDD_steps.png)
    
    ```markdown
    1. Read, understand, and process the feature or bug request.
    2. Translate the requirement by writing a unit test. If you have hot reloading set up, the unit test will run and fail as no code is implemented yet.
    3. Write and implement the code that fulfills the requirement. Run all tests and they should pass, if not repeat this step.
    4. Clean up your code by refactoring.
    5. Rinse, lather and repeat.
    ```



  - BDD: **[Behaviour-Driven Development](https://en.wikipedia.org/wiki/Behavior-driven_development)**

    - an [Agile software development](https://en.wikipedia.org/wiki/Agile_software_development) process that encourages collaboration among developers, QA and non-technical or business participants in a software project. It **encourages teams to use conversation and concrete examples** to formalize a **shared understanding of how the application should behave**.

    - BDD는 함수 단위가 아닌 시나리오를 기반으로 테스트 케이스를 작성한다. 이 시나리오는 개발자가 아닌 사람이 봐도 이해할 수 있을 정도의 수준이다. 하나의 시나리오는 Given, When, Then 구조를 가지는 것을 기본 패턴으로 권장하며 각 절의 의미는 다음과 같다.
    
      ```markdown
      - Feature : 테스트에 대상의 기능/책임을 명시한다.
      - Scenario : 테스트 목적에 대한 상황을 설명한다.
      - Given : 시나리오 진행에 필요한 값을 설정한다.
      - When : 시나리오를 진행하는데 필요한 조건을 명시한다.
      - Then : 시나리오를 완료했을 때 보장해야 하는 결과를 명시한다.
      ```
    - 현업에서는 TDD보다 BDD를 더 많이 사용하는 추세이다.



### Types of testing

There are numerous types, levels, and classifications of tests and testing approaches. The most important automated tests are:

- `Unit tests` (유닛 테스트)

  Verify **functional** behavior of **individual components**, often to **class and function level**.

- `Regression tests` (버그 수정 확인 테스트)

  Tests that **reproduce historic bugs**. Each test is initially run to verify that the **bug has been fixed**, and then re-run to ensure that it has not been reintroduced following later changes to the code.

- `Integration tests` (통합 테스트)

  Verify **how groupings of components work when used together**. Integration tests are aware of the required interactions between components, but not necessarily of the internal operations of each component. They may cover simple groupings of components through to the whole website.



### Process or create an article

```markdown
- 서버를 켠다.
- /articles/create/
- 데이터를 입력한다.
  - 올바른 데이터가 들어왔을 때
  - **올바르지 않은 데이터가 들어왔을 때**
- 저장을 누르고
- DB에 실제로 새로운 article이 저장되었는지 확인한다.
```

** 빈 칸 제출, 폼에 맞지 않는 데이터 제출, csrf 토큰 삭제 등 올바르지 않은 요청을 보내는 경우 존재한다.



```python
class YourTestClass(TestCase):
    def setUp(self):
        # Setup run before every test method.
        pass

    def tearDown(self):
        # Clean up run after every test method.
        pass

    def test_something_that_will_pass(self):
        self.assertFalse(False)

    def test_something_that_will_fail(self):
        self.assertTrue(False)
```



### Test structure overview

무엇을 테스트 할 지 자세히 보기 전에, 간단히 어디서 그리고 어떻게 테스트가 정의되는지 대략 살펴 봅시다.

장고는 유닛테스트의 모듈인 [built-in test discovery](https://docs.python.org/3/library/unittest.html#unittest-test-discovery)을 사용하는데, 이는 현재 작업중인 디렉토리의 **test\*.py**라는 패턴을 가진 모든 파일들을 체크합니다. 그 파일들의 이름을 적당하게 붙이는 한, 당신은 당신이 원하는 어떤 구조라도 이용할 수 있습니다. 우리는 당신의 테스트코드를 위한 한 모듈을 만들 것을 추천합니다. 그리고 모델들, 뷰들, 폼들 그리고 테스트가 필요한 어떤 다른 타입의 코드라도 각각을 분리하기를 바랍니다. 예를 들면:

```
catalog/
  /tests/
    __init__.py
    test_models.py
    test_forms.py
    test_views.py
```



## 실습 과정

- Assertions[¶](https://docs.djangoproject.com/en/3.1/topics/testing/tools/#assertions)





---

07 OCT 2020: Create Django TEST TIL markdown file (written in korean)
                        Contribute to localize mdn Django Testing