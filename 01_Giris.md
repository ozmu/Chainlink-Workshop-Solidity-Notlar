# İçerik

Bu belgede, solidity'ye dair derste işlenen genel bilgiler anlatılacaktır. Anlatılan kavramların özeti niteliğinde bir döküman olacaktır.

## Web3.0

Eğitime başlarken anlatılan ilk konu, Web2.0 ile Web3.0'ın kıyaslamasıydı. Web2.0'da bir uygulama backend servis olarak merkezi bir veya birden fazla sunucu ile etkileşime girerken, Web3.0'da bir blockchain ile etkileşime giriyor. Aradaki temel fark da budur. Web3.0'da bir blockchain ağıyla etkileşime girmek için yazdığımız programlara akıllı kontratlar diyoruz.

## Akıllı Kontratlar

Blockchain teknolojisinde sık kullanılan **akıllı kontrat** terimi, basitçe belirli şartlar sağlandığında gerçekleştirilen ve anonim taraflar (cüzdanlar aracılığı ile) güvenilirliği sağlayan bilgisayar programlarıdır. \
Diğer bir deyişle, blockchain'de bizim belirlediğimiz kurallara göre çalışacak olan koddur. En popüler ve büyük geliştirici kitlesine sahip olan akıllı kontrat dili [Solidity](docs.soliditylang.org)'dir. 


## Solidity

Solidity, [nesne tabanlı](https://en.wikipedia.org/wiki/Object-oriented_programming), [yüksek seviye](https://en.wikipedia.org/wiki/High-level_programming_language), [derlenen](https://en.wikipedia.org/wiki/Compiled_language) bir programlama dilidir.\
Solidity dosya uzantısı "**.sol**" dur.\
Solidity kodu'nun derleyicisi "**solc**" ve çalıştırma ortamı [Ethereum Virtual Machine](https://ethereum.org/en/developers/docs/evm/)'dir.

## Ethereum Virtual Machine ve ABI Kavramları

Bu konu teknik detay bir konu olduğu için pek üzerinde durulmamıştır. \
Kısaca yazılan Solidity kodları önce **.solc** ile derlenir ve ABI (Application Binary Interface) elde edilir. Daha sonra bu kod Ethereum Virtual Machine üzerinde çalıştırılır.
> **İlgililer için:** [What is an ABI and why is it needed to interact with contracts?](https://ethereum.stackexchange.com/a/235)

## Hardhat, Truffle

Bir akıllı kontrat geliştirirken ihtiyacımız olan ortamı bize sağlayan Runtime Environment'lardır.

## Remix IDE

Eğitim boyunca tüm örnekleri test ettiğimiz ortam [Remix IDE](https://remix.ethereum.org/)'dir. Browser tabanlı bir editor olan Remix, aynı zamanda bize yazdığımız kontratları test edip istediğimiz yere deploy alabileceğimiz bir ortam sunduğu için akıllı kontrat geliştiricilerinin çoğu tarafından kullanılmaktadır.


## Örnek Solidity Kodu

Remix IDE'de dosyalar kısmında ***contracts*** klasörü içinde yeni bir dosya oluşturalım ve adını ***Ornek.sol*** yapalım.

#### **`Ornek.sol`**
```javascript
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Ornek {

}
```

Kodun ilk satırı lisans belirteci ve ikinci satırı da hangi solidity compiler versiyonunu kullanacağımızı belirtir.\
Eğitimde 0.8.0 versiyonu kullanılmıştır.