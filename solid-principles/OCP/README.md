<br/>

# Open-Closed Principle (OCP)

Bu İlke, yazılım varlıklarının (sınıflar, modüller, fonksiyonlar, vb.) genişletmeye (extension) açık, ancak değiştirmeye (modification) kapalı olması gerektiğini belirtir.
— Robert C. Martin (Uncle Bob)

Bu, bir sisteme mevcut kodunu değiştirmeden yeni özellikler veya işlevsellik ekleyebilmeniz gerektiği anlamına gelir. Nesnenin davranışını genişletebilirsiniz, ancak kaynak kodunu değiştirmemelisiniz.
<br/>

#### Detaylı Açıklama:

- **Genişletmeye açık (Open for extension):** Bu, yazılım varlığının davranışının genişletilebileceği anlamına gelir, yani ona yeni özellikler veya davranışlar ekleyebilmeliyiz.
  <br/>
- **Değişikliğe kapalı (Closed for modification):** Bu, yazılım varlığı geliştirildikten ve gözden geçirilip test edildikten sonra koda dokunulmaması gerektiği anlamına gelir (yazılım davranışını doğrulamak için).

<br/>

OCP'nin arkasındaki fikir, kodunuzda daha fazla esnekliği teşvik etmesidir. Yeni davranış veya özellikler eklemeniz gerektiğinde, mevcut kodu değiştirmek (ve dolayısıyla muhtemelen yeni hatalar ortaya çıkarmak) yerine, eski kodu genişleten yeni bir kod yazarsınız.

<br/>

---

<br/>

## OCP Nasıl Uygulanır?

Şimdi bir TypeScript kod örneğine bakalım. Bir çevrimiçi alışveriş uygulaması oluşturduğunuzu ve bir indirim sistemi uygulamak istediğinizi varsayalım. İndirim, farklı müşteri türleri için farklı olacaktır. Başlangıçta, yalnızca iki tür müşterimiz var: "Regular" ve "Premium".
<br/>

OCP **olmadan**:

```tsx
class Discount {
  applyDiscount(customerType: string): number {
    if (customerType === "Regular") {
      return 10;
    } else if (customerType === "Premium") {
      return 20;
    }
  }
}
```

<br/>

Bu örnekte, yeni bir müşteri türü, diyelim ki farklı bir indirime sahip bir "Gold" müşteri tanıtmak istersek, Discount sınıfındaki giveDiscount yöntemini değiştirmemiz gerekecektir:

<br/>

```tsx
class Discount {
  applyDiscount(customer: Customer): number {
    if (customer.type === "Regular") {
      return 10;
    } else if (customer.type === "Premium") {
      return 20;
    } else if (customer.type === "Gold") {
      return 30;
    }
    return 0;
  }
}
```

<br/>

Bu, Açık-Kapalı İlkesini (Open-Closed Principle) ihlal ediyor çünkü yeni işlevselliği (yeni bir müşteri türü) yerleştirmek için mevcut kodu değiştiriyoruz. Açık-Kapalı İlkesine (OCP) göre, bu yeni işlevselliği mevcut kodu değiştirerek değil, yeni kod ekleyerek (genişleterek) ekleyebilmeliyiz.

<br/>

OCP **ile**:

```tsx
interface Customer {
  applyDiscount(): number;
}

class RegularCustomer implements Customer {
  applyDiscount(): number {
    return 10;
  }
}

class PremiumCustomer implements Customer {
  applyDiscount(): number {
    return 20;
  }
}

class GoldCustomer implements Customer {
  applyDiscount(): number {
    return 25;
  }
}

class Discount {
  applyDiscount(customer: Customer): number {
    return customer.applyDiscount();
  }
}
```

<br/>

Yukarıdaki örnekte, giveDiscount yöntemine sahip bir Customer arayüzü (interface) tanımladık. RegularCustomer ve PremiumCustomer sınıfları Customer arayüzünü (interface) uygular (impl).

Discount sınıfı, her yeni müşteri türü eklendiğinde değiştirilmesi gerekmediği için artık değişikliğe kapalı kalmaktadır. Customer arayüzünü (interface) uygulayan daha fazla sınıf oluşturarak kolayca daha fazla müşteri türü (customer type) ekleyebileceğimiz için onu genişletmeye açık hale getirdik.

<br/>

```tsx
class PlatinumCustomer implements Customer {
  applyDiscount(): number {
    return 30;
  }
}
```

<br/>

Bu, SOLID ilkelerindeki Açık-Kapalı İlkesinin (Open-Closed Principle) (OCP) TypeScript kod örneği ile kısa bir açıklamasıdır. Bu ilke, doğru şekilde uygulandığında kodu daha sağlam (robust), esnek (flexible) ve hatalara daha az eğilimli hale getirir.
