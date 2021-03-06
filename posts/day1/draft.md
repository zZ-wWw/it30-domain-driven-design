# 關於 Domain-driven Design

在我剛成為軟體工程師時，曾看過一份 [StackOverflow 2016 的調查](https://stackoverflow.blog/2016/10/07/do-developers-need-college-degrees/)，裡面提到 56% 的工程師並沒有 CS 相關的學歷，甚至高達 69 % 的工程師認為自己的專業部分是透過自學而來。

在這個產業，唯一不變的就是一直變化，學歷與經歷的保質期越來越短，這讓我不經思考這份工作的價值與未來到底在哪裡？

直到有天我翻到了一篇文章，裡面有一句話打動了我：

>  在這個時代，寫程式從來沒有那麼簡單過，但要解決的問題也空前的困難。

沒錯！我開始逐漸了解，軟體工程師的程式碼只是副產品，他真正的工作是「解決問題」。近年來火紅的 TDD 、重構、敏捷等等追本溯源，都是要提升我們解決問題的能力。其中框架只是工程師學習之路的第一步，甚至這篇 [To Think Like a Programmer](https://www.freecodecamp.org/news/how-to-think-like-a-programmer-lessons-in-problem-solving-d1d8bf1de7d2/)更犀利指出：

> The biggest mistake I see new programmers make is focusing on learning syntax instead of learning how to solve problems.
> — V. Anton Spraul

解決問題的重要性來自於現代企業越來越激烈的競爭環境 。尤其是服務業如金融、電商、社交軟體等等，商業決策千變萬化，軟體的開發是否能跟上決策就是一個非常重要的競爭力指標之一。

 大多時候，程式設計的重點並不在於用哪個框架、優化幾個百分比、技術領先業界，反而在於**是否能忠實表達業務的需求** 。畢竟引擎馬力再大、輪胎再耐用，音響再高級，沒辦法上路的車子一點用都沒有。

傳統的工程師只需要依照規格自己黑箱出能動的程式碼就好，就好像知道門把可以轉就好，但不知道門把是用來開門一樣。不過隨著日益複雜的業務需求 (比如變成自動門加上自動打卡)，照規格產出的老方法已經逐漸被淘汰...

因此 Eric Evans 發明了領域驅動設計 (Domain-Driven Design ，之後簡稱 DDD) ，這是一套專注在領域知識的思維以及設計模式。

這套模式可以來協助我們把業務邏輯寫進程式碼中，同時可以應對業務邏輯的維護與成長，並依此來決定技術的細節。

DDD 最大的價值之一就是把將商業領域的知識映照到程式碼中，解放「程式歸程式，業務歸業務」的傳統思維，在過程中甚至可以打破商業團隊與工程團隊間的藩籬，甚至到最後會感覺到：

> 開發其實是一場學習的過程，程式碼只是過程的副產物。

以及

> 程式即設計、設計即程式。

## DDD 是什麼

介紹 DDD 是什麼之前，我們先定義領域 (Domain) 是什麼。廣泛來說， domain (knowledge) 是指「一塊知識的範圍」。實務上，就是指「你工作上所需的一切知識集合」，包含「問題」以及「解決方案」。

DDD 是一個以**商業知識與語言**為程式設計準則以**解決複雜業務問題**的軟體開發方法論。

他有以下三個重點：

1. 跟領域專家 (domain expert) 密切且迭代地合作來定義出 domain 的範圍及相關解決的方案。
2. 切分 domain 出數個 subdomain (子領域) ，並專注在核心 subdomain 。
3. 透過一系列設計模式，將 domain 的知識濃縮進去程式模型 (model) 中。

現代軟體的複雜特性，沒有一個人或是團隊可以單獨掌握所有的知識細節，因此需要不斷地藉由與團隊間甚至與客戶的溝通來盡可能接觸到知識的全貌。而在溝通的過程中，我們將知識提煉出來，進而形成通用語言 (Ubiquitous Language) 。

在神話故事[巴別塔](https://zh.wikipedia.org/wiki/巴別塔)中，上帝懲罰自大的人類所以分化他們所說的語言，讓他們彼此無法溝通。但在應用 DDD 的過程，不同團隊藉由溝通形塑出 Ubiquitous Language ，讓彼此互相理解，然後開發正確的軟體，~~進而發大財~~ 。

![img](./ubiquitous-language.png)
(工程與商業交流後，發現其實需要的不過是訂單、商品與會員管理三個功能)

整個過程可以參考這張圖：
![img](./ddd-process.jpeg)
(source: [Patterns, Principles, and Practices of Domain-Driven Design ](https://www.amazon.com/Patterns-Principles-Practices-Domain-Driven-Design/dp/1118714709))

至於該怎麼做到有效溝通，會在之後的章節介紹 Bounded Context 、 Event Storming 以及 use case 搜集中提到。

## 如何使用 DDD

用一個簡單的例子讓大家快速理解 DDD 帶來的改變：格鬥類電玩。

相信大家就算沒玩過也看過別人打電動如快打旋風、小朋友齊打交之類的。記得小時候還要特別拿筆記本出來抄寫招式的組合按鍵如「上跳攻」、「上上下下左右左右 BA」等等。當時玩得開心到熬出幾個黑眼圈。

後來發現這跟寫程式有點像，不過這些繁瑣的指令放在程式中似乎就讓人笑不太出來...尤其是發現別人寫出「上上下下左右左右 BA」這種 code 時（如下)就代表要熬夜 debug 了...

```java
Role person  = new Person();
person.up();
person.up();

person.down();
person.down();

person.left();
person.left();

person.right();
person.right();

person.press('A');
person.press('B');
```

但當我們了解業務邏輯後，就可以將這段程式封裝成：

```java
Role person = new Person();
person.dragonFist();
```

一來業務邏輯清楚，二來避免了貧血模型的壞味道。重點是程式碼從組合碎片式的資訊轉而關注**程式的行為能力**。

但這時可能有人就會有疑問...

### 不就是個封裝，濫用了程式碼一樣複雜啊？

所以前面才提到要將業務語言注入程式之中，**只對重點業務行為進行封裝**。如此一來，程式碼就與業務邏輯綁在一起，不但不怕業務需求的變更與成長，也解決了程式設計師的[兩大難題](https://martinfowler.com/bliki/TwoHardThings.html)之一：命名。

因此 DDD 的核心精神之一就是：「知識的交流」，透過與商業團隊的交流，精準捕捉到每個使用案例的步驟，再藉由案例中使用的語言實踐到程式碼之中。一來增加程式可讀性，二來也避免了 YAGNI (You Aren't Gonna Need It)

大家可以嘗試看看用業務行為為程式碼命名。最常見就是將原先 CRUD 的貧血命名法更改為充滿業務含義的命名方式。

比如說一個 user 的 CRUD 可能就變成 `register`, `getProfile`, `getActivityHistory`, `updateProfile`, `changePassword`, `deactivateAccount`, `deleteAccount` 等等。

這種做法會在跨知識的情境下發揮更大的優勢，舉個咖啡廳的例子，假如一個正常流程包括點餐、製作、送餐，可以寫出以下程式碼：

```javascript
customer = new Customer('Bill');
order = order.create(customer, 'Coffee');
staff = new Staff(9527);
cashier = new Cashier();
staff.setCashier();
cashier.addMoney(order.total);
staff.setMoney(order.total);
order.setStaff(staff);

cup = new Cup();
staff.setCup();
cup.setFilterCone(new FilterCone());
cup.setCoffeeGround(new Coffee());
staff.brew(cup);
staff.wait();
staff.setFilterCone(null);

staff.setCoffeeTo(customer);
```

以上的程式碼可以明顯看出幾個問題：

- 所有屬性都可異動
- 業務意圖不明顯
- 一但需求更動，很難修改

經過修改後，程式碼如下：

```javascript
barista = new Barista(9527);
customer = new Customer('Bill');
order = new Order(customer, 'Coffee');
customer.placeOrder(order);
barista.processPayment(order);
barista.make(order);
barista.serveOrderTo(order, customer);
```

可以發現，移除掉那些沒有行為意義的 `getter` 與 `setter` 後，即使不會寫程式碼的人都可以看懂了。

> 使用 DDD 前，改 code 就像是在玩七龍珠，蒐集齊到四散各地的邏輯後才能完成理解怎麼修改。

### DDD 的進階模式

對於 DDD 有基本的概念後，接下來幾篇我將會帶各位進入 DDD 的幾項知名的設計模式，而大致上可以分為兩類：戰略與戰術。

- Strategic Design 戰略設計：利用 Use Case 捕捉業務模型拆分出 Subdomain ，並依此建立 Bounded Context 。
- Tactical Design 戰術設計：又稱 Model-Driven Design ，透過 Strategic Design 建立好的邊界與語言，透過一系列的 pattern 化為程式碼，保護業務核心的穩定性。

### Strategic Design 戰略設計

戰略設計目的在於協助我們建立 domain knowledge 並將其拆分成合理的區塊一一處理，同時也使我們理解軟體的核心價值在哪裡。戰略包括：

- 與領域專家互動建立 Domain 與解決方案。
- 將 Domain 切成若干 Subdomain 並找出 Core Subdomain 。
- 對應 Subdomain 來為解決方案做分類並建立邊界： Bounded Context (限界上下文，之後會詳談) 。
- Bounded Context 同時也有語言邊界的功能，所以可以在其中定義 Ubiquitous Language 。
- 定義不同 Bouund Context 之間的互動模式 Context Mapping 。

### Tactical Design 戰術設計

戰術設計幫助我們運用一些成熟的 design pattern 將 Bounded Context 轉為程式碼。有以下 Design Pattern 供使用：

- Entity
- Value Object
- Aggregate
- Repository
- Factory
- Domain Event
- Domain Service
- Application Service

### Benefits

1. 促進跨團隊的溝通
2. 更精準的捕捉 Use Case
3. 安全保護業務邏輯，不會因技術細節 (如 db 、框架、基礎設施)而影響
4. 開發時更靈活彈性、重用程式更方便，不會被繁複的業務邏輯卡住
5. 更好的模組化 = 更容易測試 (完美搭配 TDD)
6. 出現 Bug 時更快找到原因 (已經將關注點分離，查出哪邊出問題很快)
7. 少加班，發大財

不過也有以下缺點：

1. 不能幫助找到對的商業模式
2. 不適合建立 MVP 驗證商業模型
3. 不能快速建立產品
4. 需要豐富的領域知識
5. 學習成本高
6. 不適合高度科技專業的專案

## Common Questions

### 1. 什麼！？那不就代表我必須離開電腦先跟業務人員開一堆會嗎？

沒錯，乍聽之下增加了開發人員不少的負擔，但只要想到那一變再變或是永遠看不懂、做不完的 spec ，這些努力都會得到回報的！

### 2. 不過是多些封裝而已，還增加開發負擔？

DDD 一開始的學習成本並不便宜，

> 一開始開發雖然最重要，但只佔整個開發週期的一點點

### 3. 我只是個小/新創團隊

很多人會把 DDD 看似繁重的設計當成敏捷開發的絆腳石。老實說，若是團隊中沒有任何 DDD 經驗的人，這的確會是個艱難的開始。

此外，對於尚未找到穩定盈利商業模式的團隊來說，過度投資在技術的風險也太大，且團隊內並沒有 domain expert 可以提供方向。

可以參考這支影片:[Turn your startup in a stayup with DDD — Marijn Huizendveld](https://www.youtube.com/watch?v=E8QAa55tCtw)

或是也可以參考這張 DDD 適合度計分卡:

![socre board](./score-card.png)
(source: [2019-02-20-ddd taiwan-community-iddd-studygroup-1st](https://www.slideshare.net/kimKao/20190220ddd-taiwancommunityidddstudygroup1st))  
(origin post: http://www.informit.com/articles/article.aspx?p=1944876&seqNum=2)

不過也是有社群的朋友分享過即使是小專案，他也會利用 DDD 的部分 pattern 來快速建立模型，所以這部分其實見仁見智，對於 DDD 相當熟悉的人其實反而能在開發上更快速、正確的開發。

### 4. DDD 是萬能的嗎？

沒有什麼方法是銀子彈，只是各取所需罷了。 DDD 的強項在於解決複雜的業務邏輯以及拆分他們。因此搭上了 Microservice 的熱潮。很多人想引用 Microservice 卻不知如何正確拆分他們， DDD 此時提供了絕佳的切入點。

最後我必須強調， DDD 是一種設計方法，而設計本身沒有對錯，實作方法也有很多，不要只想著追求正確答案與完美設計，動手開始做才能做中學！

## Resources

- [An Introduction to Domain Driven Design and Its Benefits](https://dzone.com/articles/an-introduction-to-domain-driven-design-and-its-be)
- [Getting Started with Domain-Driven Design](http://www.informit.com/articles/article.aspx?p=1944876&seqNum=2)
- [2019-02-20-ddd taiwan-community-iddd-studygroup-1st](https://www.slideshare.net/kimKao/20190220ddd-taiwancommunityidddstudygroup1st)
- [Domain-driven Design -- What is it and how do you use it?](https://airbrake.io/blog/software-design/domain-driven-design)
