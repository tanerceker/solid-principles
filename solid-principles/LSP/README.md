<br/>

# Liskov Substitution Principle (LSP)

Barbara Liskov tarafından 1987 yılında bir konferans açılış konuşmasında tanıtılan **Liskov İkame İlkesi (Liskov Substitution Principle) (LSP)**, nesne yönelimli programlama ve tasarımın beş ilkesini temsil eden SOLID kısaltmasının üçüncü ilkesidir. İlkenin resmi ifadesi şöyledir:

"Eğer S, T'nin bir alt türü ise, bir programda T türündeki nesneler, o programın arzu edilen özelliklerinden herhangi birini değiştirmeden S türündeki nesnelerle değiştirilebilir."

Daha basit bir ifadeyle, türetilmiş sınıflar (derived classes) temel sınıflarıyla (base classes) yer değiştirebilmelidir. Yani, bir temel sınıf (base class) kullanan bir program, programın doğruluğunu etkilemeden bir alt sınıfla (subclass) yerini değiştirebilmelidir.

Bu ilke, tür (type) teorisinde, verilerin ve manipülasyonlarının matematiksel çalışmasında temel bir ilke olan davranışsal alt türlemeye (behavioral subtyping) dayanmaktadır. Bir alt türün (subtype), program doğruluğunu koruyarak her zaman üst türünün (supertype) yerine kullanılabilmesini sağlar.

<br/>

---

<br/>

## LSP Nasıl Uygulanır?

Bunu bir TypeScript örneği ile anlayalım. Bir Bird temel sınıfı (base class) düşünün:

<br/>

```tsx
class Bird {
  fly(): void {
    // ...
  }
}
```

<br/>

Şimdi, bir Penguen alt sınıfı (subclass) oluşturun:

```tsx
class Penguin extends Bird {
  fly(): void {
    throw new Error("Penguins can't fly!");
  }
}
```

<br/>
Bu, Liskov İkame İlkesini (Liskov Substitution Principle) ihlal eder çünkü potansiyel sorunlara neden olmadan Bird yerine Penguin'i koyamazsınız.
Bir fonksiyon Bird örneğini bekler ve fly yöntemini çağırır, ancak bunun yerine bir Penguin alırsa, bir hata meydana gelecektir.
<br/>
<br/>

LSP'ye uymak için kodu şu şekilde yeniden düzenleyebiliriz (refactor):

```tsx
class Bird {
  fly(): void {
    // ...
  }
}

class FlightlessBird extends Bird {
  fly(): void {
    // Belki bir mesaj günlüğe kaydedin veya hiçbir şey yapmayın
  }
}

class Penguin extends FlightlessBird {
  // ...
}
```

<br/>

Bu yeni kurulumda, bir FlightlessBird hala bir Bird'dür ve fly yöntemine (method) yanıt verebilir, ancak uçamadığı için harekete geçmez. Bu şekilde, bir Bird bekleyen herhangi bir fonksiyon bir Penguin veya FlightlessBird ile sorunsuz bir şekilde çalışabilir.

Sonuç olarak, Liskov İkame İlkesini (Liskov Substitution Principle) takip etmek, alt sınıfların (subclasses) yeniden kullanılabilirliğini ve değiştirilebilirliğini korumaya yardımcı olarak yazılım tasarımını daha sağlam (robust) ve bir alt türün (subtype) yanlış kullanımından kaynaklanan hatalara daha az eğilimli hale getirir.

<br/>

---

<br/>

## Gerçek Dünya Kullanım Örneği

Gerçek dünyadan bir örnek ele alalım: CreditCard, DebitCard ve PayPal gibi ödemeleri işlemek (process payments) için birden fazla yönteminizin olduğu bir çevrimiçi ödeme işleme sistemi.

İlk olarak, PaymentProcessor adında bir temel sınıf (base class) ve her ödeme şeklinin uygulaması (implement) gereken bir yöntem oluşturacağız:

```tsx
abstract class PaymentProcessor {
  abstract processPayment(amount: number): void;
}
```

<br/>

Şimdi, belirli ödeme yöntemi sınıflarını (payment method classes) oluşturuyoruz:

```tsx
class CreditCardProcessor extends PaymentProcessor {
  processPayment(amount: number): void {
    console.log(`Processing credit card payment of $${amount}`);
    // kredi kartı ödemelerinin işlenmesi için uygulama detayları
    // implementation details...
  }
}

class DebitCardProcessor extends PaymentProcessor {
  processPayment(amount: number): void {
    console.log(`Processing debit card payment of $${amount}`);
    // banka kartı ödemelerinin işlenmesi için uygulama detayları...
    // implementation details...
  }
}

class PayPalProcessor extends PaymentProcessor {
  processPayment(amount: number): void {
    console.log(`Processing PayPal payment of $${amount}`);
    // PayPal ödemesini işlemek için uygulama detayları...
    // implementation details...
  }
}
```

<br/>

Bu senaryoda, her ödeme işlemcisi sınıfı (payment processor class) PaymentProcessor'ın bir alt türüdür (subtype) ve her biri processPayment yöntemini (method) uygular (implement).

Şimdi, bir PaymentProcessor örneği (instance) alan ve bunu bir ödemeyi işlemek için kullanan bir fonksiyon oluşturalım. Liskov İkame İlkesi (Liskov Substitution Principle) (LSP) nedeniyle, bu fonksiyon PaymentProcessor'ın herhangi bir alt türünü (subtype) kullanabilir:

```tsx
function executePayment(paymentProcessor: PaymentProcessor, amount: number) {
  paymentProcessor.processPayment(amount);
}

// Şimdi, ödeme yöntemlerinden herhangi birini
// kullanarak ödemeleri işleyebiliriz:

const creditCardProcessor = new CreditCardProcessor();
executePayment(creditCardProcessor, 100);

const debitCardProcessor = new DebitCardProcessor();
executePayment(debitCardProcessor, 200);

const payPalProcessor = new PayPalProcessor();
executePayment(payPalProcessor, 300);
```

<br/>

Bu tasarım, herhangi bir PaymentProcessor alt türü (subtype) executePayment fonksiyonunda sorun yaratmadan kullanılabildiği için Liskov İkame İlkesine (Liskov Substitution Principle) uygundur. Her işlemcinin (processor) kendine özgü processPayment uygulaması (implementation) vardır, ancak bunların kullanım şeklinin belirli bir alt türe (specific subtype) göre değişmesi gerekmez.

---

— Taner Çeker tarafından hazırlanmıştır.
