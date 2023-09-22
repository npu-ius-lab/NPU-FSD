# Python代码开发编程规范
[toc]

> 🎉 什么样的代码是好代码？如何评审别人的代码？


一般来说，可以从如下 3 个方面评判一份代码是否是好代码。

1. **可读性（编程规范性，便于后期维护和理解）**

代码的生命周期中，大部分时间都是被人阅读的，好的代码需要做到可以让别人快速无障碍地看懂你的代码，不需要太多的解释和注释， **做到代码即文档** 。

所以在写代码的时候，不仅要考虑写的感受， **更要多考虑读代码人的感受** 。

**可阅读性可以提高代码的可维护性** 。

2. **可扩展性（当需要增加新需求时，非常方便）**

为了满足不同的需求，代码可以被更新或修改，并且同时不会破坏原有的功能。可扩展性可以帮助我们在不断变化的需求中保持代码的可用性。

3. **健壮性（增加新功能后，对原功能没有影响）**

代码可以在不同的情况下正常工作，并且可以处理异常情况（允许抛出异常，但不允许不明不白的崩溃）。健壮性好的代码可以帮助防止程序崩溃和数据丢失。

**高内聚-低耦合**：在软件设计中通常用**耦合度**和**内聚度**作为衡量模块独立程度的标准。
从模块粒度来看：**高内聚：尽可能类的每个成员方法只完成一件事（最大限度的聚合）；** **低耦合：减少类内部，一个成员方法调用另一个成员方法。**
从类角度来看-高内聚低耦合：**减少类内部，对其他类的调用；**
从功能块来看-高内聚低耦合：**减少模块之间的交互复杂度（接口数量，参数数据）即横向：类与类之间、模块与模块之间；纵向：层次之间；尽可能，内容内聚，数据耦合**。

**编程规范正是从以上这些方面，来促进我们写出更高质量的代码**

***

