<br/>

# Dependency Inversion Principle (DIP)

Yüksek seviyeli modüller (High-level modules) düşük seviyeli modüllere
(low-level modules) bağımlı olmamalıdır. Her ikisi de soyutlamalara (abstractions) bağlı olmalıdır. Soyutlamalar detaylara bağlı olmamalıdır. Detaylar soyutlamalara (abstractions) bağlı olmalıdır.
— Robert C. Martin

Bağımlılık Ters Çevirme İlkesi (Dependency Inversion Principle) (DIP), yazılım tasarımlarını daha anlaşılır (understandable), esnek (flexible) ve sürdürülebilir (maintainable) hale getirmeyi amaçlayan beş tasarım ilkesinden oluşan SOLID kısaltmasının son ilkesidir.

Bağımlılık Ters Çevirme İlkesi'nin birebir tanımı aşağıda verilmiştir:

Bu ilke öncelikle kod modülleri arasındaki bağımlılıkların (dependencies) azaltılmasıyla ilgilidir, bu da daha ayrık (decoupled) ve bakımı kolay sistemlere yol açar.

Bunu biraz daha açalım:

#### **1. Yüksek seviyeli modüller (High-level modules) düşük seviyeli modüllere (low-level modules) bağımlı olmamalıdır. Her ikisi de soyutlamalara (abstractions) bağlı olmalıdır:**

Bu, yüksek seviyeli modüllerin (iş mantığını (business logic) veya kullanım durumlarını (use cases) uygulayan modüller) düşük seviyeli modüllere (bir veritabanına yazma veya HTTP isteklerini işleme (handling HTTP requests) gibi temel, düşük seviyeli işlevleri gerçekleştiren modüller) doğrudan bağlı olmaması veya bunlarla etkileşime girmemesi gerektiğini önermektedir. **Her ikisi de soyutlamalar (abstractions) (arayüzler (interfaces) veya soyut sınıflar (abstract classes) gibi) aracılığıyla etkileşime girmelidir.**

<br/>

#### **2. Soyutlamalar (Abstractions) detaylara bağlı olmamalıdır. Detaylar soyutlamalara bağlı olmalıdır:**

Bu, soyutlamanın (abstraction) altta yatan uygulama (implementation) hakkında bilgi sahibi olmadığı anlamına gelir. Soyutlama tarafından tanımlanan sözleşmeye (contract) uymak altta yatan detayın (yani arayüzü (interface) uygulayan (implement) sınıfların) sorumluluğundadır.

TypeScript'te aşağıdaki örneği düşünün:

<br/>

(DIP) **olmadan:**

```tsx
class MySQLDatabase {
  save(data: string): void {
    // MySQL veritabanına veri kaydetme mantığı
  }
}

class HighLevelModule {
  private database: MySQLDatabase;

  constructor() {
    this.database = new MySQLDatabase();
  }

  execute(data: string): void {
    // yüksek seviyeli mantık
    this.database.save(data);
  }
}
```

<br/>

Yukarıdaki örnekte, HighLevelModule yüksek seviyeli bir modüldür (high-level module) ve doğrudan düşük seviyeli (low-level) MySQLDatabase modülüne bağımlıdır. Bu, veritabanınızı MySQL'den MongoDB'ye değiştirmeye karar verirseniz, HighLevelModule'u değiştirmeniz gerekeceği anlamına gelir ki bu iyi değildir.

<br/>

(DIP) **ile:**

```tsx
interface IDatabase {
  save(data: string): void;
}

class MySQLDatabase implements IDatabase {
  save(data: string): void {
    // MySQL veritabanına veri kaydetme mantığı
  }
}

class MongoDBDatabase implements IDatabase {
  save(data: string): void {
    // verileri bir MongoDB veritabanına kaydetme mantığı
  }
}

class HighLevelModule {
  private database: IDatabase;

  constructor(database: IDatabase) {
    this.database = database;
  }

  execute(data: string): void {
    // yüksek seviyeli mantık
    this.database.save(data);
  }
}
```

<br/>

Bu örnekte, HighLevelModule artık IDatabase soyutlamasına (abstraction) bağlıdır. Altta yatan veritabanının MySQL ya da MongoDB olması umurunda değildir. Sadece veritabanı nesnesi üzerinde save yöntemini (method) çağırabileceğini biliyor. Bu tasarım, HighLevelModule'ü değiştirmek zorunda kalmadan veritabanını değiştirmemizi sağlar.
Bu, Bağımlılık Ters Çevirme İlkesi'nin (Dependency Inversion Principle) iş başında olmasıdır.

HighLevelModule'ü farklı veritabanı türleriyle nasıl örneklendirebileceğinizi aşağıda bulabilirsiniz.

```tsx
// HighLevelModule'ü bir MySQL veritabanı ile örnekleyin.
let mySQLDatabase: IDatabase = new MySQLDatabase();
let highLevelModule1: HighLevelModule = new HighLevelModule(mySQLDatabase);

// Şimdi modülü bazı yüksek seviye fonksiyonları çalıştırmak için kullanın.
highLevelModule1.execute("Some Data for MySQL");

// HighLevelModule'ü bir MongoDB veritabanı ile örnekleyin.
let mongoDBDatabase: IDatabase = new MongoDBDatabase();
let highLevelModule2: HighLevelModule = new HighLevelModule(mongoDBDatabase);

// Şimdi modülü bazı yüksek seviye fonksiyonları çalıştırmak için kullanın.
highLevelModule2.execute("Some Data for MongoDB");
```

<br/>

Yukarıdaki örnekte, HighLevelModule kodunu değiştirmeden veritabanı bağımlılığını (database dependency) nasıl değiştirebileceğimizi görebilirsiniz. Önce bir MySQLDatabase, daha sonra da bir MongoDBDatabase kullanıyoruz. Bu, Bağımlılık Ters Çevirme İlkesinin (Dependency Inversion Principle) (DIP) sağladığı güç ve esnekliğin mükemmel bir örneğidir.

---

— Taner Çeker tarafından hazırlanmıştır.
