## Projenin amacı

 [kickstarter.com](https://www.kickstarter.com/) gibi centralized bir uygulamayı peer to peer hale getirerek decentralized bir uygulama haline dönüştürmeyi amaçlamaktır.

## Yapılacaklar Listesi

- [x]  Kontratımız deploy edilirken kontrat sahibini, deploy eden adres olarak belirlemek
- [x]  Kontrat deploy edilirken projenin adını, açıklamasını ve minimum bağış miktarını belirlemek
- [x]  Kaç kişinin bağış yaptığını görebilmek
- [x]  Toplam bağış miktarını görebilmek
- [x]  Projeye bağış yapabilmek
- [x]  Verilen adresin bağış yapıp yapmadığını sorgulayabileceğimiz bir logic eklemek
- [x]  Harcama isteği oluşturmak
- [x]  Oluşturulan harcama isteğine, sadece bağışçıların oy verebilmesi için bir logic oluşturmak
- [x]  Yeni oluşturulacak isteği sadece kontratı deploy eden adresin oluşturabilmesini sağlayacak kontrol mekanizması eklemek
- [x]  Sadece bağış yapan kişilerin oy vermesini sağlayacak kontrol mekanizması eklemek
- [x]  Eğer onay sayısı toplam bağışçı sayısının yarısı veya daha fazlası ise bu harcama isteğini kabul edecek fonksiyonu oluşturmak

### Kaynak Kod

Projenin kaynak koduna [examples/Campaign.sol](examples/Campaign.sol) adresinden erişebilirsiniz.
#### **`Campaign.sol`**
```javascript
// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.0; // Hangi versiyonu kullandığımızı belirtiyoruz.

// Kontratımızı tanımlıyoruz
contract ProjectFunding{
        
    address public owner; // Projenin sahibinin adresini tutan değişken
    
    uint public minContribution; // Projenin minimum bağış miktarını tutan değişken
    string public name; // Projenin adını tutan değişken
    string public description; // Projenin açıklamasını tutan değişken
    uint public contribution = 0;     // Projenin toplam bağış miktarını tutan değişken
    uint public contributorCount = 0; // Projenin toplam bağış yapan yatırımcı sayısını tutan değişken
    mapping (address => bool) public contributor; // Projeye hangi adreslerin bağış yaptığını sorgulayabilmemiz için gerekli olan mapping    

    // Proje harcama isteklerinin bilgilerini tutacak olan struct
    struct Request{
        string description; // Harcama isteğinin açıklamasını tutan değişken
        uint value; // Harcama isteğinin miktarını tutan değişken
        address recipient; // Harcama isteğinin alıcısını tutan değişken
        bool completed; // Harcama isteğinin tamamlanıp tamamlanmadığını tutan değişken
        uint approversCount; // Harcama isteğinin onaylanma sayısını tutan değişken
        mapping(address => bool) approvers; // Harcama isteğini onaylayan ve onaylamayan yatırımcılarını tutan mapping
          
    }
    
    // Tüm harcama isteklerini tutacak olan array
    Request[] public requests;

    // Kontrat deploy edildiği anda çalışacak olan ve gerekli değerleri initialize edecek olan constructor
    constructor(uint _minContribution, string memory _name, string memory _description) {
        owner = msg.sender; // Proje sahibini kontratın kurucusu olarak atıyoruz
        name = _name; // Projenin adını atıyoruz
        description = _description; // Projenin açıklamasını atıyoruz
        minContribution = _minContribution; // Projenin minimum bağış miktarını atıyoruz
    }

    // Projenin sahibi olup olmadığını kontrol eden modifier  
    modifier onlyOwner(){
        require(owner == msg.sender,'You are not the owner!');
        _; // hangi fonksiyon çağırıyorsa o fonksiyondaki işleme devam et demektir.
    }
    
    // Projeye bağış yapılmasını sağlayacak olan fonksiyon
    function Contribute() public payable{
        require(msg.value >= minContribution,"You cannot send a value lover than minimum amount!");
                 
        // Toplam bağış miktarını arttırmak için contribution değişkenini kullanıyoruz 
        // ve her contribute fonksiyonu çağırıldığında yapılan bağış miktarını bir önceki değere ekliyoruz.
        contribution += msg.value;
        
        // Array kullanmak yerine, mapping kullanmak daha mantıklıdır.
        // Çünkü ilerde hangi bağışçının yatırım yapıp yapmadığını mapping ile daha kolay sorgulayabiliriz
        
        // Mappingde ilgili adresin value değerini true yapıyoruz. 
        // Bu sayede sadece addresi sorgulayarak bu adresin bağış yapıp yapmadığını söyleyebiliriz.
        // İlerleyen süreçte sadece bu mappingte true olan adreslerin harcama isteklerine onay vermesine izin verilecektir.
        contributor[msg.sender] = true;
      
        // TEğer aynı yatırımcı daha önce bağış yapmamışsa, toplam bağışçı sayısını 1 arttırıyoruz
        if (contributor[msg.sender] == true){
            contributorCount++;
        }
    }


    // Proje için gerekli olan harcama isteklerini açmak için kullanılacak olan fonksiyon
    // Bu fonksiyonu sadece kontratı deploy eden kişi çağırabilir. Bunu da onlyOwnner modifierı ile kontrol ediyoruz.
    function createRequest(string calldata _description, uint _value, address _recipient) public onlyOwner{
       // Yeni bir harcama isteği oluşturmak için, requests arrayının sonuna yeni bir eleman ekliyoruz.
       Request storage newReq = requests.push(); 

       // Yeni harcama isteği için gerekli olan değerleri initialize ediyoruz.
       newReq.description = _description; // Harcama isteğinin açıklamasını atıyoruz
       newReq.value = _value; // Harcama isteğinin miktarını atıyoruz
       newReq.recipient = _recipient; // Harcama isteğinin alıcısını atıyoruz
       newReq.completed = false; // Harcama isteğinin tamamlanıp tamamlanmadığını atıyoruz
    }  


    // Proje için gerekli olan harcama isteklerini onaylamak için kullanılacak olan fonksiyon
    function approveRequest (uint _index) public{
        // Bu fonksiyonu çağırabilmek için daha önce projeye bağış yapmış olmak gerekmektedir. Bunu da require modifier ile kontrol ediyoruz.
        require(contributor[msg.sender] == true,"You have to contribute first to approve a request");
        // Bu fonksiyonu çağırırken harcama isteği indexi gönderilmeli. Bu indexi kontrol ediyoruz.
        Request storage request = requests[_index];
        // İkinci require modifierı ise aynı bağışçının birden fazla onay vermesine izin verilmemesi için daha önce onaylama yapılıp yapılmadığını kontrol ediyor.
        require(request.approvers[msg.sender]==false,"Already approved");
        // Bu isteğe onay veren kullanıcının adresinin value değerini true yapıyoruz.
        request.approvers[msg.sender] = true;
        // Bu isteğe onay veren kullanıcı sayısını bir arttırıyoruz.
        request.approversCount ++;
    }

    // Proje için gerekli olan harcama isteklerini tamamlamak için kullanılacak olan fonksiyon
    // Bu fonksiyonu sadece kontratı deploy eden kişi çağırabilir. Bunu da onlyOwnner modifierı ile kontrol ediyoruz.
    function finalizeRequest(uint _index) public onlyOwner{
        // Bu fonksiyonu çağırırken harcama isteği indexi gönderilmeli. Bu indexi kontrol ediyoruz.
        Request storage finalRequest = requests[_index];
        // Bu isteğin tamamlanıp tamamlanmadığını kontrol ediyoruz.
        require(!finalRequest.completed,"Already completed");
        // Bu istek için gerekli olan onay sayısına ulaşılp ulaşılmadığını kontrol ediyoruz.
        require(finalRequest.approversCount > contributorCount/2,"Not enough approve");


        // Bu isteğin tamamlanması için gerekli olan değerleri initialize ediyoruz.
        finalRequest.completed = true;
        // Transfer fonksiyonu ile isteğin alıcısına harcama miktarını gönderiyoruz.
        payable(finalRequest.recipient).transfer(finalRequest.value);

    }

}
```