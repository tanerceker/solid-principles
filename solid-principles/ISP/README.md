<br/>

# Interface Segregation Principle (ISP)

Hiçbir istemci kullanmadığı arayüzlere bağımlı olmaya zorlanmamalıdır.
— Robert C. Martin

Bu ilke, büyük, karmaşık arayüzleri (interfaces) daha küçük, daha spesifik arayüzlere bölerek gerekli değişikliklerin yan etkilerini (side effects) ve sıklığını azaltmakla ilgilidir. Bir arayüzün (interface) belirli yöntem kümelerine ayrılması, istemcinin yalnızca kendisini ilgilendiren yöntemler (methods) hakkında bilgi sahibi olmasını sağlayacaktır.

Daha basit bir ifadeyle, yeni yöntemler (methods) ekleyerek mevcut bir arayüze (interface) ek işlevsellik eklemeyin. Bunun yerine yeni bir arayüz (interface) oluşturun.

<br/>

---

<br/>

## ISP Nasıl Uygulanır?

Diyelim ki print, scan ve fax için yöntemleri olan bir Machine arayüzümüz (interface) var.

<br/>

```tsx
interface Machine {
  print(document: Document): void;
  scan(document: Document): void;
  fax(document: Document): void;
}

class MultiFunctionPrinter implements Machine {
  print(document: Document): void {
    // actual implementation
  }

  scan(document: Document): void {
    // actual implementation
  }

  fax(document: Document): void {
    // actual implementation
  }
}
```

<br/>

Peki ya sadece yazdırmayı (print) destekleyen basit bir yazıcı tanıtmak isterseniz? Yukarıdaki senaryoda, ihtiyacınız olmamasına rağmen tarama (scan) ve faks (fax) yöntemlerini (methods) de uygulamak (implement) zorunda kalırsınız. Bu açıkça Arayüz Ayrımı İlkesinin (Interface Segregation Principle) (ISP) ihlalidir.

<br/>

Daha iyi bir yaklaşım, Machine'i aşağıdaki gibi daha spesifik arayüzlere (interfaces) ayırmaktır:

```tsx
interface Printer {
  print(document: Document): void;
}

interface Scanner {
  scan(document: Document): void;
}

interface FaxMachine {
  fax(document: Document): void;
}

class SimplePrinter implements Printer {
  print(document: Document): void {
    // ...
  }
}

class MultiFunctionMachine implements Printer, Scanner, FaxMachine {
  print(document: Document): void {
    // ...
  }

  scan(document: Document): void {
    // ...
  }

  fax(document: Document): void {
    // ...
  }
}
```

<br/>

Bu şekilde, bir SimplePrinter kullanmadığı tarama (scan) veya faks (fax) yöntemlerine (methods) bağımlı olmaya zorlanmaz. Bu, Arayüz Ayrımı İlkesinin (Interface Segregation Principle) (ISP) en önemli noktasıdır.

<br/>

---

<br/>

## Gerçek Dünya Kullanım Örneği

Blog gibi dijital bir içerik platformu için gönderi oluşturabileceğiniz, gönderilere yorum yapabileceğiniz ve gönderileri paylaşabileceğiniz bir web hizmeti API'ının gerçek dünyadaki bir örneğini ele alalım.

İlk olarak, bir istemcinin gerçekleştirebileceği tüm olası eylemleri tanımlayan tek bir arayüzle (interface) başlayabiliriz:

```tsx
interface BlogService {
  createPost(post: Post): void;
  commentOnPost(comment: Comment): void;
  sharePost(post: Post): void;
}
```

<br/>

Ancak, uygulama büyüdükçe ve farklı türde kullanıcılarla ilgilenmeye başladıkça, bu arayüzün (interface) tüm istemciler için uygun olmadığını görebiliriz. Örneğin, yalnızca salt okunur (read-only) izinlere sahip bir kullanıcı gönderi oluşturamamalıdır. Benzer şekilde, gönderi oluşturabilen bir kullanıcı bunları paylaşamayabilir.

Arayüz Ayrımı İlkesine (Interface Segregation Principle) (ISP) uymak için, bu BlogService arayüzünü (interface) ayrı, daha spesifik arayüzlere (interfaces) ayırmalıyız:

```tsx
interface PostCreator {
  createPost(post: Post): void;
}

interface CommentCreator {
  commentOnPost(comment: Comment): void;
}

interface PostSharer {
  sharePost(post: Post): void;
}
```

<br/>

Şimdi, farklı sınıfların (classes) gerçek yeteneklerine göre bu arayüzleri (interfaces) uygulamalarını (implement) sağlayabiliriz:

```tsx
class Admin implements PostCreator, CommentCreator, PostSharer {
  createPost(post: Post): void {
    // ...
  }

  commentOnPost(comment: Comment): void {
    // ...
  }

  sharePost(post: Post): void {
    // ...
  }
}

class RegularUser implements CommentCreator, PostSharer {
  commentOnPost(comment: Comment): void {
    // ...
  }

  sharePost(post: Post): void {
    // ...
  }
}

class ReadOnlyUser {
  // Bu eylemlerden hiçbirini gerçekleştiremedikleri için
  // arayüzlerden hiçbirini uygulamaz.
}
```

<br/>

Artık her istemci yalnızca gerçekte kullandıkları arayüzlere (interfaces) bağımlı. Bir Admin tüm işlevlere erişebilirken, bir RegularUser sadece yorum yapabilir ve gönderi paylaşabilir. ReadOnlyUser bu yeteneklere sahip olmadığı için bu arayüzlerin hiçbirine bağımlı değildir.
Bu, Arayüz Ayrımı İlkesinin (Interface Segregation Principle) (ISP) gerçek dünya senaryosundaki bir uygulamasıdır (application).

Bu tasarım, hangi rollerin hangi yeteneklere sahip olduğunu açıkça (clearly) belirtme avantajına sahiptir ve kodu gelecekteki değişikliklere daha uyarlanabilir hale getirir. Örneğin, yalnızca Admin kullanıcılarının gerçekleştirebileceği yeni bir eylem eklemek istersek, bu eylem için yeni bir arayüz (interface) oluşturabilir ve diğer sınıfların hiçbirini etkilemeden Admin sınıfının bunu uygulamasını (implement) sağlayabiliriz.

---

— Taner Çeker tarafından hazırlanmıştır.