依据约束性的强弱，此约定将约束依次分为【强制】、【推荐】、【参考】三类。在每条约定的解释中，说明对该条约定做了适当的扩展和解释，正例给出了提倡使用的方式，反例给出了需要避免使用的错误方式。
python社区开源贡献者基本都遵守[PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)编码规范,推荐观看一下,有中文版.
对于此能够使用如 [Pylint](https://pylint.readthedocs.io/en/latest/) 等工具进行自动格式化的（如赋值符号=前后空格，本规范不再提及)。
比如用pycharm来编写Python代码时，如果有出现不符合PEP8规范的话，pycharm就会提示我，如图:
![alt](C:\Users\TendernessMook\Desktop\MD文档\编程规范图片\pep8.png)


## 一、编码约定

1. 【强制】代码的可读性非常重要。代码的生命周期中，绝大部份时间处于被人阅读的状态。因此，当有多个考量相互冲突时，请 **优先** 保证可读性。

2. 【强制】每行最多不超过 120 个字符。
   * 说明：过长的行会导致阅读障碍，使得缩进失效。
   * 确定单行字符个数时，应该考虑横屏，竖屏，git 对比 diff 等使用场景。
   * 【反例】-MetroAeb：
      ```python
      idx = np.where((points_raw[:,3] >= self.min_intensity) & (points_raw[:,3] <=self.max_intensity ) & (points_raw[:,2] < self.height ) &(points_raw[:,0] > self.left )& (points_raw[:,0] < self.right ) &  (points_raw[:,1] >self.min_y ) &  (points_raw[:,1] < self.max_y ))
      ```
   * 【正例】-MetroAeb:
```python
      # 定义条件
      is_intensity_within_range = (points_raw[:, 3] >= self.min_intensity) & (points_raw[:, 3] <= self.max_intensity)
      is_height_below_limit = points_raw[:, 2] < self.height
      is_x_within_bounds = (points_raw[:, 0] > self.left) & (points_raw[:, 0] < self.right)
      is_y_within_bounds = (points_raw[:, 1] > self.min_y) & (points_raw[:, 1] < self.max_y)

    # 合并所有条件
    idx = np.where(
        is_intensity_within_range &
        is_height_below_limit &
        is_x_within_bounds &
        is_y_within_bounds)
```


3. 【强制】模块中一级函数和类定义之间空 2 行，类中函数定义之间空 1 行，模块尾部有且仅有 1 个空行。
   * 【反例】：
```python
    # 一级函数之间没有两个空行
    def function_1():
        pass
    def function_2():
        pass

    class MyClass:
        # 类定义之后没有两个空行
        def __init__(self):
            pass
        # 类中的函数定义之间没有一个空行
        def method_1(self):
            pass
        def method_2(self):
            pass
        
	# 模块尾部没有空行
```

   * 【反例】：
```python
    # 一级函数之间有两个空行
    def function_1():
        pass


    def function_2():
        pass


    class MyClass:
        # 类定义之后有两个空行

        def __init__(self):
            pass

        # 类中的函数定义之间有一个空行
        def method_1(self):
            pass

        def method_2(self):
            pass


    # 模块尾部有且仅有一个空行

```

4. 【强制】在判断条件中应使用 is not，而不使用 not ... is。
   * [正例]: if foo is not None:
   * [反例]: if not foo is None:

5. 【强制】函数参数中，禁止使用可变类型变量（mutable）作为默认值。
   * 例子

    ```python
    # 不推荐的写法
    def add_item(item, my_list=[]):
        my_list.append(item)
        return my_list
   
    result1 = add_item(1)  # 结果是 [1]
    result2 = add_item(2)  # 结果是 [1, 2]
   
    # 推荐的写法
    def add_item(item, my_list=None):
        if my_list is None:
            my_list = []
        my_list.append(item)
        return my_list
   
    result1 = add_item(1)  # 结果是 [1]
    result2 = add_item(2)  # 结果是 [2]
   
    ```
   在上面的示例中，不推荐的写法会导致意外的行为，因为默认列表在**多次调用中被共享**。而推荐的写法避免了这个问题，每次都会创建一个新的空列表作为默认值，从而更加可预测和安全。因此，为了避免潜在的问题和错误，最好遵循这一规则，不要在函数参数中使用可变类型变量作为默认值。
6. 【强制】禁止定义了变量却不使用。
这样会造成代码冗余，可读性变差
7. 【强制】禁止单个函数超过 35 行(**重要！**)。
   * 说明：人脑的阅读记忆能力有限。研究表明，人的短期记忆只能同时记住不超过 10 名字。因此，当某个函数过长（一般来说，超过一屏的函数被认为过长），应该及时把它拆分为多个小函数。
   * 【反例】
```python
def process_data(data):
    result = []
    for item in data:
        if item['status'] == 'active':
            if item['value'] > 100:
                processed_item = {}
                processed_item['name'] = item['name']
                processed_item['value'] = item['value']
                processed_item['category'] = item['category']
                if 'description' in item:
                    processed_item['description'] = item['description']
                result.append(processed_item)
    return result

```
在这个反例中，process_data 函数过长，难以一目了然地理解其功能。它可以被拆分成多个小函数，以提高可读性和维护性。
根据编码规范，应该尽量避免编写过长的函数，因为过长的函数通常难以理解和维护。拆分函数可以使代码更加模块化，易于测试和维护，有助于提高代码的质量。

   * 【正例】
```python
def calculate_average(numbers):
    if not numbers:
        return 0
    
    total = sum(numbers)
    count = len(numbers)
    
    if count == 0:
        return 0
    
    average = total / count
    return average
```

8. 【强制】禁止超过 2 个 for 语句或过滤器表达式，否则使用传统 for 循环语句替代。
    * 说明：当两个 for 语句的列表解析式比较复杂时候，也应该使用传统 for 循环语句替代。
    * 正例

```python
    number_list = [1, 2, 3, 10, 20, 55]
    odd = [i for i in number_list if i % 2 == 1]

    result = []
    for x in range(10):
        for y in range(5):
            if x * y > 10:
                result.append((x, y))
```

* 反例

```python
result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]  
```

9. 【强制】列表推导式适用于简单场景。 如果语句过长，则不应该使用列表解析式。

    * 正例

    ```python
    fizzbuzz = []
    for n in range(100):
        if n % 3 == 0 and n % 5 == 0:
            fizzbuzz.append(f'fizzbuzz {n}')
        elif n % 3 == 0:
            fizzbuzz.append(f'fizz {n}')
        elif n % 5 == 0:
            fizzbuzz.append(f'buzz {n}')
        else:
            fizzbuzz.append(n)
    ```

    * 反例

    ```python
    # 条件过于复杂，应该采用for语句展开
    fizzbuzz = [
        f'fizzbuzz {n}' if n % 3 == 0 and n % 5 == 0
        else f'fizz {n}' if n % 3 == 0
        else f'buzz {n}' if n % 5 == 0
        else n
        for n in range(100)
    ]
    ```

10. 【强制】尽量使用 `from x imoprt *`。

**代码可读性提高**： 明确地指定导入的模块或变量可以让代码更易于阅读和理解。通配符导入可能使代码不清晰，因为读者无法轻松确定从哪个模块导入了哪些名称。

**避免命名冲突**： 通配符导入可能导致变量名冲突，特别是当多个模块都具有相同名称的变量或函数时。这会导致不可预测的行为和错误，因为你无法确定哪个定义将优先使用。

**代码维护更容易**： 使用通配符导入可能使代码更难维护，因为你无法轻松识别依赖关系。当需要更新、修改或删除导入的模块或变量时，会更容易出错。

**更好的命名空间管理**： 明确导入只引入你实际需要的内容，这有助于管理命名空间，减少可能的冲突，提高代码的可维护性。

**代码可移植性增强**： 当你的代码需要在不同的项目或环境中使用时，明确导入可以帮助确保代码在各种情况下都能正常工作，因为你知道你的依赖关系清晰可见。

***
>
11. 【推荐】变量名要有描述性，不能太宽泛，好的变量名可以极大提高代码的整体可读性。

    * 说明：在 **可接受的长度范围内** ，变量名能把它所指向的内容描述的越精确越好。因此，尽量不要用过于宽泛的词来作为变量名。

12. 【推荐】变量名最好让人能猜出类型。
    * 布尔类型变量的最大特点是：它只存在两个可能的值「是」或「不是」，因此推荐使用 is、has 等非黑即白的词修饰的变量名。

    * 正例：is_superuser，has_error，allow_vip，use_msgpack，debug 【约定俗成】

    * 阅读者看到和数字相关的名词时，都会默认想到它们是 int 、float 类型。

    * 正例：释义为数字的所有单词：port（端口号）、age（年龄）、radius（半径）

    * 正例：使用 length、count 开头或者结尾的单词： length_of_username、max_length 。不要使用普通的复数来表示一个 int 类型变量，比如 apples、trips，最好用 number_of_apples、trips_count 来替代。
    * 【综合例子】：
```python
    # 正例

    # 描述性变量名提高了代码可读性
    total_score = 0
    user_count = 10
    product_name = "Widget"
    customer_address = "123 Main St"

    # 用于表示状态的变量名
    is_active = True
    has_permission = False

    # 使用描述性变量名提高代码理解
    for item in shopping_cart:
        if item['quantity'] > 0:
            calculate_total_price(item)

    # 反例

    # 变量名过于宽泛，不清楚其含义
    x = 10
    temp = "ABC"
    data = getData()
    result = process()

    # 缩写或非具体的变量名
    fnc = get_user_data()
    val = calculate()
    info = data

    # 变量名不清晰，不容易理解其用途
    for i in lst:
        if i > 0:
            foo(i)

```
>
13. 【推荐】变量名不能过长，也不能过短，在 1～5 个单词之内较为合适。

    * 说明：绝大多数情况下，都应该 **避免使用** 只有一两个字母的短名字。如数组索引三剑客 i、j、k，用有明确含义的名字，比如 person_index 来代替它们总是会更好一些。
>
14. 【推荐】不要使用带否定含义的变量名。

    * 正例：is_special

    * 反例：is_not_normal
>
15. 【推荐】变量使用应该保持一致性。

    * 说明：如果你在一个方法内里面把图片变量叫做 photo，在其他的地方就不要把它改成 image，这样只会让代码的阅读者困惑：image 和 photo 到底是不是同一个东西？
>
16. 【推荐】变量定义尽量靠近使用。

    * 说明：很多人在学习编程时，会有一个习惯。就是把所有的变量定义写在一起，放在函数或方法的最前面。这样做只会让你的代码「看上去很整洁」，但是对提高代码可读性没有任何帮助。

    * 更好的做法是， **让变量定义尽量靠近使用** 。那样当你阅读代码时，可以更好的理解代码的逻辑，而不是费劲的去想这个变量到底是什么、哪里定义的。
>
17. 【推荐】合理定义临时变量，提升可读性。

    * 说明：代码里的复杂表达式可以通过**增加临时变量**的方式明确含义。

    * 反例：

    ```python
    # 为所有性别为女性，或者级别大于 3 的活跃用户发放 10000 个金币
    if user.is_active and (user.sex == 'female' or user.level > 3):
        user.add_coins(10000)
        return
    ```

    * 正例：

    ```python
    # 为所有性别为女性，或者级别大于 3 的活跃用户发放 10000 个金币
    user_is_eligible = user.is_active and (user.sex == 'female' or user.level > 3):
    
    if user_is_eligible:
        user.add_coins(10000)
        return
    ```

    * 反例：

    ```python
    def get_best_trip_by_user_id(user_id):
        # 心理活动：「嗯，这个值未来说不定会修改/二次使用」，先把它定义成变量.
        user = get_user(user_id)
        trip = get_best_trip(user_id)
        result = {
            'user': user,
            'trip': trip
        }
        return result
    ```

    * 正例：

    其实，你所想的「未来」永远不会来，这段代码里的三个临时变量完全可以去掉，变成这样：

    ```python
    def get_best_trip_by_user_id(user_id):
        return {
            'user': get_user(user_id),
            'trip': get_best_trip(user_id)
        }
    ```
>
18. 【推荐】如非必要，不要用 `globals()`、`locals()`。

    * 说明：也许你第一次发现 `globals()`、`locals()`这对内建函数时很兴奋，迫不及待的写下下面这种极端「简洁」的代码：

    * 反例：

    ```python
    def render_trip_page(request, user_id, trip_id):
        user = User.objects.get(id=user_id)
        trip = get_object_or_404(Trip, pk=trip_id)
        is_suggested = judg_is_suggested(user, trip)
        # 利用 locals() 节约了三行代码，我是个天才！
        return render(request, 'trip.html', locals())
    ```

    * 千万不要这么做，这样只会让读到这段代码的人「包括三个月后的你自己」痛恨你。

    * 因为他需要记住这个函数内定义的所有变量，更别提 locals()还会把一些不必要的变量传递出去。

    * 重要的编码原则： **Explicit is better than implicit.** **（显式优于隐式）**。因此，推荐下面的方式：

    * 正例：

    ```python
    return render(request, 'trip.html', {
            'user': user,
            'trip': trip,
            'is_suggested': is_suggested
        })
    ```
>
19. 【推荐】避免多层分支嵌套。

    * 说明：要竭尽所能的避免分支嵌套， **提前结束** 可以**优化**很多情况下的分支嵌套问题。

    * 反例

    ```python
    def buy_fruit(nerd, store):
        """去水果店买苹果
        
        - 先得看看店是不是在营业
        - 如果有苹果的话，就买 1 个
        - 如果钱不够，就回家取钱再来
        """
        if store.is_open():
            if store.has_stocks("apple"):
                if nerd.can_afford(store.price("apple", amount=1)):
                    nerd.buy(store, "apple", amount=1)
                    return
                else:
                    nerd.go_home_and_get_money()
                    return buy_fruit(nerd, store)
            else:
                raise MadAtNoFruit("no apple in store!")
        else:
            raise MadAtNoFruit("store is closed!")
    ```

    * 正例

    ```python
    def buy_fruit(nerd, store):
        if not store.is_open():
            raise MadAtNoFruit("store is closed!")
    
        if not store.has_stocks("apple"):
            raise MadAtNoFruit("no apple in store!")
    
        if nerd.can_afford(store.price("apple", amount=1)):
            nerd.buy(store, "apple", amount=1)
            return
        else:
            nerd.go_home_and_get_money()
            return buy_fruit(nerd, store)
    ```
>
20. 【推荐】封装过于复杂的逻辑判断。

    *  说明：如果条件分支里的表达式过于复杂，出现了太多 not、and、or，那么此代码的可读性就会大大降低。

    *  反例
    ```python
    email = "example@email.com"
    
    if "@" in email and "." in email and email.count("@") == 1 and email.count(".") == 1 and email.index("@") < email.index("."):
        print("Valid email")
    else:
        print("Invalid email")
    
    ```
    *  正例
    ```python    
    def is_valid_email(email):
        if "@" not in email or "." not in email:
            return False
        if email.count("@") > 1 or email.count(".") > 1:
            return False
        if email.index("@") > email.index("."):
            return False
        return True
    
    # 使用简化的逻辑判断
    if is_valid_email("example@email.com"):
        print("Valid email")
    else:
        print("Invalid email")
    ```
>
21. 【推荐】总是注意不同分支下的重复代码

    *  说明：重复代码是代码质量的天敌，而条件分支语句又非常容易成为重复代码的重灾区。因此，当我们编写条件分支语句时，需要特别留意， **不要生产** 不必要的重复代码。

    *  反例

    ```python
    # 对于新用户，创建新的用户资料，否则更新旧资料
    if user.no_profile_exists:
        create_user_profile(
            username=user.username,
            email=user.email,
            age=user.age,
            address=user.address,
            # 对于新建用户，将用户的积分置为 0
            points=0,
            created=now(),
        )
    else:
        update_user_profile(
            username=user.username,
            email=user.email,
            age=user.age,
            address=user.address,
            updated=now(),
        )
    ```

    *  正例：

     ```python
    if user.no_profile_exists:
        profile_func = create_user_profile
        extra_args = {'points': 0, 'created': now()}
    else:
        profile_func = update_user_profile
        extra_args = {'updated': now()}
    
    profile_func(
        username=user.username,
        email=user.email,
        age=user.age,
        address=user.address,
        **extra_args
    )
     ```
这样修改后，如果想修改代码，会避免在多个地方产生同样的修改，增加修改工作量。
>
22. 【推荐】多个条件判断： or条件表达式应该将值为真可能性较高的写在前面，and则应该写在后面。

    ```python
    abbreviations = ["cf.", "e.g.", "ex.", "etc.", "flg."]
    
    for w in ("Mr.", "Hat", "is", "chasing", "."):
        if w in abbreviations and w[-1]=='.': # 这句性能较差，会进入循环遍历
        # if w[-1] == '.' and w in abbreviations: # 性能好，某些情况直接剪枝
            pass
    ```
>
23. 【推荐】使用德摩根定律： not A or not B 等价于 not (A and B)，推荐使用前者。

    * 说明：人类思维不擅长处理过多的【否定】以及【或】这种逻辑关系。

    *  正例

    ```python
    # 如果用户没有登录或者用户没有使用 chrome，拒绝提供服务
    if not user.has_logged_in or not user.is_from_chrome:
        return "our service is only available for chrome logged in user
    ```

    *  反例

    ```python
    if not (user.has_logged_in and user.is_from_chrome):
        return "our service is only available for chrome logged in user"
    ```
>
24. 【推荐】Ask forgiveness not permission（先尝试，然后请求原谅）是Python编程中的一种设计原则，鼓励开发者在代码中使用异常处理（try/except）来处理潜在的异常情况，而不是提前进行条件检查，以增加代码的可读性和简洁性。这种方式适用于那些异常情况很少发生的情况。

    *  说明：如果代码执行只有极少数情况会产生异常，此时建议使用 try/exception而不是提前做条件判断。
>    *  反例
>
>    ```python
>    def read_file(file_path):
>        if os.path.isfile(file_path):
>            with open(file_path, 'r') as file:
>                content = file.read()
>            return content
>        else:
>            return None
>    
>    file_content = read_file('example.txt')
>    if file_content is not None:
>        print("File content:", file_content)
>    else:
>        print("File not found or could not be read.")
>    
>    ```
在这个反例中，我们提前使用 os.path.isfile() 来检查文件是否存在，然后再尝试打开和读取文件。这样的代码结构较为繁琐，并且在多个地方需要进行条件检查，降低了代码的可读性。
>    * 正例
>
>    ```python
>    def read_file(file_path):
>        try:
>            with open(file_path, 'r') as file:
>                content = file.read()
>            return content
>        except FileNotFoundError:
>            return None
>    
>    file_content = read_file('example.txt')
>    if file_content is not None:
>        print("File content:", file_content)
>    else:
>        print("File not found or could not be read.")
>    
>    ```
在这个正例中，我们尝试打开文件并读取其内容。如果文件不存在，会抛出 FileNotFoundError 异常，然后我们使用异常处理来捕获该异常并返回 None，而不是提前检查文件是否存在。这种方式使代码更加简洁，并且在异常情况下能够提供合适的处理。
>
>    
25. 【推荐】在判断条件中使用 **all**和 **any**内置函数，可以使代码简洁，高效。

**all**函数接受一个可迭代对象（如列表或元组），并检查其中的所有元素是否都为 True。如果所有元素都为 True，则 all 函数返回 True，否则返回 False。

```python
# 示例：检查列表中的所有元素是否大于零
numbers = [1, 2, 3, 4, 5]
result = all(num > 0 for num in numbers)
print(result)  # 输出 True

# 示例：检查列表中的所有元素是否为偶数
numbers = [2, 4, 6, 8, 9]
result = all(num % 2 == 0 for num in numbers)
print(result)  # 输出 False
```
**any** 函数接受一个可迭代对象，并检查其中的任何元素是否为 True。如果至少有一个元素为 True，则 any 函数返回 True，否则返回 False。

```python
# 示例：检查列表中是否存在奇数
numbers = [2, 4, 6, 8, 9]
result = any(num % 2 != 0 for num in numbers)
print(result)  # 输出 True

# 示例：检查列表中是否存在负数
numbers = [1, 2, 3, -4, 5]
result = any(num < 0 for num in numbers)
print(result)  # 输出 True

```
26. 【推荐】对序列（字符串、列表 、元组），空序列为 False的情况判断，**不要使用len函数**。
    * 正例

     ```python
    if not seq:
       pass
    
    if seq:
       pass
     ```

    * 反例

     ```python
    if len(seq):
       pass
    
    if not len(seq):
       pass
     ```

27. 【推荐】使用 def定义简短函数而不是使用 Lambda匿名函数。
    * 说明：使用 def定义函数有助于在 trackbacks中打印有效的类型信息。
    * Python 曾经想要移除Lambda，后来妥协了。
>以下是一个使用 `def` 定义函数和使用 `lambda` 匿名函数的比较示例：
```python
>def add(x, y):
    """This function adds two numbers."""
    return x + y

result = add(2, 3)
print(result)  # 输出 5
```
```python
add = lambda x, y: x + y

result = add(2, 3)
print(result)  # 输出 5
```
在这个示例中，使用 lambda 创建了一个匿名函数 add，但是没有提供名称和文档字符串，因此它的用途不太明确。如果在复杂的应用程序中使用大量的 lambda 匿名函数，会**降低**代码的**可读性**。
28. 【推荐】经常改动的列表定义、字典定义、函数参数，建议每行一个元素，并且每行增加一个（方便版本管理）。
    
    GitHub是一个分布式版本控制平台，它允许团队协作开发代码。"diff"功能用于比较不同版本之间的代码差异，帮助开发人员了解每个提交的内容。
    
    * 正例
    
     ```python
    yes = ('y', 'Y', 'yes', 'TRUE', 'True', 'true', 'On', 'on', '1')  # 基本不再改变
    
    kwlist = [
        'False',
        ...
        'yield',  # 最后一个元素也增加一个逗号 ，方便以后diff不显示此行
    ]
    
    person = {
        'name': 'bob',
        'age': 12,      # 可能经常增加字段
        }
     ```
    
    * 反例
    
     ```python
    kwlist = ['False', 'None', 'True', 'and', 'as',
              'assert', 'async', ...
    ]
    
    person = {'name': 'bob', 'age': 12} # 经常增加字段，不利于 git 做 diff 比较
     ```
>
29. 【推荐】过长的计算表达式，操作符应该在换行符之后出现。
    * 正例

     ```python
    # YES: 易于将运算符与操作数匹配，可读性高
    income = (gross_wages
              + taxable_interest
              + (dividends - qualified_dividends)
              - ira_deduction
              - student_loan_interest)
     ```

    * 反例

     ```python
    # No: 运算符的位置远离其操作数
    income = (gross_wages +
              taxable_interest +
              (dividends - qualified_dividends) -
              ira_deduction -
              student_loan_interest)
     ```
>
30. 【推荐】公共函数、类、模块需要包含文档字符串。内部使用的函数，在函数名不能让使用者一眼就明白作用的情况下，需要提供注释。
    * 说明：一个函数如果被设计为可以直接被其他开发者使用，那么请提供详细文档明确其含义，明确指出输入、输出、以及异常内容。
>   * 例子：
```python
def add_numbers(a, b):
    """
    这个函数用于将两个数字相加。

    Args:
        a (int): 第一个整数。
        b (int): 第二个整数。

    Returns:
        int: 返回两个整数的和。
    """
    return a + b

class Rectangle:
    """
    这是一个矩形类，用于表示矩形的属性和行为。
    """
    def __init__(self, length, width):
        """
        创建一个矩形对象。

        Args:
            length (float): 矩形的长度。
            width (float): 矩形的宽度。
        """
        self.length = length
        self.width = width

    def area(self):
        """
        计算矩形的面积。

        Returns:
            float: 矩形的面积。
        """
        return self.length * self.width

    def perimeter(self):
        """
        计算矩形的周长。

        Returns:
            float: 矩形的周长。
        """
        return 2 * (self.length + self.width)

    
def complex_operation(x, y):
    # 这是一个内部函数，执行一些复杂的操作。
    # 由于操作复杂，提供注释来解释其作用。
    result = x * y + (x - y) / 2
    return result


# 使用函数和类，并查看文档字符串
sum_result = add_numbers(5, 3)
print("Sum:", sum_result)

rectangle = Rectangle(4, 6)
print("Area:", rectangle.area())
print("Perimeter:", rectangle.perimeter())

# 调用内部函数，提供注释说明
result = complex_operation(10, 7)
print("Result:", result)
```
31. 【推荐】文档字符串（docstring）必须使用三重双引号 """ 。
>
32. 【推荐】在使用文档字符串时，推荐使用 reStructuredText 风格类型。

    * 前三引号后不应该换行，应该紧接着在后面概括性的说明模块、函数、类、方法的作用，然后再空一行进行详细的说明。后三引号应该单独占一行。

    * 其中函数的 docstring 推荐大致**顺序**是：**概述、详细描述、参数、返回值、异常**。

    * 一般不要求描述实现细节，除非其中涉及非常复杂的算法。

    * 示例

    ```python
    def fetch_bigtable_rows(big_table: Table, keys: list[str], other_silly_variable: bool=None)->dict[str,Any]:
            """Fetches rows from a Bigtable.
    
            Retrieves rows pertaining to the given keys from the Table instance
            represented by big_table.  Silly things may happen if
            other_silly_variable is not None.
    
            :param big_table: An open Bigtable Table instance.
            :param keys: A sequence of strings representing the key of each table row
                to fetch.
            :param other_silly_variable: Another optional variable, that has a much
                longer name than the other args, and which does nothing.
    
            :return: A dict mapping keys to the corresponding table row data
                fetched. Each row is represented as a tuple of strings. For
                example:
    
                {'Serak': ('Rigel VII', 'Preparer'),
                'Zim': ('Irk', 'Invader'),
                'Lrrr': ('Omicron Persei 8', 'Emperor')}
    
                If a key from the keys argument is missing from the dictionary,
                then that row was not found in the table.
    
            :raises ValueError: if `keys` is empty.
            :raises IOError: An error occurred accessing the bigtable.Table object.
            """
            pass
    ```
>

33. 【推荐】所有 try/except  子句的代码要尽可的少，以免屏蔽其他的错误。

    ```python
    try:
        value = collection[key]
    except KeyError:
        return key_not_found(key)
    else:
        return handle_value(value)
    #反例
    try:
        # 范围太广
        return handle_value(collection[key])
    except KeyError:
        # 会捕捉到 handle_value() 中的 KeyError
        return key_not_found(key)
    ```

34. 【推荐】函数的返回类型要单一，不能返回不同的类型【允许返回 None】。

    * 正例

    ```python
    def add(a, b):
        if isinstance(a, int) and isinstance(b, int):
            return a + b
        else:
            raise Exception("Only int are allowed")
    ```

    * 反例

    ```python
    def add(a, b):
        if isinstance(a, int) and isinstance(b, int):
            return a + b
        else:
            return str(a) + str(b)
    ```
>
35. 【推荐】对于未知的条件分支或不应进入的分支，应该抛出异常，而不是返回一个值【如 None、False】。

    * 正例

     ```python
    def foo(x):
        if x in ('SUCCESS',):
            return True
        else:
            # 如果一定不会走到的条件，应该增加异常，防止将来未知的语句执行。
            raise Exception() 
     ```

    * 反例

     ```python
    def foo(x):
        if x in ('SUCCESS',):
            return True
        return None # 如果对未知的分支返回None，可能导致后续未知的执行，产生不可控的结果，增加寻找bug时间。
     ```
---
36. 【参考】应该充分使用 Python 的语法，但是不应该过度的使用 Python 的奇技淫巧

    * 说明：比如 Python 的 Slice 语法（我们编程时常犯的错误）

    * 正例

     ```python
    a = [1, 2, 3, 4]
    c = 'abcdef'
    print(list(reversed(a)))
    print(list(reversed(c)))
     ```

    * 反例

     ```python
    a = [1, 2, 3, 4]
    c = 'abcdef'
    print(a[::-1])# 不知道切片代表的意思，可以定义临时变量命名来增加可读性。
    print(c[::-1])
     ```
>
37. 【参考】使用 dict来返回多个值。

    ```python
    # 一个函数返回多个值
    def latlon_to_address(lat, lon):
        return country, province, city
    
    # 但是，这样的用法会产生一个小问题：
    # 如果某一天，latlon_to_address 函数需要增加返回城区「District」时怎么办？
    # 如果是上面这种写法，你需要找到所有调用 latlon_to_address 的地方，
    # 补上多出来的这个变量，否则就会抛出 ValueError: too many values to unpack 异常。 ValueError: too many values to unpack  异常。
    ```

    *  对于这种可能变动的多返回值函数，使用 dict会更方便一些。当你新增返回值时，不会对之前的函数调用产生任何破坏性的影响

    ```python
    def latlon_to_address(lat, lon):
        return {
            'country': country,
            'province': province,
            'city': city
        }
    
    addr_dict = latlon_to_address(lat, lon)
    ```

38. 【参考】多参考标准库，标准库中有很多实用的功能。如 operator.methodcaller()  等。
operator.methodcaller() 是Python标准库中的一个函数，用于创建一个可调用对象，用于调用对象的指定方法。这个函数通常与高阶函数（如 map 和 sorted）一起使用，以便对对象集合中的元素执行特定的方法。

operator.methodcaller() 的主要作用是提供一种更方便的方式来调用对象方法，尤其在函数式编程中很有用。它可以接受一个方法名和可选的参数，然后返回一个可调用对象，用于在后续操作中调用该方法。

以下是 operator.methodcaller() 的基本用法示例：
```python
import operator

class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def get_name(self):
        return self.name
    
    def get_age(self):
        return self.age

people = [Person("Alice", 30), Person("Bob", 25), Person("Charlie", 35)]

# 使用 operator.methodcaller 调用 get_name 方法
get_name = operator.methodcaller("get_name")
names = map(get_name, people)

# 使用 operator.methodcaller 调用 get_age 方法
get_age = operator.methodcaller("get_age")
ages = map(get_age, people)

print(list(names))  # 输出 ['Alice', 'Bob', 'Charlie']
print(list(ages))   # 输出 [30, 25, 35]
```
39. 【参考】使用 []，{}而不是 list，dict（速度更快）。

在Python中，使用 [] 和 {} 语法创建列表和字典比使用 list 和 dict 构造函数通常更快，因为它们是更轻量级的语法形式。以下是一个Python例子，演示了如何使用 [] 和 {} 创建列表和字典：
```python
# 使用 [] 创建一个空列表
my_list = [] # 快
my_list = list() # 慢

# 使用 [] 创建一个带有初始值的列表
numbers = [1, 2, 3, 4, 5]
fruits = ["apple", "banana", "cherry"]

# 使用 [] 进行列表操作
fruits.append("orange")
fruits.remove("banana")

print(fruits)  # 输出 ['apple', 'cherry', 'orange']

```
总之，使用 [] 和 {} 语法创建列表和字典通常更简洁，而且速度更快，因为它们是Python语言的一部分，不需要调用构造函数。这种方式在实际编程中非常常见。
40. 【参考】使用for/else简化异常处理。

    * 以下两段代码等价

    * 我们借助了一个标志量 found 来判断循环结束是不是由 break语句引起的

    ```python
    def find_index(nums: list[int], target: int) -> None:
        found = False
        for index, num in enumerate(nums):
            if num == target:
                found = True
                break
    
        if found:
            print("not find {target}")
        else:
            print("find {target} at index {index}")
    ```

    ```python
    def find_index(nums: list[int], target: int) -> None:
        for index, num in enumerate(nums):
            if num == target:
                print("find {target} at index {index}")
                break
        else:
            print("not find {target}")
    ```

## **二、编码原则**

此部分提供一些编码的指导，特别是在多种写法在语法上都正确的时候，应该如何抉择。
>
1. **【推荐】** **进攻式实现、防御式调用**
* 对于实现：没有人能保证自己的代码「完美」地处理所有 case。
  
* 对于调用：没有不出错的代码，但没有人希望自己的系统因别人的手残挂掉（假定外部系统是沙子）。
>
2. **【推荐】** **避免"魔鬼数字"**

   * 如果一个代码中频繁出现不知道具体意义的数字,会极大影响代码可读性,甚至过一周后代码开发者本人也会忘记,切记不能使用!!!
   * 反例-节选自"metro_aeb":
   
    ```python
   def point_in_plane(self,p_raw,equ_):
      
      if equ_[2] < 0: # 2是什么?
        for i in range(len(equ_)):
          equ_[i] = - equ_[i]
     
      self.equ_list.append(equ_)
      if len(self.equ_list ) > self.midian_fliter_size:
       del self.equ_list[0]
   
      tmp_array = np.array(self.equ_list)
   
      a_sort = np.sort(tmp_array,axis=0)  #列排序 中值滤波
   
      t = len(a_sort) // 2 # 2是什么?
   
      equ_ = np.array([a_sort[t][0],a_sort[t][1],a_sort[t][2],a_sort[t][3]])# 1，2，3是什么
   
      
      idx_smaller_70 = np.where(p_raw[:,1] < self.dist_to_fit ) #idx_smaller_70什么意思,后面切片p_raw[:,1]是什么?
          
      pt_small_70 = p_raw[idx_smaller_70[0]]
   
      idx_bigger_70 = np.where(p_raw[:,1] > self.dist_to_fit )
      pt_big_70 = p_raw[idx_bigger_70[0]]
      # pt_big_70 = np.delete(p_raw,idx_smaller_70,axis = 0)  
     
      points_ = pt_small_70[:,:3].T
      # points=np.delete(points,inliers,axis = 0)  
   
      equ_ = np.array(equ_)
      one_array = np.ones(points_.shape[1])
      point_t = np.vstack([points_,one_array])
   
      z = np.dot(equ_,point_t)
   
      if equ_[2] < 0:
        idx = np.where( z<-self.lift_plane )
      else:
        idx = np.where( z>self.lift_plane)
      points_ = points_.T
      points_clip = points_[idx]
      points_clip = np.hstack(([points_clip],[pt_big_70[:,:3]]))
      return points_clip[0]
    ```
   
>
3. **【推荐】** **不放大错误**
* raise 不是用来推卸责任。 合理的 assert、raise只是向外声明：我的代码发生了一个不可忽视的错误。代码的调用者必须负责处理这个错误（因为我声明了它不可忽视）。
  
* 维护者：raise不是推卸责任，维护者有义务抛出并且只抛出被认为不对的 case。
  
* 调用者：调用别人的函数，就应该遵守别人的规范。调用方需要自己判断，是否需要继续 raise 给上层。
* 反例：
```python
    def divide(a, b):
        if b == 0:
            raise Exception("除以零")  # 避免使用通用的异常类，不显示上下文信息
        return a / b

    def process_data(data):
        try:
            result = divide(10, data)
            return result
        except Exception as e:
            # 在维护者层次捕获所有异常
            print(f"处理数据时出错: {e}")
            raise  # 将异常传递给调用者

    try:
        result = process_data(0)
        print("结果:", result)
    except Exception as e:
        # 在调用者层次捕获所有异常
        print(f"调用 process_data 时出错: {e}")
```
在这个反例中：
· 我们在 divide 函数中引发通用的 Exception，这不是推荐的做法，因为它缺乏具体性。
· 在 process_data 函数中，我们使用 Exception 来捕获所有异常，这使得很难区分具体的错误情况。
· 我们在维护者层次处理所有异常，并将它们传递给调用者，而没有提供清晰的错误消息。

* 正例：
```python
    # 自定义异常类
    class CustomError(Exception):
        def __init__(self, message):
            super().__init__(message)

    def divide(a, b):
        if b == 0:
            raise CustomError("不允许除以零")
        return a / b

    def process_data(data):
        try:
            result = divide(10, data)
            return result
        except CustomError as e:
            # 在维护者层次处理异常
            print(f"处理数据时出错: {e}")
            raise  # 将异常传递给调用者

    try:
        result = process_data(0)
        print("结果:", result)
    except CustomError as e:
        # 在调用者层次处理异常
        print(f"调用 process_data 时出错: {e}")

```
在这个正例中：
· 我们定义了一个自定义异常类 CustomError。
· 在 divide 函数中，如果分母为零，则会引发 CustomError 异常，这是一个被识别的错误。
· 在 process_data 函数中，我们捕获异常，打印错误消息，然后重新引发异常以将其传递给调用者。
· 在主程序中，我们调用 process_data，捕获和处理异常，并打印错误消息。

4.  **【推荐】** **严厉排查死循环**
   * 无论多么小的while true循环，**当break条件无法触发**时（因为各种 bug），我们的服务就死掉了。
   
5.  **【推荐】** **合理的处理异常**
    * 代码不可能永不出错，合理的处理错误，能提高代码的健壮性。对于出错的处理情况，可以大致分为三类。
    * 调用者不关心的异常：**降级函数**代替（确保降级函数不会出错）
    * 例子：
```python
import requests

def fetch_data(url):
    try:
        response = requests.get(url)
        response.raise_for_status()  # 检查是否有异常状态码
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"无法获取数据：{e}")
        return get_default_data()  # 使用默认数据代替

def get_default_data():
    # 返回默认数据
    return {"message": "无法获取数据"}

url = "https://nwpu//Yao.com"
data = fetch_data(url)
print(data)
```

在这个例子中，如果网络请求失败（例如，无法连接到服务器），则 fetch_data 函数不会抛出异常，而是返回默认数据，确保调用者不需要特别处理网络问题。


* 关键功能异常：使用 raise抛出(上面的情况)

* 重要但是不应该影响主流程的异常：降级函数 + 日志报警
* 例子：
```python
import logging

def process_data(data):
    try:
        # 处理数据的关键功能
        result = perform_critical_operation(data)
        return result
    except Exception as e:
        # 记录异常日志
        logging.error(f"处理数据时发生错误：{e}")
        return fallback_operation(data)

def perform_critical_operation(data):
    # 关键功能的实现
    if not is_critical_condition_met(data):
        raise RuntimeError("关键条件未满足")
    return data

def is_critical_condition_met(data):
    # 检查关键条件是否满足
    return True

def fallback_operation(data):
    # 降级操作
    return data

data = {"value": 42}
result = process_data(data)
print("结果:", result)
```
在这个例子中，process_data 函数尝试执行关键功能，如果发生异常，它会**记录错误日志并执行降级操作**，以确保**主流程不会中断**，但仍然提醒开发人员注意异常情况。

6.  **【推荐】** **接口返回类型统一**

    * 接口的返回类型尽量统一，减少 None值的返回。
    
    ```python
    def get_number_list(num: int):
        if num < 10:
            return 0
        elif 10 <= num < 100:
            return [10 for i in range(10)]
        else:
            return None # 应该改为抛出异常
    ```

   * 一个命名为 get_number_list的函数应当总是返回数组或者抛出异常。

7.  **【推荐】** **只做自己该做的事情**

    ```python
    def pickup_slotcard(feeds):
        ...
        for feed in feeds:
            if feed.type == slot_card:
                return format_feed(slot_card)
        return None
    ```

   * pickup_slotcard：找出 feed 中的slot card ，只要负责找就行了，不要干别的事情，不能想到什么功能就写什么功能，会使得代码**非模块化**，函数功能不单一，**耦合严重**。

        * format_feed 不需要做。

8.  **【推荐】** **尽量避免默认参数**

    * 默认意味着参数可以不传，容易给调用者造成困扰：是传递还是不传？
    
    *  不负责的调用者：直接就不传了。
    
    * 负责的调用者：看下文档，代码才知道怎么传。
    
    * 代码维护者：如果对默认值进行修改，则必须**排查所有调用方**，增加时间成本。

9. **【推荐】** **合理分层，判断参数是否合理**

   * 对于那些只是为了控制调用流程的参数，可以考虑拆分单独函数。
   * 例子1：
   ```python
   #反例
   def func(control, a, b ,c):
       if control == 1:
           pass
       elif control == 2:
           pass
       else:
           pass
   return
   
   # 只实现功能逻辑， 调用掌管业务逻辑
   def f_control_1(a, b, c)
   def f_control_2(a, b, c)
   def f_control_3(a, b, c)
   ```
   * 例子2（根据客户类型打折后价格）：
   ```python
   def calculate_discount(price, customer_type):
   if not is_valid_customer_type(customer_type):
       raise ValueError("无效的客户类型")
   
   discount_rate = get_discount_rate(customer_type)
   discounted_price = price - (price * discount_rate)
   return discounted_price
   
   def is_valid_customer_type(customer_type):
       # 检查客户类型是否有效
       valid_types = ["regular", "premium", "VIP"]
       return customer_type in valid_types
   
   def get_discount_rate(customer_type):
       # 获取客户类型对应的折扣率
       discount_rates = {
           "regular": 0.1,
           "premium": 0.2,
           "VIP": 0.3
       }
       return discount_rates.get(customer_type, 0.0)
   
   # 调用示例
   customer_type = "premium"
   price = 100.0
   discounted_price = calculate_discount(price, customer_type)
   print(f"折扣后价格：${discounted_price:.2f}")
   ```
   在这个例子中，我们进行了合理的分层，并判断了参数是否合理：
   （1）calculate_discount 函数负责**计算折扣后的价格**，但它不处理客户类型的验证和折扣率的获取。
   （2）is_valid_customer_type 函数用于**验证客户类型是否有效**。
   （3）get_discount_rate 函数用于**获取客户类型对应的折扣率**。
   这种分层和参数验证的方法使得代码更具**可读性**和**可维护性**。我们将**不同的责任分配给不同的函数**，确保每个函数专注于特定的任务，而**不是混合在一起**。这样的代码更易于扩展和维护，因为我们可以在**不影响其他部分的情况下修改特定函数**的行为。

10. **【推荐】** **类设计，少用继承，多用组合。**

   * 过深的继承链会导致代码维护成本成倍的增加。

11. **【推荐】** **压缩类属性**

   * 如果一个类有很多相互关联的属性，其中某些熟悉由其他属性计算而出。那么这样的属性应该被去除，转而使用 get_xx 方法获取，或者使用 property。

   * 没有衍生属性，类的 init 变得简单，代码的可测试性得到提高，可维护性得到提高。

~~~python
```
class Sample:

    def __init__(self, price: float, amount: int):
        self.price = price
        self.amount = amount

    @property
    def get_profit(self):
        return self.price * self.amount
```
~~~

12. **【推荐】** **禁止改变函数签名【里氏替换原则】**

   * 所有用到父类方法的地方，都可以用子类替换的同时而不产生代码语法错误，类型错误。

```python
class Base:
    def func(self, a: int):
        raise NotImplementedError()
```


```python
class ClassA(Base):
    def func(self, a: int):
        pass
```


```python
class ClassB(Base):
    def func(self, a: int):
        pass
```


~~~python
class ClassC(Base):
    def func(self, b: list[str]):  # 作者觉得参数不满足
        pass
        
# 代码调用者
ClassA().func(a)   # no bug
ClassB().func(a)   # no bug
ClassC().func(a)   # bug?  why?  怎么会挂？？？傻了？
```
~~~

13. **【推荐】** **考虑使用者的感受**

   * **写代码时，【考虑的是怎么写，还是怎么用】** 是区分是否是优秀工程师的一个重要指标。只对作者友好的代码绝对称不上是优秀的代码。面向工程，面向合作，要让用的人舒服，让用的人 **心理成本低。**

14. **【推荐】** **稍后等于用不【**LeBlanc （勒布朗）法则】

   * 如果旧的架构**不能满足新的需求**，那就需要调整，而且是尽快调整。

   * 合理的时间进行合理的重构才能让架构足够的活力，而不是「下次再说」。所有的新功能都是「应该在哪里」，而不是「狗皮膏药」漫天飞。

15. **【推荐】** **尽量无状态设计**

   * 有状态的代码，可维护性，可测试性都会大大降低。测试代码时，维护者需要痛苦的构造上下文，debug 场景难以复现
   * 【反例】
```python
    class ShoppingCart:
        def __init__(self):
            self.items = []

        def add_item(self, item):
            self.items.append(item)

        def remove_item(self, item):
            self.items.remove(item)

        def calculate_total(self):
            total = 0
            for item in self.items:
                total += item.price
            return total

    class Item:
        def __init__(self, name, price):
            self.name = name
            self.price = price

    # 使用有状态设计的购物车
    cart = ShoppingCart()
    item1 = Item("Laptop", 1000)
    item2 = Item("Phone", 500)
    cart.add_item(item1)
    cart.add_item(item2)
    total_price = cart.calculate_total()
    print(f"Total price: ${total_price}")

```
在这个示例中，ShoppingCart 类维护了一个 items 列表来跟踪购物车中的商品。这个设计是有状态的，因为购物车的内容随着时间的推移而变化，这会导致以下问题：

(1)**难以测试**：要测试 calculate_total 方法，需要首先创建购物车对象，添加商品，然后再计算总价。这使得测试变得复杂，因为测试依赖于购物车的状态。
(2)**难以维护**：如果购物车类的实现发生变化，可能需要修改多个地方，包括购物车的添加和删除商品的方法。

   * 【正例】
```python
    def calculate_total(items):
        total = 0
        for item in items:
            total += item.price
        return total

    class Item:
        def __init__(self, name, price):
            self.name = name
            self.price = price

    # 使用无状态设计的购物车
    item1 = Item("Laptop", 1000)
    item2 = Item("Phone", 500)
    items_in_cart = [item1, item2]
    total_price = calculate_total(items_in_cart)
    print(f"Total price: ${total_price}")
```
在这个示例中，我们没有使用购物车类，而是将购物车的内容表示为一个无状态的列表 items_in_cart。这个设计是无状态的，因为计算总价的函数不依赖于任何对象的状态。
无状态设计的优势包括：
**(1)易于测试**：我们可以轻松地测试 calculate_total 函数，因为它只依赖于输入参数 items，无需创建购物车对象。
**(2)易于维护**：无状态设计不依赖于对象状态，因此更容易维护和修改。
总之，尽量避免使用有状态的设计，将逻辑表示为无状态函数或方法，可以提高代码的可维护性和可测试性。

16. **【推荐】** **删除无用代码**（YAGNI 原则）

   * You Ain't Gonna Need It（你不会需要它），好处：

   * 节约工程成本，避免编写不必要代码，无需维护这些代码。

   * 提高代码质量，不会让暂时无用甚至永远无用的代码，污染项目。

   * 开发者不应该编写无用逻辑，也 **不应该** 允许无用逻辑的存在。所有下线的逻辑（被注释的代码、没有调用的函数、永远为 False 的判断）必须干掉代码！

   * 如果将来某天需要，可以依赖 git 版本控制、仓库打 tag、文档维护 commit 节点等等，会有无数个比注释掉代码更高效的方案。

17. **【推荐】** **好的代码胜过注释【代码本身即注释】**

   * 良好的变量、函数命名是最好的注释。

   * 因为注释没有任何机制保证强制更新，不及时的注释容易更其他人带来更多的理解上的困惑。依赖注释的代码很可能由「工程导向」恶化成「文档导向」。漫天都是解释这个几百行函数在干嘛，可是没人能看懂。

18. **【推荐】** **减少变量，让变量尽量局部**

   * 变量的中间赋值、写逻辑，往往是代码千奇百怪 bug 的导火索。让变量的使用限制在尽量小的范围。

   * 可以通过使用子函数、代码块等手段，来减少变量漫天飞的情况。
   * 【反例】
考虑一个简单的任务，计算一组数字的平均值。首先，我们来看一个使用大量中间变量的实现：
```python
    def calculate_average(numbers):
        # 步骤1：计算总和
        total = 0
        for num in numbers:
            total += num

        # 步骤2：计算平均值
        average = total / len(numbers)

        # 步骤3：返回平均值
        return average
```
在上面的示例中，使用了多个中间变量（total 和 average）来执行计算过程。虽然这段代码能够正确工作，但它使用了多个变量，并且将计算过程分成多个步骤，可能导致维护和理解变得困难。
现在，让我们来看一个减少变量的示例，将变量限制在更小的范围内:

```python
    def calculate_average(numbers):
        # 计算并返回平均值，无需中间变量
        return sum(numbers) / len(numbers)
```
在这个示例中，我们消除了中间变量 total 和 average，将计算过程简化为一行代码。这样做的好处包括：
**(1)可读性更高**：减少了代码中的变量，使代码更**简洁**，易于**理解**。
**(2)可维护性更好**：不需要**维护多个中间变量**，减少了出错的机会。
**(3)性能更好**：不需要额外的**存储空间**来存储中间结果，因此在某些情况下可以提高性能。
这个示例强调了将变量限制在尽量小的范围内，尽可能**减少中间变量的使用**，以提高代码的清晰度和可维护性。

19.  **【推荐】** **接口隔离原则 **【**Interface Segregation Principle**】

   * 一个类，方法，不应该依赖于它不需要的接口（与单一职责原则有相似之处）
假设我们正在设计一个多媒体播放器应用程序，其中包括音频和视频功能。我们希望定义一个接口来处理播放媒体文件的操作，但不同类型的媒体（音频和视频）可能需要不同的功能。在不遵循ISP的情况下，接口可能包含太多的方法，导致类需要实现不必要的功能。
```python
    # 不符合ISP的示例
    class MediaPlayer:
        def play_audio(self):
            pass

        def play_video(self):
            pass
```
在上面的示例中，我们定义了一个名为 MediaPlayer 的接口，其中包含了 play_audio 和 play_video 两个方法。现在，假设我们有一个只关心音频播放的类：
```python
    class AudioPlayer(MediaPlayer):
        def play_audio(self):
            # 实现音频播放的逻辑
            pass

        def play_video(self):
            # 这个方法对于AudioPlayer来说是不需要的
            pass
```
上述设计违反了ISP，因为 AudioPlayer 类不需要实现 play_video 方法，但它被迫要实现它。这导致了不必要的方法实现，降低了代码的清晰性和可维护性。
修正的示例，符合ISP：

```python
    # 定义音频播放接口
    class AudioPlayer:
        def play_audio(self):
            pass

    # 定义视频播放接口
    class VideoPlayer:
        def play_video(self):
            pass
```
在这个示例中，我们将音频播放和视频播放的功能分别定义在两个不同的接口中，每个接口只包含相关的方法。这样，类可以选择性地实现所需的接口，而不需要实现不相关的方法，符合ISP原则。例如，AudioPlayer 只需要实现 AudioPlayer 接口，而不需要关心视频播放的方法。
通过遵循ISP，我们可以更好地组织接口和类，使代码更清晰、更可维护，并**降低不必要的依赖关系**。这有助于确保每个类仅依赖于其需要的接口，而不会受到不相关接口的影响。

20.  **【推荐】** **依赖倒置原则 **（**Dependence Inversion**）

   * 高层方法不依赖于低层方法，二者都应该依赖于抽象。
假设我们正在设计一个电子邮件通知系统，其中有两种通知方式：电子邮件通知和短信通知。我们希望确保高层模块（通知发送者）不依赖于低层模块（具体通知方式），而是依赖于通用的抽象接口。

不符合DIP的示例：
```python
    class EmailNotifier:
        def send_notification(self, message):
            # 实现电子邮件通知的逻辑
            pass

    class SMSNotifier:
        def send_notification(self, message):
            # 实现短信通知的逻辑
            pass

    class NotificationSender:
        def send_email_notification(self, message):
            email_notifier = EmailNotifier()
            email_notifier.send_notification(message)

        def send_sms_notification(self, message):
            sms_notifier = SMSNotifier()
            sms_notifier.send_notification(message)
```
在上面的示例中，NotificationSender 类依赖于具体的 EmailNotifier 和 SMSNotifier 类，这违反了DIP。高层模块 NotificationSender 直接依赖于低层模块的具体实现。

修正的示例，符合DIP：
```python
    from abc import ABC, abstractmethod

    # 定义通知方式的抽象接口
    class Notifier(ABC):
        @abstractmethod
        def send_notification(self, message):
            pass

    # 实现具体的电子邮件通知
    class EmailNotifier(Notifier):
        def send_notification(self, message):
            # 实现电子邮件通知的逻辑
            pass

    # 实现具体的短信通知
    class SMSNotifier(Notifier):
        def send_notification(self, message):
            # 实现短信通知的逻辑
            pass

    class NotificationSender:
        def __init__(self, notifier):
            self.notifier = notifier

        def send_notification(self, message):
            self.notifier.send_notification(message)
```
在这个示例中，我们引入了一个名为**Notifier**的抽象接口，定义了通用的 **send_notification**方法。具体的通知方式（**EmailNotifier**和**SMSNotifier**）都实现了这个接口。

**NotificationSender**类不再依赖于具体的通知方式类，而是通过构造函数注入一个实现了 **Notifier**接口的对象。这样，高层模块**NotificationSender**依赖于抽象接口 **Notifier**，而不依赖于具体的实现，符合**DIP**原则。

通过遵循DIP，我们可以降低模块之间的依赖性，使代码更具**灵活性**和**可扩展性**。这样，我们可以轻松地添加新的通知方式，而无需修改高层模块的代码。

21.  **【推荐】** **开闭原则 **（**Open Closed Principle**）
   * 一个软件实体如类、模块和函数应该**对扩展开放**，对**修改关闭**。模块应**尽量在不修改原代码的情况**下进行扩展。

    **上面的例子对此原则也有体现**（当需要增加通知方法时，只需要增加一个子类，而不需要修改任何代码）

22.  **【推荐】** **迪米特法则 **（**Law of Demeter**）
   * 迪米特法则则不希望类之间建立直接的关系。如果真的有需要建立联系，也希望能通过它的友元类（中间类或者跳转类）来转达。这有助于减少类之间的耦合，提高代码的可维护性和灵活性。
例子：体育课上，体育老师要进行点名，她让体育课代表去点名。类Teacher与类 StudentList, Student, Representative都有耦合关系，但他们都是内嵌到类Teacher的command方法中。这就不符合迪米特原则了。代码如下：
```python
    class Teacher(object):

        def __init__(self):
            pass

        def command(self):
            print('体育老师让课代表点名')
            studentlist = StudentList()
            for _ in range(20):
                studentlist.append(Student())
            print('课代表开始点名')
            representative = Representative()
            representative.count(studentlist)


    class Student(object):
        pass


    class StudentList(list):
        pass


    class Representative(object):

        def count(self, studentlist):
            print('课代表已经点完名，共有人数%d' % len(studentlist))


    class Client(object):

        def main(self, teacher):
            teacher.command()

    if __name__ == '__main__':
        client = Client()
        teacher = Teacher()
        client.main(teacher)
```
把上面的代码**修改为符合迪米特原则**
从上面的代码可知，**teacher**的下级是**representative**，其应该只与**representative**是直接朋友关系。**representative**也应该只与**studentlist**是直接朋友关系。因此代码可以修改为这样：

```python
    class Teacher(object):

        def __init__(self, representative):
            self.representative = representative

        def command(self):
            print('老师让课代表点名')
            self.representative.count()


    class Representative(object):

        def __init__(self, studentlist):
            self.studentlist = studentlist

        def count(self):
            print('课代表开始点名')
            self.studentlist.size()


    class StudentList(list):

        def add_student(self, num):
            for _ in range(num):
                self.append(Student())

        def size(self):
            print('学生的人数为%s' % str(len(self)))


    class Student(object):
        pass


    class Client(object):
        def main(self, teacher):
            teacher.command()

    if __name__ == '__main__':
        studentlist = StudentList()
        studentlist.add_student(10)
        representative = Representative(studentlist)
        teacher = Teacher(representative)
        client = Client()
        client.main(teacher)
```
从修改后的代码可以看出，**teacher**，**representative**，**studentlist**是层级关系，这种关系也可以以内嵌的形式都写进**teacher**的**command**方法中，也可以像修改后的代码一样，采用逐级传递参数的方式来反映层级关系。

23.  **【推荐】** **合成复用原则 **
   * 原则是尽量使用合成/聚合的方法，而不是使用继承。

24.  **【推荐】** **单一职责原则 **
   * 它规定一个类应该只负责一项职责。
单一职责原则注意事项和细节：
（1）、降低类的复杂度，一个类只负责一项职责。
（2）、提高类的可读性，可维护性。
（3）、降低变更引起的风险
（4）、通常情况下，我们应当遵守单一职责原则，只有当逻辑足够简单时，才可以在代码级别违反单一职责原则。
***
## **三、总结**
**设计原则的核心思想:**
【1】找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起。
【2】 针对接口编程，而不是针对实现编程。
【3】为了交互对象之间的松耦合设计而努力。
**简单理解就是：**
**开闭原则是总纲**，它指导我们要对扩展开放，对修改关闭;
**单一职责原则**指导我们实现类要职责单一;
**里氏替换原则**指导我们不要破坏继承体系;
**依赖倒置原则**指导我们要面向接口编程;
**接口隔离原则**指导我们在设计接口的时候要精简单一;
**迪米特法则**指导我们要降低耦合;

设计模式就是通过这七个原则，来指导我们如何做一个好的设计。但是设计模式不是一套“奇技淫巧”，它是一套方法论，一种高内聚、低耦合的设计思想。我们可以在此基础上自由的发挥，甚至设计出自己的一套设计模式。

学习设计模式或者在工程中实践设计模式，必须深入到某一个特定的业务场景中去，再结合对业务场景的理解和领域模型的建立，才能体会到设计模式思想的精髓。如果脱离具体的业务逻辑去学习或者使用设计模式，那是极其空洞的。

**对于科研,尽量遵守编程规范会增加科研效率**:
**(1)当你毕业时,或者别人接手你的项目时能更加快速上手;**
**(2)当你你要在已有代码上增加新的idea时,能尽量少修改源码,且快速实现,还方便git版本管理;**
**(3)理解编码规范,会提升对软件设计的理解,同时养成良好编程习惯有利于找工作;**