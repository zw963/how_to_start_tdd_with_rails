## How to start TDD with Rails

----

## TDD

What is TDD ?
<!-- .element: class="fragment" -->

<p class="fragment"><span class="fragment highlight-red">Test</span>-Driven Development</p>

---

### Red/Green/Refactor Cycle

- Write test first <!-- .element class="fragment" -->
<li class="fragment">See it <span class="fragment highlight-red">fail<span></li>
- Write code <!-- .element class="fragment" -->
<li class="fragment">See test <span class="fragment highlight-green">pass<span></li>
<li class="fragment"><span style="color:yellow">Refactoring</span>, Yep!</li>
 
---

![red_green_refactor.jpg](http://zw963.github.io/images/21746255897926.jpg)

### Write Test First
<!-- .element class="fragment" -->

Thinking code from test.
<!-- .element class="fragment" -->

<p class="fragment">GDD<span class="fragment"> -> Guess-Driven Development<span></p>

---

### Looks Good ？

## <span style="color:red">NO !</span>
<!-- .element: class="fragment" -->

<img src="http://zw963.github.io/images/206073273225308.png" alt='ddh.png' height="200" width="200">
<!-- .element: class="fragment fade-left" -->

<small>My name is David, and I do not write software test-first.</small>
<!-- .element: class="fragment fade-left" -->

<small>I refuse to apologize for that any more, much less hide it.</small>
<!-- .element: class="fragment fade-left" -->
---

<small>David Heinemeier Hansson(@dhh), creator of Ruby on Rails, Founder & CTO at Basecamp (formerly 37signals).</small>

Railsconf 2014 [keynote](https://www.youtube.com/watch?v=9LfmrkyP81M) 之后, David 写了一篇 [blog](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)

博客的标题是 ...

![teed_is_dead.png](http://zw963.github.io/images/20656313371523.png)
<!-- .element: class="fragment" -->
---

## TDD is <strong style="color:red">dead</strong>

# END #
<!-- .element: class="fragment" -->

好吧, 开玩笑.
<!-- .element: class="fragment" -->

<p class="fragment fade-left">但是, <span class="fragment fade-left">如果我当年就知道 David 这么说 ...<span></p>

我可能就不写测试了.
<!-- .element: class="fragment fade-left" -->

---

### 由此引发开 OSS 一场世界级的口水战!

https://martinfowler.com/articles/is-tdd-dead/
![martin.png](http://zw963.github.io/images/12997113904704.png)

---

大家是不是都只看标题前半句, TDD is <strong style="color:red">dead</strong>!

其实还有后半句
<!-- .element: class="fragment" -->

<p>
![teed_is_dead.png](http://zw963.github.io/images/20656313371523.png)
测试万岁!
</p>
<!-- .element: class="fragment" -->

<small>严格遵循 `测试先行' 的教条主义, 就像 XXXXX(不会翻译), 根本是不切实际的、无效的宣传,只会带来自我厌恶和羞愧。</small>
<!-- .element: class="fragment" -->

---

## Does we need **test first ?**

下面是 David 的一些观点.

---

1. 我不以 **测试先行** 的方式开发软件, 在公开场合，我也不再打算碍于我是社区的公众人物, 就一味昧着良心承认，**测试先行** 是总是正确的做法
2. 狂热的 TDD 导致过度关注单元测试(Unit Test)，为了高速测试，而过度隔离(isolate)，导致滥用 stub/mock, (例如：数据库操作, IO 操作)
3. 而我们应该将测试的重点放到(更慢的) *集成(Integration)测试* 和 *系统(System)测试*, 使用 *真实的数据* 来进行测试。
5. 避免进入另一个教条, 只写系统/系统测试。
6. 对我来说, TDD 已死, 但是测试万岁!

---

## 另一方的意见

1. TDD 也在演进, 里没有说一定要 isolated unit test 才算 TDD, 集成(Integration)/系统(System) 测试, 某个角度来说, 也可以称作 TDD.
2. TDD 的测试先行, 带来很多额外的好处, bla bla ... 讲了很多, 也很有道理.
3. 也承认 TDD 同样有它不适合额领域, 例如: 针对 UI 的测试等.
4. 你是大神, TDD 对你来说已死不意外, 但是不代表别人也一样, 对没接触过 TDD 的开发者, TDD 可能是一个不能错过的强大武器.

---

## 一些相关的链接

- David 写的有关 TDD 的三篇 blog: [1](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html) [2](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html)
[3](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html)

- Kent 大叔的 [rip-tdd](https://www.facebook.com/notes/kent-beck/rip-tdd/750840194948847)

- Bob 大叔 [Monogamous TDD](http://blog.cleancoder.com/uncle-bob/2014/04/25/MonogamousTDD.html)

- [When-tdd-does-not-work](https://8thlight.com/blog/uncle-bob/2014/04/30/When-tdd-does-not-work.html)

- 一些音频视频  [Martin Fowler/Kent/David](https://martinfowler.com/articles/is-tdd-dead/)

- David 的推特: https://twitter.com/dhh

---
### 特定于 Rails, 再加一些我自己的观点

<small>
- Unit 测试和 Integration 测试都需要, 取决于当前业务场景, 复杂的业务逻辑, 可能需要细分单元测试,
  但是在业务逻辑变得很复杂之前, 编写集成测试更快速, 有效.
- Rails 里面写单元测试, 大多数也只是 **数据**驱动的测试(也可以叫做 DDD?), 在这种测试中, 测试数据
  取代了绝大多数传统单元测试中, 需要 stub 方法 或 mock 对象的情形.
- 避免做过多的隔离, 这会导致, **当前测试的对象依赖的其他对象** 发生变化时, 测试用例变的无效, 
  而且, 随着硬件的提高, 因为性能原因, 而对 DB 或 IO 做隔离, 这样带来的提升已经微乎其微了.
- 我们的 backend 项目, 刚开始, 需要更多的集成测试, 来确保软件质量, 做回归测试, 但是, 慢慢的会引入
  更多粒度更加细致的单元测试, 两者具体孰轻孰重, 要看将来具体业务.
  
---

说下我写测试的方式.

<small>
1. 写一点很简单的实现, 注意: __即使很简单的实现, 也不一定一次就能写对__, 人总是倾向于犯错误.
2. 编写少量测试, 让测试通过，把 TDD 循环驱动起来．
3. 针对业务逻辑构造一些测试数据, 这里就是测试先行了, 但是在没有想清楚之前, 我不会去写太多断言, 
   因为没有实现代码, 这些测试肯定是挂的 (这也是传统 TDD 期望的行为, 先它挂掉, 再实现它), 只是我没有刻意这样做.
   为了完成这一步, 你可能需要思考我们需要那些表, 那些字段, 表之间的关系, 取决于是否新的业务.
4. 根据业务需求, 编写实现代码，此时会通过跑测试, 来驱动业务逻辑的开发, 
   这个过程中, 可能会不断完善测试数据, 并逐渐增加一些断言, 很多都是临时性的中间步骤.
5. 3,4 反复循环, 这就是 TDD 的过程, 你会从实现, 测试的两个角度, 思考同一个问题两次.
6. 最终实现业务逻辑和对应测试代码, 保证测试通过.
7. 时间允许, 会立即重构, 如果比较赶, 有测试代码在, 为稍后的重构, 提供信心.

---
### **这样做有什么好处？** 

<small>
其实上面的流程中已经说了很多了.
<!-- .element: class="fragment" -->

开发之前, 通过测试了解实现代码的功能, 后面会有个如果不看测试, 完全看不懂在干什么的单元测试例子, 事实上, 我们的 backend 项目也有同样的问题, 因为采用 graphQL 的缘故, 因为没有测试, 根本不知道 backend 对外提供那些接口, 相比传统 RESTful, 我们更需要测试.
<!-- .element: class="fragment" -->

<p>开发过程中, 快速的构造任意 业务逻辑所需数据, 通过 <span style="color: yellow">运行测试</span> 来驱动小步快速的开发迭代, 为在某个上下文, <span style="color: yellow">编写/验证</span>业务逻辑提供便利</p>
<!-- .element: class="fragment" -->

<p>留下大量测试代码用做 <span style="color: yellow">回归测试</span>, 不需要担心改动会引入很严重的 bug, 提高开发效率.(无法取代常规测试, 但会让测试同学测起来更有信心)</p>
<!-- .element: class="fragment" -->


<p>你在用图表和文字, 帮助在大脑里建立测试用例, 而我直接写出测试用例</p>
<!-- .element: class="fragment" -->

---

# DRY

Don't Repeat Yourself
<!-- .element: class="fragment" -->

(不做重复的工作！)
<!-- .element: class="fragment" -->

----
## 测试的分类

---

### 测试金字塔
![pyrmid](https://www.netguru.co/hs-fs/hubfs/blog-files/0-Why_TDD_is_still_alive_and_still_worth_using-03.png?t=1520108113073&width=1125&name=0-Why_TDD_is_still_alive_and_still_worth_using-03.png)

---

### 验收测试 (Acceptance Test)

<small>
这应该是给公司产品人员用的, 语言无关的, 抽象层次很高的测试.

如果你听说过 BDD (Behavior-driven development), 验收测试是很重要的一环, BDD 推崇自外向内的开发方式, 
你需要首先为当前业务场景, 编写一个个的 Story, 然后, 由产品人员使用一种语言无关的框架, 将这些 Story 
使用 feature/scenario 的方式描述出来.

这种测试我很少写, 在 Ruby 社区中, 编写验收测试, 应该都是用 Cucumber.

---
### 这是一个可以执行加法的计算器

```rb
class Calculator
  def push(n)
    @args ||= []
    @args << n
  end

  def sum
    @result = @args.reduce(:+)
  end

  def result
    @result
  end
end
```

```rb
calculator = Calculator.new
calculator.push(50)
calculator.push(30)
calculator.sum
calculator.result
```
---
### 为这个测试编写 feature

```feature
Feature: 这是一个计算器
  Scenario: 我希望执行两个数字相加
    Given 我有一个计算器
    And 输入 50
    And 输入 30
    When 求和
    Then 结果是 80.
```
---

### 这是针对 Feature 的 step 定义 

```rb
require_relative '../../calculator'

Given(/^我有一个计算器$/) do
  @calculator = Calculator.new
end

Given(/^输入 (\d+)$/) do |number|
  @calculator.push(number.to_i)
end

When(/^求和$/) do
  @calculator.sum
end

Then(/^结果是 (\d+)\.$/) do |result|
  # use minitest verify result
  assert_equal @calculator.result, result.to_i
end
```
---
![cucumber.png](http://zw963.github.io/images/29128651120239.png)

---
### 系统测试 (System Test) Or 集成测试 (Integration Test)

让 David 大神来回答吧！

![2.png](http://zw963.github.io/images/1360194626613.png)

---

[Rails 5.1 Release 公告](http://weblog.rubyonrails.org/2017/4/27/Rails-5-1-final)

![system_test_in_rails51.png](http://zw963.github.io/images/10924177720636.png)

---

### 系统测试示例

![3.png](http://zw963.github.io/images/285941003826844.png)

抽象度很高的测试.

---

### 系统测试示例

![4.png](http://zw963.github.io/images/259841889111943.png)

<small>
可以点击, 填入表单, 并测试页面中元素是否存在等, 类似于爬虫做的事.

---

感觉我们 backend 是 API 项目, 用不到系统测试中的大部分模块, 集成测试更有意义.

---

### Ruby 测试框架

RSpec
<!-- .element: class="fragment" -->

Minitest
<!-- .element: class="fragment" -->

<small>
RSpec 大而全, 包含各种你可能从来用不到的功能点, 采用 Spec 语法, 读起来更人性化, 完全社区维护, Cucumber + Rspec, 构成了 行为驱动开发(BDD).
<!-- .element: class="fragment" -->

MiniTest 小而快, 源码可以说是教科书一样的典范, 采用 Assert 语法, 功能也很全, Ruby 语言内置的测试框架, Rails 自带测试默认也采用 Minitest.
<!-- .element: class="fragment" -->

对于 Rails 测试来说, 两者大同小异, 都可以满足需求, 采用 Minitest 零配置, 直接可用.
<!-- .element: class="fragment" -->

但是仍然有很多公司选用 RSpec 作为 Rails 的框架. (这可能又是一场口水战 ......)
<!-- .element: class="fragment" -->

很多 Ruby Gem 也用 RSpec 作为测试框架, 我觉得是 RSpec 提供了一个 rspec 命令, 可以很方便的运行测试吧.
<!-- .element: class="fragment" -->

---

 这是 RSpec 编写集成测试的例子:
 
 ![5.png](http://zw963.github.io/images/20109110021741.png)

---

运行测试，

```sh
$: rspec spec/controller/some_controllers_spec.rb
```

输出结果是这个样子:

![8.png](http://zw963.github.io/images/387238602958.png)


---
<small>
但多数情况下, 如果写出来不是那么太文艺, 我们只是输出一些绿色的点点.
![7.png](http://zw963.github.io/images/323772869912546.png)

---

Rails 框架中默认测试使用 Ruby 自带的 Minitest.

Rails 很早的版本开始, 就一直自带一套测试框架.

测试是 Rails 不可分割的一部分.

使用 Rails 开发 Web 程序，写测试成本**非常低**

---

### 第一步：添加测试文件, 以及一些 scaffold 代码.

```rb
require 'test_helper'

class MainPageTest < ActionDispatch::IntegrationTest
  test '当进入 app 首页, 返回所有老师发送的列表' do
    
  end
end
```

这一步, Rails 已经帮我们做好了.
<!-- .element: class="fragment" -->

---

### 第二步：创建所需的测试数据

```rb
class MainPageTest < ActionDispatch::IntegrationTest
  test '#index' do
    # 创建 20 个学生, 还有对应的小孩记录
    students = 20.times.map { create(:student, :with_child) }
  end
end
```
<small>
FactoryBot (前身 FactoryGirl), 是用来方便的构建一系列 关联的 测试数据的 Gem
<!-- .element: class="fragment" -->

```rb
# 创建一条学生记录. (name 自动生成)
create(:student)

# 创建一条学生记录, 自己指定
create(:student, name: '笨笨熊')

# 创建一条学生记录, 同时创建对应的小孩记录.(全部自动生成)
create(:student, :with_child)

# 创建一条学生记录及小孩, 指定名称, 同时指定 child 的生日.(生日是 children 表中的字段)
create(:student, :with_child, name: '笨笨熊', birthday: '2015-12-25)                             )
```
<!-- .element: class="fragment" -->
---

### 构建更多的数据

```rb
# 创建 20 个学生, 还有对应的小孩记录.
students = 20.times.map { create(:student, :with_child) }
# 为幼儿园创建一个教师.
teacher = create(:teacher)
# 老师选择了一个主题.
subject = create(:subject, creator: teacher)
activity = create(
      :activity,
      :with_feed, # <== 创建一条相关的 feed
      subject: subject,
      creator: teacher
)

feed = activity.feed
    
# 这条活动包含 3 张图片
images = 3.times.map { create(:image, :with_feed_media, feed_id: feed.id) }

# 指定发送给一个小孩.
student4 = students[3]
create(:feed_visibility, feed: feed, student: student4, child: student4.child)
```

---
### 第三步：发送请求

```rb
class ReportsControllerTest < ActionDispatch::IntegrationTest
  test '#index' do
    # ...... 构建一大堆数据.
    post graphql_path(format: :json), params: {
      query:  { query: { .... } },
      operationName: "FeedListQuery",
      variables: {clazz_id: 1, first: 10}.to_json
    }
    assert_response :success
  end
end
```
---

### 最后一步：验证结果的正确性

```rb
class ReportsControllerTest < ActionDispatch::IntegrationTest
  test '#index' do
    # .....
    assert_response :sucess
    body = JSON.parse(response.body)
    assert_equal activty.id.to_s, body[data][activites].first
    # 更多的 assert ...
  end
end
```
---
 
## 单元测试

**最小模块**进行正确性验证

<small>

Ruby 中, 最小模块就是一个 public 可见性的方法定义.
<!-- .element: class="fragment" -->

Ruby 写 Gem 的时候常常有单元测试。
<!-- .element: class="fragment" -->

Github 贡献代码，不但要提交实现，基本上都要附上单元测试。
<!-- .element: class="fragment" -->

---

<small>
Rails 中的单元测试，几乎都是对 Model 层的业务逻辑进行测试。

最小单元的业务逻辑足够复杂时, 才需要写单元测试.

---

这是我以前写的一段代码.

```
class Category < ApplicationRecord
  has_many :goods_items
  has_many :followers, as: :followable, class_name: 'Follow'
  
  i18n_attributes :name # <= 这行代码什么鬼?
  
  validates :name, presence: true
end

```

<small>'i18n_attributes :name' 被调用的上下文的 self 就是 Category 自身.</small>
<!-- .element: class="fragment" -->

<small>因此 'i18n_attributes' 是一个类方法</small>
<!-- .element: class="fragment" -->

<small>继承了超类的类方法</small>
<!-- .element: class="fragment" -->


---

```rb
class ApplicationRecord < ActiveRecord::Base
  def i18n_attributes(*attributes)
    attributes.each do |attr|
      define_method("i18n_#{attr}") do
        i18n_config = method("#{attr}_i18n_config")
        return send(attr) if I18n.locale.to_s == 'zh-CN' or i18n_config.call.blank?

        rows = i18n_config.call.lines.each_with_object({}) do |row, o|
          key, value = row.split(':')
          o[key.strip] = value.strip unless value.nil?
        end

        rows[I18n.locale.to_s].presence || rows['en'].presence || name
      end
    end
  end
end
```

<small>我现在没时间研究这些元编程的鬼东西</small>
<!-- .element: class="fragment" -->


<small>我只想知道 i18n_attribute :name 是什么</small>
<!-- .element: class="fragment" -->



---

看测试一目了然

```rb
test "当用户提供了 name_i18n_config, i18n_name 输出 I18n 版本的 name" do
  category = create(:category,
    name: "其他",
    name_i18n_config: "
    en:others
    jp    :その他
    sg
    ru  :другое
  ")

  I18n.with_locale 'zh-CN' do
	assert_equal '其他', category.i18n_name
  end

  I18n.with_locale 'en' do
	assert_equal 'others, category.i18n_name
  end

  I18n.with_locale 'jp' do
	assert_equal 'その他', category.i18n_name
  end

  I18n.with_locale 'ru' do
	assert_equal 'другое', category.i18n_name
  end

  # 如果没有指定, 默认值使用 en.
  I18n.with_locale 'sg' do
	assert_equal 'others', category.i18n_name
  end
end
```

---

1. 测试用例帮助代码的读者(可能就是将来的你)了解被测试的代码
2. 作为回归测试, 保证需求的正确性.
3. 合理的测试数据, 在开发之前, 帮我我们编写代码.

---

### 不谈点 TDD 的缺点似乎没有信服力

<ol>
<li class="fragment">增加工作量<span class="fragment"> -> 尤其是从短期来看</span><span class="fragment"> 但是</span></li>

<small>如果考虑将来回归测试, 及修 bug 的成本, 这些都是值得的! </small>
<!-- .element: class="fragment" -->

<li class="fragment">维护测试很花时间<span class="fragment"> -> 它就是这样的</li>

<small>动态语言动态性, 为你高效开发提供了便利, 成本就是需要编写大量测试, 来保证代码质量, 事实上, 测试代码量远大于生产代码量, 
而且, 写测试或修改测试的时间, 通常也会大于编写生产代码的时间.</small>
<!-- .element: class="fragment" -->

<li class="fragment">从测试的角度思考, 熟悉测试框架, 都是学习成本</li>

</ol>

---

### TDD 不适合的领域

1. 不了解的东西, 不应该写测试
2. TDD 不适合初创业务, 变化特别快的部分, 等稳定后再写测试.
3. 针对 UI 写测试, 得不偿失, 你能想出来怎么给 CSS 样式写测试吗?

----

### 一些个人测试的经验分享

---

### 一、写测试不需要过分追求完整。

<small>
### 一开始只需要少量测试, 最重要的是, 先让 '测试驱动开发' 跑起来. 
<!-- .element: class="fragment" -->

### 如果有人报 bug, 先使用测试用例让 bug 重现(很重要), 再修复它.
<!-- .element: class="fragment" -->

### 改功能时, 慢慢加测试, 慢慢就多起来了
<!-- .element: class="fragment" -->

---

<p>关键是<span class="fragment"> -> 测试一定要有.</p>

不完美, 但是每个 entrypoint 都有测试, 
<!-- .element: class="fragment" -->

好过
<!-- .element: class="fragment" -->

写的很完美, 但是只覆盖到很少的 entrypoint.
<!-- .element: class="fragment" -->

---
### 二、如果很难为一个功能写测试
<small><h3>这可能味着 ***bad smell*** (来自于 Kent 大叔)</h3></small>
<!-- .element: class="fragment" -->

<small>
常常意味着: 代码耦合严重, 层次不清, 或接口混乱
<!-- .element: class="fragment" -->

意味着代码正在腐烂，需要很好的 refactor, 目的就是可以编写测试.
<!-- .element: class="fragment" -->

能够编写测试, 常常意味着, 代码有很好的关注点分离
<!-- .element: class="fragment" -->
---

讲一个特殊的例子, 可能有一点难懂.

![1.png](http://zw963.github.io/images/20985267967651.png)

如果要测试下面的条件.

```rb
if params[:renewal_users_count].to_i < @client.organiztion.users.count
    # ......
end
```
<small>params[:renewal_users_count] 需要可确定的值<span class="fragment"> -> 传进来就好了</span></small>
<!-- .element: class="fragment" -->

<small>@client.organiztion.users.count 返回可确定的值<span class="fragment"> -> 怎么办？</span></small>
<!-- .element: class="fragment" -->

---

FactoryBot, 以直接构建测试数据库的数据.

<small>
@client.organiztion.users.count

- create 一个 organization
<!-- .element: class="fragment" --> 
- create 10 个 user,
<!-- .element: class="fragment" --> 
- user 和 organiztion 建立关联
 <!-- .element: class="fragment" --> 
- client 和 organization 建立关联, 
<!-- .element: class="fragment" -->

在这个测试用例中, @client.organiztion.users.count 返回 10.
<!-- .element: class="fragment" -->

---

<small>
但是如果无法用 FactoryBot 构建 organization 和 user 怎么办?

例如: 当前项目在 Model 里面, 直接连接到另一个项目的表. 
<!-- .element: class="fragment" -->

两个项目, 本该通过 API 接口进行交互的
<!-- .element: class="fragment" -->
---

以 Rspec 为例

不得不用　test double (电影里替身的意思)

其实就是 TDD 里面常说的：mock 对象
<!-- .element: class="fragment" -->

不得不用 mock/stub, 有时候就意味着 bad smell.
<!-- .element: class="fragment" -->

---

<small>构建一个假的 users 对象, 使得 users.count 返回 10. </small>

```rb
users = double('user', count: 10)
```

<small>再构建一个假的 organization, 让 organization.users 返回上面假的 users</small>

```rb
organization = double('organiztion', users: users)
```

<small>最后，让 `@client.organization` 返回 假的 organization (做测试主体做 stub)</small>

```rb
expect(client).to receive(:organization).at_least(:once) { organization }
expect(Client).to receive(:find).with(client.id).at_least(:once) { client }
expect(Client).to receive(:find).with(client.id.to_s).at_least(:once) { client }
```

还没晕？
<!-- .element: class="fragment" -->

---

这就是 David 说的，很不好的实践, 我们把多个 model 层的对象，全部换成了假对象

因为只有这样，才能做到， 无需访问另一个项目的数据库．

错误的设计，造成过度的 mock/stub, 测试很难写．

好在，在做 Rails 开发时，极少需要写这种测试。

---

### 三、写冗余的测试代码, 不要 DRY.(个人习惯)

<small>

 一般不抽取公共代码到 before { ... } 或 setup { ... } 里面．
 
因为，多个测试用例用同一个 before, 不容易维护

而且, 常常会有冗余的测试数据，让测试变慢．
 
<p> 使用 VC 大法的时候，不方便.<span class="fragment"> -> Ctrl+V Ctrl+C</span><p>
 

 
---

### 四、写运行很快的测试

***没有人愿意等待测试的结果。***

---

引入并行测试。

Rails < 5 [parallel_test](https://github.com/grosser/parallel_tests) gem

Rails > 5 Rails 自己提供了。

    $: PARALLEL_WORKERS=15 bin/rails test

---

引入 preloader

Rails 自己已经有了 [spring](https://github.com/rails/spring) gem, 

还有另一个第三方替代 [zeus](https://github.com/burke/zeus) gem

---

测试数据库使用事务, 允许快速回滚, 来加速测试的运行.


<small>每一个测试用例在跑之前, 数据库是空的</small>

<small>Rails 自带测试, 基于 ActiveRecord, 默认就是事务</small>

<small>我们用的 Sequel, 不得不引入另一个 database_cleaner GEM 来做到这一点</small>

```rb
DatabaseCleaner.strategy = :transaction
```

RSpec: 加下面代码到 `spec/rails_helper.rb`

```rb
RSpec.configure do |config|
  config.use_transactional_fixtures = true
end
```

---

隔离对外部网络请求, 来加速测试运行。

我们测试在断网的情况下，可以成功运行。

---

[VCR](https://github.com/vcr/vcr) gem

Record your test suite's HTTP interactions

and replay them during future test runs for fast, deterministic, accurate tests. 

<small>
1. 如果开发流程依赖于外部 API 可以用 VCR 先录下来, 之后跑测试，response 总是返回录制时的那个结果。(可确定性, 返回精确地结果)
<!-- .element: class="fragment" -->
2. 隔离外部请求，加速测试, 有时候也省钱. (例如: 短信接口, 云存储上传图片等).
<!-- .element: class="fragment" -->
---

```rb
class Teacher < Sequel::Model(:teachers)
  REGIST_URL = '/v1/teacher/auths/regist'

  many_to_many :clazzs
  many_to_one :kindergarten

  # 在新建一个 teacher 之后, 会访问 auths 接口, 注册这个教师.
  def after_create
    conn = Faraday.new ENV['AUTH_API']
    conn.post(REGIST_URL, phone: phone, user_id: id)
  end
end
```

```rb
# 我们用一个叫做: teacher_registration 的磁带. 把 http 交互录下来.
VCR.use_cassette('teacher_registration') do
  # 创建 teacher 测试数据
  @teacher = create(:teacher)
end

```
<small>
磁带被录下来之后, 这段代码再次被运行时, 使用磁带中记录下来的响应和代码交互.
<!-- .element: class="fragment" -->

---

这是磁带的源码, 用 json 格式:

```js
{
  "http_interactions": [
    {
      "request": {
        "method": "post",
        "uri": "http://gw-dev.kid17.com:92/v1/teacher/auths/regist",
        "body": {
          "encoding": "UTF-8",
          "string": "phone=17778890867&user_id=65"
        },
        "headers": {
          "User-Agent": [
            "Faraday v0.14.0"
          ],
          "Content-Type": [
            "application/x-www-form-urlencoded"
          ],
          "Accept-Encoding": [
            "gzip;q=1.0,deflate;q=0.6,identity;q=0.3"
          ],
          "Accept": [
            "*/*"
          ]
        }
      },
      "response": {
        "status": {
          "code": 201,
          "message": "Created"
        },
        "headers": {
          "Content-Type": [
            "application/json"
          ],
          "Vary": [
            "Origin"
          ],
          "Content-Length": [
            "65"
          ]
        },
        "body": {
          "encoding": "UTF-8",
          "string": "{\"code\":0,\"message\":\"success\",\"data\":{\"user_id\":65,\"auth_id\":90}}"
        },
        "http_version": null
      },
      "recorded_at": "Sun, 06 May 2018 15:34:41 GMT"
    }
  ],
  "recorded_with": "VCR 4.0.0"
}
```


---

如果愿意的话, 你甚至可以手动编辑这个 json.

更深入了解相关概念, 可以尝试 Google 下 `契约测试`

---

### 五、取代 Postman, 或 GraphQL IDE, 高效 API 开发

它们能做的，通过编写测试, 都可以做到, 而且实现自动化, 添加断言.

例如: 用测试来验收线上的 API 接口。


---

### 六、调试后台/异步任务

上线后，才发现后台任务不正确?
<!-- .element: class="fragment" -->

<small>我们可以在开发阶段, 就通过测试来运行后台任务, 检查正确性.</small>
<!-- .element: class="fragment" -->


<p class="fragment"><small>[SideKiq](https://github.com/mperham/sidekiq), Simple, efficient background processing for Ruby</small></p>

```rb
expect(DelayedWorker.jobs.size).to eq 1
# Sidekiq 给我们提供了 drain 方法, 可以立即执行某个 worker.
DelayedWorker.drain
expect(DelayedWorker.jobs.size).to eq 0

# 验证 Worker 到底做了什么。
expect(pusher.executed_at.to_s(:sec)).to eq '2017-12-12 12:12:12' 
```
<!-- .element: class="fragment" -->

----

## 我们的项目如何开始 TDD ?

---

简单说下目前和立春暂定下来的测试目录结构:


```sh
 ╰─ $ tree -L 1 test
test
├── controllers
├── factories.rb
├── graph
├── models
└── test_helper.rb
```

<small>
test/controller, 是我们常说的集成测试, 每一个 entrypoint 都会有一个测试用例, 但是针对 graph 的测试,
我们放在单独的 test/graph 目录里.

test/models 使我们常说的单元测试, 针对 model 层业务逻辑编写测试.

test/factories.rb 是 FactoryBot 的定义

test/test_helper 是测试的配置文件, 所有测试都会 require 这个文件.

---

test/graph 下面的目录结构和 app/graph 下面的目录结构一一对应

app/__graph/queries/android/pages/main_page__.graphql

对应的测试文件是:

test/__graph/queries/android/pages/main_page__\_test.rb

---

开发的步骤: (暂定, 可能还会优化)

<small>
1. 参考页面, 先编写 graphql 文件, 确定 api 规范.
2. 添加测试文件, 让 TDD 流程先走起来, 且不管返回格式, 结果是否正确.
3. 如果需要的话, 设计表, 字段
4. 构建测试数据, 添加测试用例.
5. 编写业务逻辑.
6. 重复以上步骤, 直到满足业务逻辑, 并且返回的 json 和 graphQL 请求的一致.
7. 别忘了重构!
8. 实现代码越少越好, 测试代码, 多点也不打紧.

----

### 其他

---
<small>
正式上线后, 会引入针对线上 API 的一些测试.(需要一些测试用例)

单独的库, 如果抽取成 Gem 包, 会编写单独的测试.

为 线上 bug 编写(补)测试, 将来应该作为一个制度.

CI 里面, 将来要加入运行测试的代码: `bundle exec rake test`

---

## 谢谢！
