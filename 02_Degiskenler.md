# İçerik

Bu belgede, Solidity'deki değişken tanımlama ve veri tipleri anlatılacaktır.

## Karakter Dizileri: Strings, Bytes

Diğer programlama dillerinde bulunan [string](https://en.wikipedia.org/wiki/String_(computer_science))'dir. Bir karakter dizisini tutmak için kullanılır. Bytes kullanarak da belirli boyutlarda string değeri tutabiliriz.
```javascript
bytes32 name = "John"  // Değişkenin tutacağı değerin 32 byte'dan küçük olacağı kesin ise böyle bir kullanım olabilir.
string description = "The quick brown fox jumps over the lazy dog" // Uzun bir karakter dizisi olduğu için string ile tutmak daha mantıklıdır.
```
## Numerik Tipler: Uint, Int

Aradaki fark int tipi sıfırdan küçük değerler (negatif) de saklayabilirken, uint (*unsigned integer*) yalnızca pozitif değerler saklayabilir. Uint tipinde default değer uint256'dır. Yani 2<sup>256</sup> değere kadar saklayabilir. 

```javascript
uint8 normalInteger = 250;  // Değişkenin tutacağı değer 2 üzeri 8'den küçük olacağı kesin ise böyle bir kullanım olabilir.
uint bigInteger = 123456789101112131415; // Değişkenin tutacağı değerin büyük olma durumu varsa böyle tutmak daha mantıklıdır.
```

## Array

Aynı tip değişkenleri bir dizi içinde saklamak istersek kullanacağımız veri tipidir. 
```javascript
string[] words = ["Hello", "World"]; // String tipinde verilerin olduğu bir liste tutmak istediğimizde kullanacağımız veri tipidir.
```

## Mapping

Diğer programlama dillerinden Java'da [Hash Map](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html), Python'da [dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries), Javascript'te [objects](https://www.w3schools.com/js/js_objects.asp) olan veri tipidir. Basitçe, key - value şeklinde bir çift tutar. \
Tanımlanırken key ve value'nın tipleri belirtilmelidir.

```javascript
mapping(string => uint) scores;  // Bu satırda scores adında bir mapping değişkeni tanımladık. Bunun key değişkeninin string, value değerinin ise uint olacağını belirttik.

scores["Muhammet"] = 50; // Bu satırda da scores değişkeninin Muhammet key'i için value değerini 50 olarak atadık.
```

> **Not:** Mapping veri tipinde her key'in varsayılan (default) değeri vardır. Eğer **boolean** ise varsayılan değer **false**, **uint** ise **0**dır.


## Address

Solidity'de cüzdan adresi veri tipini tutmak için kullanılır.
```javascript
address senderAddress; // Burada ağda herkesin sahip olduğu cüzdanın adresi tutulacaktır. 
```


## Struct

Birden fazla veri tipini bir arada tutmak istiyorsak kullanacağımız veri tipidir. Diğer programlama dillerindeki struct ile aynıdır.

```javascript
struct Kisi {         // Kişi adında bir struct tanımladık
    string isim;      // Bu kişi yapısının (struct) string tipinde bir "isim" değişkeni tutacağımızı belirttik
    address adres;    // address tipinde bir "adres" değişkeni tutacağımızı belirttik
    uint yas;         // uint tipinde bir "yas" değişkeni tutacağımızı belirttik
}

Kisi yeniKisi; // Tanımlanan "Kisi" isimli struct'tan yeni bir instance yarattık
yeniKisi.isim = "Tohan";  // Bu instance'ın isim değişkeninin tuttuğu değeri değiştirdik.
yeniKisi.address = 0x000;
yeniKisi.yas = 24;
```


# Global Değişkenler

Global namespace'te bulunan ve blockchain hakkında çeşitli bilgileri bize sağlayan değişkenlerdir. Her kontratta kullanılabilir.  

### msg

Kontratla etkileşime giren cüzdanın bilgilerini getirir. En çok kullanılan özellikleri **msg.value** ve **msg.sender**'dır.
```javascript
address senderAddress = msg.sender;
uint sentAmount = msg.value;
```

### block

İlgili block hakkında bilgi getirir.
```javascript
block.difficulty; // Blockchain'de ilgili bloğun difficulty değerini getirir.
```

# Değişken Tutulacağı Yerler

Ethereum Virtual Machine'de verinin depolanacağı ***memory***, ***storage*** ve ***stack*** denilen 3 alan bulunur. Her cüzdan kendine özel verilerin saklanabileceği **storage** alanı barındırır. Eğitimde üzerinde durulan konular memory ve storage'dır.
## memory

Değişkenin hafızada tutulacağını ve bir yere depolanmayacağını belirtir. Her fonksiyon çağrıldığında değişecektir.
```javascript
string memory ornekMesaj; // Bu tanımlamada "ornekMesaj" isimli değişkenin memory'de saklanacağını belirttik
```

## storage

Değişkenin virtual machine üzerinde ilgili alanda depolanacağını belirtir.

```javascript
string storage ornekMesaj; // Bu tanımlamada "ornekMesaj" isimli değişkenin storage'da saklanacağını belirttik
```

# Erişim

Değişkene nereden erişilebileceğini belirler. **public** ise kontrat dışından da erişilebilirken, ***private** ise yalnızca kontrat içinden erişilir.

```javascript
string public mesaj = "Hello World"; // Bu tanımlamada "mesaj" isimli değişkenin herkese açık olacağını belirttik
uint private sayi = 123; // Bu tanımlamada "sayi" isimli değişkene yalnızca kontrat içinden erişilebileceğini belirttik
```