# İçerik

Bu belgede, Solidity'deki değişken tanımlama ve veri tipleri anlatılacaktır.

## Karakter Dizileri: Strings, Bytes

Diğer programlama dillerinde bulunan [string](https://en.wikipedia.org/wiki/String_(computer_science))'dir. Bir karakter dizisini tutmak için kullanılır. Bytes kullanarak da belirli boyutlarda string değeri tutabiliriz.
```javascript
bytes32 name = "John"  // Değişkenin tutacağı değerin 32 byte'dan küçük olacağı kesin ise böyle bir kullanım olabilir.
string description = "The quick brown fox jumps over the lazy dog" // Uzun bir karakter dizisi olduğu için string ile tutmak daha mantıklıdır.
```

# Strings
### Bytes32
### Bytes2
(memory-storage konusu)


## Numerik Tipler: Uint, Int

Aradaki fark int tipi sıfırdan küçük değerler (negatif) de saklayabilirken, uint (*unsigned integer*) yalnızca pozitif değerler saklayabilir. Uint tipinde default değer uint256'dır. Yani 2<sup>256</sup> değere kadar saklayabilir. 

```javascript
uint8 normalInteger = 250  // Değişkenin tutacağı değer 2 üzeri 8'den küçük olacağı kesin ise böyle bir kullanım olabilir.
uint bigInteger = 123456789101112131415 // Değişkenin tutacağı değerin büyük olma durumu varsa böyle tutmak daha mantıklıdır.
```

## Mapping

Diğer programlama dillerinden Java'da [Hash Map](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html), Python'da [dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries), Javascript'te [objects](https://www.w3schools.com/js/js_objects.asp) olan veri tipidir. Basitçe, key - value şeklinde bir çift tutar. \
Tanımlanırken key ve value'nın tipleri belirtilmelidir.

```javascript
mapping(string => uint) scores;  // Bu satırda scores adında bir mapping değişkeni tanımladık. Bunun key değişkeninin string, value değerinin ise uint olacağını belirttik.

scores["Muhammet"] = 50; // Bu satırda da scores değişkeninin Muhammet key'i için value değerini 50 olarak atadık.
```

> **Not:** Mapping veri tipinde her key'in varsayılan (default) değeri vardır. Eğer **boolean** ise varsayılan değer **false**, **uint** ise **0**dır.


# Address

# Global Variables
### msg
### block