# İçerik

Bu belgede, Solidity'deki fonksiyon tanımlama ve veri tipleri anlatılacaktır.


## ****Fonksiyon Tanımlama****


Solidity dilinde fonksiyon tanımı aşağıdaki gibidir:

```solidity
function approveRequest (uint _index, string memory _name) {
}
```

Yukarıda bulunan `approveRequest` fonksiyonu  uint ve string türünde 2 değişken alıyor. 


> ***Not:** Değişkenlerimizin isimleri görüldüğü üzere `_index` ve `_name` olarak tanımlanmıştır. Değişken isminde alt çizgi yani `_` olmasının nedeni, fonksiyon içerisinde tanımlanan değerler ile global tanımladığımız değişkenlerin isimlerinin birbirleri ile karışmaması içindir.*
>


Fonksiyonumuzda string değişkeni tanımlarken  `_name` değişkeninin `memory` 'de tutulması gerektiğini belirttik. Bunun nedeni arrayler, stringler, structlar ve mappingler gibi bütün referans tipleri için bu bilginin gerekli olması diyebiliriz. 

Referans tipi nedir diye soracak olursanız
Solidity'de argümanı göndermenin iki yolu vardır
- By value(değer ile): Argümanlar by value ile gönderildiğinde, Solidity compilerı parametrenin bir kopyasını oluşturur ve oluşturduğu bu kopyayı fonksiyona gönderir. Bu özellik sayesinde, ilk olarak belirlediğiniz değerinizde bir değişiklik olup olmadığını düşünmek zorunda kalmadan bu kopyanın değerini dilediğiniz gibi değiştirebilirsiniz.
- By reference(referans ile): Bu özellik ile fonksiyonunuz asıl değerinizin referansı ile çağırılır. Bu yüzden eğer fonksiyonunuz bu değişkenin değerini değiştirirse, asıl değişen değeriniz de değişmiş olur. 


Fonksiyonu çağırmak için :

```solidity
approveRequest(2,"SolidityDev");
```


## ****Private / Public Functions****


```solidity
function approveRequest (uint _index, string memory _name)  public{
}
```

Farkedildiği üzere fonksiyonumuzun görünürlüğünü `public` olarak belirttik. Bu sayede farklı contratlardan veya frontend servisleri üzerinden oluşturduğumuz `approveRequest` fonkiyonuna ulaşabiliriz.

Solidity'de fonksiyonların default görünülürlüğü public olarak belirlenmiştir. Bu demek oluyor ki herkes (veya herhangi bir kontrat) kontratınız içerisinde belirttiğiniz fonksiyonunuzu çağırabilir ve içerisinde yazdığınız kodu çalıştırabilir.

Tabii ki bu her zaman istenilen bir durum değildir ve oluşturduğunuz akıllı kontratınızı ataklara karşı korumasız hale getirebilir. Bu yüzden de fonksiyonlarınızı oluştururken `private`  olarak belirleyip sadece herkes tarafından erişilmesini istediğiniz fonksiyonları `public` olarak belirtmek güzel bir alışkanlık olacaktır.

Private fonksiyon tanımı: 

```solidity
uint[] numbers;

function _addNumbersToArray(uint _number) private {
  numbers.push(_number);
}
```

Yukarıda tanımladığımız `_addNumbersToArray` fonksiyonu `private` anahtar kelimesi sayesinde sadece bizim kontratımız tarafından çalıştırılabilir veya çağırılabilir durumda. Bu sayede de bizim kontratımız dışında hiç kimse veya hiçbir kontrat, `numbers` arrayine numara ekleme işlemi yapamaz.

Gördüğünüz üzere fonksiyon parametrelerinde olduğu gibi private fonksiyonlarının isimlerinin başına da alt çizgi (`_`) koyulması ortak görüş(convention) olarak benimsenmiştir.


### ****Return Değerleri****


Fonksiyonumuzdan bir değer döndürmek istiyorsak, aşağıdaki gibi belirtmemiz gerekmektedir:

```solidity
string greeting = "Merhaba!";

function sayHello() public returns (string memory) {
  return greeting;
}
```

Solidity'de fonksiyon tanımlarında, hangi değeri döndürmek istiyorsak, o değerin tipini de belirtmemiz gerekmektedir. (Yukarıdaki fonksiyon için konuşacak olursak return edecek değerin tipi `string`dir.)


