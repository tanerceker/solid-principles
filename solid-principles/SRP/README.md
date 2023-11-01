<br/>

# Single Responsibility Principle (SRP)

Bir sınıfın değişmesi için tek bir nedeni olmalıdır.
— Robert C. Martin (Uncle Bob)

Bu, bir sınıfın yalnızca bir sorumluluğu (responsibility) veya işi olması gerektiği anlamına gelir. Bir sınıfın birden fazla sorumluluğu varsa, değişmesi için birden fazla neden vardır ve bu da onu daha karmaşık (complex) ve sürdürülmesi zor hale getirebilir. Bu ilke, sınıflarda uyumu teşvik eder ve değişiklikler yapıldığında beklenmedik şekillerde bozulabilecek kırılgan kod olasılığını azaltır.

Bir sınıf birden fazla sorumluluğa (responsibility) sahipse, birbirine bağlı hale gelir ve bir sorumluluktaki değişiklik diğerlerini etkileyebilir. Bu bağlantı (coupling), kodun anlaşılmasını, değiştirilmesini ve sürdürülmesini daha zor hale getirebilir.

<br/>

---

<br/>

## SRP'nin Amacı

Amaç, değişikliği izole ederek etkisini en aza indirmektir. Bir şeyi değiştirmemiz gerekiyorsa, yalnızca bir sınıfı (class) güncellememiz gerekir. Bir sınıfın çok fazla sorumluluğu varsa, bir sorumluluğun gereksinimlerindeki (requirements) bir değişiklik sınıfın diğer sorumluluklarını etkileyebilir.

<br/>

---

<br/>

## TypeScript'te SRP

Typescript'te, diğer nesne yönelimli programlama dillerinde olduğu gibi, SRP büyük, karmaşık sınıfları daha küçük, daha yönetilebilir sınıflara bölerek uygulanabilir. Bu küçük sınıfların her biri tek bir sorumluluğa sahip olmalıdır.

Diyelim ki bir web uygulamasında bir User sınıfınız var ve bu sınıf hem kullanıcı veri yönetimini (kullanıcı bilgileri ve tercihleri gibi) hem de kullanıcı kimlik doğrulamasını gerçekleştiriyor. SRP'ye göre, bunlar farklı sorumluluk alanlarını temsil ettikleri için ayrı sınıflar olmalıdır. Kullanıcı verilerinin yönetilme şeklindeki olası bir değişiklik, kullanıcı kimlik doğrulamasının ele alınma şeklini etkilememelidir ve bunun tersi de geçerlidir.

<br/>

```tsx
class User {
  name: string;
  email: string;

  constructor(name: string, email: string) {
    this.name = name;
    this.email = email;
  }
}

class UserAuthentication {
  user: User;

  constructor(user: User) {
    this.user = user;
  }

  authenticate(password: string): boolean {
    // Kimlik doğrulama mantığını burada uygulayın
  }
}
```

<br/>

Yukarıdaki örnekte, kullanıcı bilgilerinin yönetilmesinden sorumlu olan User sınıfını, kullanıcının kimlik doğrulamasını gerçekleştiren UserAuthentication sınıfından ayırdık. Artık her sınıfın Tek Sorumluluk İlkesi (Single Responsibility Principle) (SRP) ile uyumlu tek bir değişiklik nedeni vardır.

<br/>

---

<br/>

## Gerçek Dünya Kullanım Örneği

Başlangıçta hem içerik yönetiminden (blog gönderileri oluşturma, güncelleme ve silme gibi) hem de gönderileri HTML olarak görüntülemekten sorumlu bir BlogPost sınıfına sahip olabileceğimiz bir blog uygulamasındaki gerçek dünya örneğini ele alalım. Bu sınıf aşağıdaki gibi görünebilir:

<br/>

```tsx
class BlogPost {
  title: string;
  content: string;

  constructor(title: string, content: string) {
    this.title = title;
    this.content = content;
  }

  // İçerik yönetimi ile ilgili yöntemler
  createPost() {}

  updatePost() {}

  deletePost() {}

  // Post gösterimiyle ilgili yöntem
  displayHTML() {
    return `<h1>${this.title}</h1><p>${this.content}</p>`;
  }
}
```

<br/>

Burada BlogPost sınıfının değişmesi için iki neden vardır.

— Birincisi, içerik yönetimi işlevselliği değiştiğinde,

— Diğeri ise gönderileri görüntülemek istediğimiz yol değiştiğinde.

Bu tasarım Tek Sorumluluk İlkesini (Single Responsibility Principle) (SRP) takip etmemektedir.

İçerik yönetimi ve görüntüleme sorumluluklarını (responsibilities) iki sınıfa ayırarak bunu SRP'yi takip edecek şekilde yeniden düzenleyebiliriz (refactor):

<br/>

```tsx
class BlogPost {
  title: string;
  content: string;

  constructor(title: string, content: string) {
    this.title = title;
    this.content = content;
  }

  // İçerik yönetimi ile ilgili yöntemler
  createPost() {}

  updatePost() {}

  deletePost() {}
}

class BlogPostDisplay {
  blogPost: BlogPost;

  constructor(blogPost: BlogPost) {
    this.blogPost = blogPost;
  }

  displayHTML() {
    return `
    <h1>${this.blogPost.title}</h1><p>${this.blogPost.content}</p>
    `;
  }
}
```

<br/>

Artık BlogPost sınıfı yalnızca içerik yönetiminden, BlogPostDisplay sınıfı ise gönderilerin görüntülenmesinden sorumludur. Bu sınıfların her birinin değişmesi için Tek Sorumluluk İlkesine (Single Responsibility Principle) (SRP) uygun tek bir neden vardır.

Bu ayrım, sürdürülebilirliği ve anlaşılabilirliği artırır. Yazıların görüntülenme şeklini değiştirmemiz gerekirse, yalnızca BlogPostDisplay sınıfını değiştirmemiz gerekir. Benzer şekilde, yazıların nasıl yönetildiğini değiştirmemiz gerekirse, yalnızca BlogPost sınıfını değiştirmemiz gerekir. Bu, bir işlevsellik alanındaki değişikliğin yanlışlıkla başka bir alanı etkileme olasılığını azaltır.

---

— Taner Çeker tarafından hazırlanmıştır.