## ****Handling Multiple Return Values****


Fonksiyonları tanımlarken birden fazla değer dönmemiz gereken durumlar ile karşılaşabiliriz. Bu gibi durumlarda yapabileceğimiz bazı seçenekler aşağıdaki gibidir:


# Fonksiyon tanımı: 


```solidity
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}
```


# Fonksiyonu çağırırken aşağıdaki gibi çağırmalıyız. 


```solidity
function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // Bu durumda eğer return edilen bütün değerleri kullanmak istiyorsak: 
  (a, b, c) = multipleReturns();
}
```

// Eğer sadece tek bir değeri kullanmak istiyorsak:

```solidity
function getLastReturnValue() external {
  uint c;
  // Diğer değerleri boş bırakabiliriz:
  (,,c) = multipleReturns();
}
```


### ****Internal ve External****


Fonksiyon görünürlüğünde belirttiğimiz `public` ve `private` anahtar kelimelerine ek olarak, Solidity fonksiyonlar için 2 farklı görünebilirlik tipine sahiptir: `internal` ve `external`.

`internal` ile `private` birbirinini aynısıdır, sadece internal anahtar kelimesi sayesinde oluşturduğumuz fonksiyon, bizim kontratımız ve bizim kontratımızdan inherit eden diğer kontratlar tarafından ulaşılabilir hale gelmiş olur. 

`external` ile de `public` birbirlerinin aynısıdır, fakat aradaki fark external olarak belirttiğimiz fonksiyonlar içinde bulunduğu kontrat tarafından veya aynı kontrat içindeki farklı fonksiyonlaran değil sadece kontrat dışından çağırılabilirler(Başka bir kontrat veya başka bir kişi).

Fonksiyonları `internal` veya `external` olarak belirtmek, `private` veya `public` olarak belirtmek ile aynı şekilde yazılır:

```solidity
contract Contributor {
  uint private contributorCount = 0;

  function contribute() internal {
    contributorCount++;
  }
}

contract NewContract is Contributor {
  uint private contribution = 0;

  function eatWithBacon() public returns (string memory) {
    contribution += msg.value;
    // Fonksiyonumuzu internal olarak belirttiğimiz için, farklı bir kontrattan(inherit edilmiş) çağırabiliriz.
    contribute();
  }
}
```



### ****Pure ve View****


```solidity
string greeting = "Merhaba!";

function sayHello() public returns (string memory) {
  return greeting;
}
```

Yukarıdaki fonksiyon herhangi bir durumu değiştirmiyor. Yani herhangi bir değeri değiştirmiyor veya herhangi bir değer yazmıyor.

Bu durumda, fonksiyonumuzu ***view*** fonksiyonu olarak belirtebiliriz, yani bu fonksiyon sadece veriyi okuyor, veride değişiklik yapmıyor demek:

```solidity
function sayHello() public view returns (string memory) {}
```

`view` dışındaki diğer anahtar kelimemiz ise ***pure***. Yani fonksiyon herhangi bir veriye erişim sağlamıyor demek oluyor:  

```solidity
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
```

Yukarıdaki fonksiyon uygulamanın durumunu bile okumuyor, sadece fonksiyona gönderilen parametreleri dönüyor. O zaman bu durumda fonksiyonumuzu ***pure*** olarak belirtebiliriz. 

> *Not: Fonksiyonları ne zaman pure/view olarak tanımlamamız gerektiğini hatırlamakta zorluk yaşayabiliriz. Şansılıyız ki Solidity derleyicisi bize hangi durumlarda pure hangi durumlarda view koymamız gerektiğini belirtmede çok iyi iş çıkarıyor.*
>


# **'View' Fonksiyonu ile gas fee'den tasarruf etmek**


## **View functions don't cost gas**


`view` fonksiyonları external(harici) bir kullanıcı tarafından çağırıldığında herhangi bir gas fee alınmaz.

Çünkü`view` fonksiyonları blockchain üzerinde herhangi bir değişiklik yapmazlar – sadece veriyi okurlar. Yani bir fonksiyonu `view` ile işaretlemek, `web3.js` e bu fonksiyonu çalıştırmak için sadece local Etherium makinemi sorgulamasının yeterli olduğunu söyler, ve aslında blockchain üzerinde bir transaction oluşturulmuyor(Eğer oluşturulsaydı herbir node üzerinde çalışması gerekirdi ve gas fee kesilirdi).

> Not: Eğer view fonksiyonu, aynı kontrat içinde bulunan ve view fonksiyonu olmayan bir fonksiyon tarafından çağırılırsa, o zaman gas fee alınır. Bunun nedeni view olmayan fonksiyon, ethereum ağında transaction oluşturur va her node tarafından verify edilmesi gerekir. 
Özetle  view fonksiyonlaro sadece external olarak çağırılırlarsa ücretsizdirler.
>


### ****Fonksiyon Değiştiriciler (Function Modifiers)****


```solidity
modifier onlyOwner(){
    require(owner == msg.sender,'You are not the owner!');
    _; // hangi fonksiyon çağırıyorsa o fonksiyondaki işleme devam et demek.
}
```
```solidity
// payable parametresi fonksiyonun para gönderilebilir olduğu anlamına gelir.
// fonksiyonda require varsa ve revert olursa gas fee iade edilir. assert kullanılırsa iade edilmez!
function addBalance() public payable onlyOwner{
    balance += msg.value;
}
```


## **Function modifiers with arguments**


Fonksiyon modifierları aynı zamanda argüman da alabilirler. Örnek olarak:

```solidity
// Kullanıcının yaşını tutmak için bir mapping

mapping (uint => uint) public age;

// Kullanıcının belirli bir yaşın üzerinde olduğunu kontrol eden modifier:
modifier olderThan(uint _age, uint _userId) {
  require(age[_userId] >= _age);
  _;
}

// Araba kullanabilmek için 18 yaşından büyük olmalı. 
// `olderThan` modifier'ını argüman kullanarak şu şekilde çağırabiliriz:

function driveCar(uint _userId) public olderThan(18, _userId) {
// Some function logic
}

```


## **Constructor fonksiyonu**


Constructor fonksiyonu, kontrat deploy edilirken, değişkenlere başlangıç değerleri vermek için kullanılır.
Aşağıdaki örnekte constructor, kontratın owner değişkeninini, kontratı deploy eden kişinin adresine eşitliyor. `addBalance` fonksiyonunda kullanılan `onlyOwner` modifierı sayesinde de eğer fonksiyonu çağıran kişi owner değil ise, `Bu kontratın sahibi değilsiniz!` uyarısı verir.
 
```solidity
contract ConstructorContractSample{
    address private owner;

    modifier onlyOwner(){
        require(owner == msg.sender,'Bu kontratın sahibi değilsiniz!');
        _; // hangi fonksiyon çağırıyorsa o fonksiyondaki işleme devam et demek.
    }

    constructor(){
        owner = msg.sender;
    }

    function addBalance() public payable onlyOwner{
        balance += msg.value;
    }
}
```


### ****Assert() ve Require()****


Solidityde bazı durumları kontrol etmemiz gerekebilir. Yukarıdaki kodda `onlyOwner` modifierı içinde bulunan require fonksiyonu sayesinde sadece belirli durumların sağlanması koşulu ile o fonksiyonun çalışabileceğini söylemiş oluyoruz aslında. 

`require()` ve `assert()` fonksiyonları bu durumları kontrol etmek ve gerektiğinde error döndürmek gibi önemli görevlere sahiptir. İki fonksiyon da neredeyse her açıdan birbirinin aynısıdır. Farklı oldukları tek konu gas fee ile alakalı olan kısımlarıdır. 

Solidity dilinde hata alındığı anda, işlem geri alınır(revert edilir).

- `require()` : Eğer hata alınırsa, revert işleminden sonra kullanıcının ödediği gas fee, kullanıcıya geri iade edilir. 
- `assert()` : Eğer hata alınırsa, revert edilse bile herhangi bir geri iade işlemi gerçekleşmez.

Peki hangisini ne zaman kullanmamız gerekiyor ?

Solidity dökümanının da açıkca belirttiği üzere;

- `assert()` fonksiyonu sadece değişmezleri(invariants) incelemek ve dahili sorunları test etmek için kullanılmalıdır.

- `require()` fonksiyonu, harici kontratlara yapılan çağrılardan return değerlerini kontrol etmek veya girdiler yada kontrat durum değişkenleri(state variables) gibi geçerli koşulların karşılandığını garanti etmek için kullanılmalıdır.
