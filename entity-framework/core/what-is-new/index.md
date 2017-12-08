---
title: "EF çekirdek 2.0 - EF çekirdek yenilikler nelerdir?"
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
ms.technology: entity-framework-core
uid: core/what-is-new/index
ms.openlocfilehash: a0e54fe85e810793913b9c5fb5e745419a0983b2
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="dfbc3-102">EF çekirdek 2.0 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="dfbc3-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="dfbc3-103">.NET standart 2.0</span><span class="sxs-lookup"><span data-stu-id="dfbc3-103">.NET Standard 2.0</span></span>
<span data-ttu-id="dfbc3-104">EF çekirdek şimdi .NET Core 2.0, .NET Framework 4.6.1 ve .NET standart 2.0 uygulayan diğer kitaplıkları ile çalışabilirsiniz anlamı .NET standart 2.0, hedefler.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="dfbc3-105">Bkz: [desteklenen .NET uygulamalarında](../platforms/index.md) nelerin desteklendiği hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="dfbc3-106">Modelleme</span><span class="sxs-lookup"><span data-stu-id="dfbc3-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="dfbc3-107">Tablo bölme</span><span class="sxs-lookup"><span data-stu-id="dfbc3-107">Table splitting</span></span>

<span data-ttu-id="dfbc3-108">Artık, burada birincil anahtar sütunları paylaşılır ve her satır için iki veya daha fazla varlık karşılık gelen aynı tabloda iki veya daha fazla varlık türlerine eşlemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="dfbc3-109">Tablosunu kullanmak için (burada yabancı anahtar özelliklerini birincil anahtar oluşturmak) bir tanımlayıcı ilişkisi bölme tüm tablo paylaşımı varlık türleri yapılandırılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="dfbc3-110">Ait türleri</span><span class="sxs-lookup"><span data-stu-id="dfbc3-110">Owned types</span></span>

<span data-ttu-id="dfbc3-111">Başka bir ait varlık türü ile aynı CLR türüne ait varlık türü paylaşabilir, ancak yalnızca CLR türü tarafından tanımlanamıyor beri olmalıdır bir gezinti için başka bir varlık türü.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="dfbc3-112">Tanımlayan gezinmeyi içeren varlık sahibi değil.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="dfbc3-113">Sahibi sorgulanırken ait türleri varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="dfbc3-114">Kurala göre ait türü için gölge birincil bir anahtar oluşturulur ve onu sahibi olarak aynı tabloya tablo bölme kullanarak eşleşecektir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="dfbc3-115">Bunun yapılması sağlar nasıl karmaşık türler EF6 kullanılan ait kullanım benzer şekilde türleri:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

### <a name="model-level-query-filters"></a><span data-ttu-id="dfbc3-116">Model düzeyindeki sorgu filtreleri</span><span class="sxs-lookup"><span data-stu-id="dfbc3-116">Model-level query filters</span></span>

<span data-ttu-id="dfbc3-117">EF çekirdek 2.0 Model düzeyindeki sorgu filtreleri diyoruz yeni bir özellik içerir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-117">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="dfbc3-118">LINQ Sorgu koşulları (genellikle burada LINQ sorgu işleci için geçirilen Boole ifadesi) bu özelliği sağlar (genellikle içinde OnModelCreating) meta veri modelindeki varlık türlerinde doğrudan tanımlanmamış.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-118">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="dfbc3-119">Bu tür filtreleri, varlık türleri INCLUDE kullanımıyla dolaylı olarak gibi başvurulan veya doğrudan gezinti özelliği başvuruları dahil olmak üzere bu varlık türleriyle ilgili herhangi bir LINQ Sorgu otomatik olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-119">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="dfbc3-120">Bu özellik, bazı ortak uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-120">Some common applications of this feature are:</span></span>

- <span data-ttu-id="dfbc3-121">Yumuşak delete - IsDeleted özelliği bir varlık türlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-121">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="dfbc3-122">Çoklu kiracı - bir varlık türü bir Tenantıd özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-122">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="dfbc3-123">Aşağıda, yukarıda listelenen iki senaryo özelliğini gösteren basit bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-123">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="dfbc3-124">Çoklu kiracı ve soft-delete uygulayan bir model düzeyindeki filtre tanımlarız örneklerini ```Post``` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-124">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="dfbc3-125">DbContext örnek düzeyi özelliği kullanımına dikkat edin: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-125">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="dfbc3-126">Model düzeyindeki filtreleri doğru bağlamı örneğinden değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-126">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="dfbc3-127">Yani sorgu yürütülürken bir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-127">I.e. the one that is executing the query.</span></span>

<span data-ttu-id="dfbc3-128">Filtreler IgnoreQueryFilters() işlecini kullanarak tek tek LINQ sorguları için devre dışı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="dfbc3-129">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="dfbc3-129">Limitations</span></span>

- <span data-ttu-id="dfbc3-130">Gezinti başvurulara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-130">Navigation references are not allowed.</span></span> <span data-ttu-id="dfbc3-131">Bu özellik, geri bildirim göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="dfbc3-132">Filtreler yalnızca varlık türü bir hiyerarşinin kökünde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="dfbc3-133">Veritabanı skaler işlev eşleme</span><span class="sxs-lookup"><span data-stu-id="dfbc3-133">Database scalar function mapping</span></span>

<span data-ttu-id="dfbc3-134">Önizleme 2 içeren gelen önemli bir katkı [Paul Middleton](https://github.com/pmiddleton) yöntemine eşleme veritabanı skaler işlevler sağlayan böylece bunlar LINQ sorgularında kullanılır ve SQL çevrilen yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-134">Preview 2 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="dfbc3-135">Aşağıda, özellik nasıl kullanılabileceğini kısa bir açıklaması verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="dfbc3-136">Üzerinde bir statik yöntem bildirin, `DbContext` ve onunla açıklama `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="dfbc3-137">Bu gibi yöntemleri otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="dfbc3-138">Sorgularınızda bir yöntem kaydedildikten sonra herhangi bir yere kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-138">Once a method has been registered you can use it anywhere in your queries:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="dfbc3-139">Dikkat edilecek bazı noktalar:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-139">A few things to note:</span></span>

- <span data-ttu-id="dfbc3-140">Yöntemin adı bir işlev (Bu durumda kullanıcı tanımlı bir işlev) adı olarak kullanıldığında kurala göre SQL, ancak oluşturma adı ve şema yöntemi kaydı sırasında geçersiz kılabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="dfbc3-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="dfbc3-141">Şu anda yalnızca skaler işlevler desteklenir</span><span class="sxs-lookup"><span data-stu-id="dfbc3-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="dfbc3-142">Veritabanını, örneğin geçişler oluşturma ilgilenebilmek değil EF çekirdek eşlenen işlevi oluşturmanız gerekir</span><span class="sxs-lookup"><span data-stu-id="dfbc3-142">You must create the mapped function in the database, e.g. EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="dfbc3-143">Kod için kendi içinde bulunan tür yapılandırma ilk</span><span class="sxs-lookup"><span data-stu-id="dfbc3-143">Self-contained type configuration for code first</span></span>

<span data-ttu-id="dfbc3-144">İçinde EF6 türetme tarafından belirli bir varlık türünün kodu ilk yapılandırması kapsülleyen mümkün *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-144">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="dfbc3-145">EF çekirdek 2. 0'biz bu deseni geri getirme:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-145">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="dfbc3-146">Yüksek performans</span><span class="sxs-lookup"><span data-stu-id="dfbc3-146">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="dfbc3-147">DbContext havuzu</span><span class="sxs-lookup"><span data-stu-id="dfbc3-147">DbContext pooling</span></span>

<span data-ttu-id="dfbc3-148">Özel bir DbContext türü bağımlılık ekleme sistemine kaydetme ve daha sonra denetleyicileriyle Oluşturucu parametreleri üzerinden bu türdeki örneklerin alma EF çekirdeği ASP.NET Core uygulamada genellikle kullanmak için temel düzeni içerir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-148">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="dfbc3-149">Başka bir deyişle, DbContext yeni bir örneğini her istek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-149">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="dfbc3-150">Sürüm 2.0, saydam olarak yeniden kullanılabilir DbContext örneklerinin bir havuzu tanıtır bağımlılık ekleme özel DbContext türleri kaydetmek için yeni bir yol sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-150">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="dfbc3-151">DbContext havuzu kullanmak için ```AddDbContextPool``` yerine ```AddDbContext``` hizmet kaydı sırasında:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-151">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="dfbc3-152">Bu yöntem kullandıysanız, aynı anda biz öncelikle olup olmadığını örneği kullanılabilir havuzda kontrol eder denetleyicisi tarafından bir DbContext örneği istendi.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-152">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="dfbc3-153">İstek işleme sonlandırır sonra örneği üzerinde herhangi bir durumu sıfırlanır ve kendisini havuza geri döner örneğidir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-153">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="dfbc3-154">Nasıl bağlantı havuzu ADO.NET sağlayıcıları çalışır ve bazı DbContext örneğinin başlatma maliyetini kaydetme avantajı vardır, bu kavramsal olarak benzer.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-154">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="dfbc3-155">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="dfbc3-155">Limitations</span></span>

<span data-ttu-id="dfbc3-156">Bazı sınırlamalar ne yapılabilir iki yöntem sunar ```OnConfiguring()``` DbContext yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-156">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="dfbc3-157">İstekler genelinde Paylaşılmaması gereken, türetilmiş bir DbContext sınıfı içinde kendi durumunu (örneğin, özel alanları) korumak DbContext havuzu kullanarak kaçının.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-157">Avoid using DbContext Pooling if you maintain your own state (e.g. private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="dfbc3-158">EF çekirdek yalnızca DbContext örnek havuzuna eklemeden önce farkında durumuna sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-158">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="dfbc3-159">Açıkça derlenmiş sorguları</span><span class="sxs-lookup"><span data-stu-id="dfbc3-159">Explicitly compiled queries</span></span>

<span data-ttu-id="dfbc3-160">Büyük ölçekli senaryolarda yararlar için tasarlanmış ikinci katılımı performans özellikleri budur.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-160">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="dfbc3-161">API EF önceki sürümlerini hem de LINQ-SQL sorguları çevrilmesi yalnızca bir kez hesaplanabilir böylece önbelleğe almak uygulamaların izin vermek için kullanılabilir ve birçok kez yürütülmüş el ile veya açıkça derlenmiş sorgu.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-161">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="dfbc3-162">Karma ve önbellek araması hesaplama atlayarak küçük performans kazanç elde etmek için genel EF çekirdek otomatik olarak derleyin ve önbellek karma bir sorgu ifadeleri gösterimini üzerinde temel alan sorgular rağmen bu mekanizma kullanılabilir izin verme önceden derlenmiş bir sorgu çağırma temsilcisi ile kullanmak için uygulama.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-162">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="dfbc3-163">Değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="dfbc3-163">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="dfbc3-164">Attach yeni ve mevcut varlıkların bir grafik izleyebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="dfbc3-164">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="dfbc3-165">EF çekirdek çeşitli mekanizmalar anahtarı değerlerini otomatik olarak oluşturulmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-165">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="dfbc3-166">Anahtar özelliği CLR varsayılan--genellikle sıfır ya da null ise, bu özelliği kullanıldığında, bir değer oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-166">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="dfbc3-167">Bu varlıkların bir grafik için geçirilebilir anlamına gelir `DbContext.Attach` veya `DbSet.Attach` ve EF çekirdek zaten olarak ayarlanmış bir anahtara sahip bu varlıkların işaretleyecek `Unchanged` bir anahtar kümesi yok Bu varlıkların olarak işaretlenmiş ancak `Added`.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-167">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="dfbc3-168">Bu karma, yeni bir grafik eklemek kolaylaştırır ve anahtarlarını kullanırken, var olan varlıkları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-168">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="dfbc3-169">`DbContext.Update`ve `DbSet.Update` bir anahtar kümesi ile varlıklar olarak işaretlenmiş dışında aynı şekilde, iş `Modified` yerine `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-169">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="dfbc3-170">Sorgu</span><span class="sxs-lookup"><span data-stu-id="dfbc3-170">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="dfbc3-171">Geliştirilmiş LINQ çevirisi</span><span class="sxs-lookup"><span data-stu-id="dfbc3-171">Improved LINQ Translation</span></span>

<span data-ttu-id="dfbc3-172">Başarılı bir şekilde, veritabanı (yerine bellek içi) ve daha az gereksiz yere veritabanından alınan veri değerlendirilen daha fazla mantığı ile yürütmek daha fazla sorguları sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-172">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="dfbc3-173">GroupJoin geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="dfbc3-173">GroupJoin improvements</span></span>

<span data-ttu-id="dfbc3-174">Bu çalışma grubu birleştirmeler için oluşturulan SQL artırır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-174">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="dfbc3-175">Grup birleştirmeleri çoğunlukla isteğe bağlı Gezinti özellikleri üzerinde alt sorgularda sonucudur.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-175">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="dfbc3-176">FromSql ve ExecuteSqlCommand dize ilişkilendirme</span><span class="sxs-lookup"><span data-stu-id="dfbc3-176">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="dfbc3-177">C# 6 dize yapı dizeleri çalışma zamanında iyi bir yol sağlayarak ilişkilendirme, dize değişmez değerleri doğrudan katıştırılacak C# ifadeleri sağlayan bir özelliği kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-177">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="dfbc3-178">EF çekirdek 2.0 ara değerli dizeler için özel destek ham SQL dizeleri kabul bizim iki birincil API'leri için eklediğimiz: ```FromSql``` ve ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-178">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="dfbc3-179">Bu yeni Destek 'güvenli' bir biçimde kullanılacak C# ilişkilendirme dize verir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-179">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="dfbc3-180">Yani bir şekilde, SQL çalışma zamanında dinamik olarak oluşturma sırasında oluşabilecek yaygın SQL Yerleştirme hataları karşı korur.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-180">I.e. in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="dfbc3-181">Örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-181">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="dfbc3-182">Bu örnekte, iki değişken SQL biçim dizesine katıştırılmış vardır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-182">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="dfbc3-183">EF çekirdek aşağıdaki SQL üretir:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-183">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="dfbc3-184">EF. Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="dfbc3-184">EF.Functions.Like()</span></span>

<span data-ttu-id="dfbc3-185">EF ekledik. EF çekirdek veya sağlayıcıları tarafından veritabanı işlevleri veya işleçleri eşleme ve böylece bu LINQ sorgularında çağrılabilir yöntemlerini tanımlamak için kullanılabilir işlevler özelliği.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-185">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="dfbc3-186">Bu tür bir yöntem ilk örneği Like() verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-186">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

<span data-ttu-id="dfbc3-187">Bellek içi veritabanına karşı çalışırken veya koşul değerlendirme con istemci tarafı ortaya gerektiğinde faydalı olabilen Like() bir bellek içi uygulama ile geldiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-187">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur con the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="dfbc3-188">Veritabanı Yönetimi</span><span class="sxs-lookup"><span data-stu-id="dfbc3-188">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="dfbc3-189">DbContext İskele çoğullaştırma kanca</span><span class="sxs-lookup"><span data-stu-id="dfbc3-189">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="dfbc3-190">EF çekirdek 2.0 tanıtır yeni *IPluralizer* varlık tekil hale getirin için kullanılan hizmet adlarını yazın ve DbSet adlarını çoğul.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-190">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="dfbc3-191">Burada insanların kendi pluralizer kolayca ekleyebilirsiniz bir kanca bu nedenle varsayılan bir no-op, uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-191">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="dfbc3-192">İşte bu şekilde bir geliştirici, kendi pluralizer kanca için görünür:</span><span class="sxs-lookup"><span data-stu-id="dfbc3-192">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="dfbc3-193">Diğerleri</span><span class="sxs-lookup"><span data-stu-id="dfbc3-193">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="dfbc3-194">ADO.NET SQLite sağlayıcısına SQLitePCL.raw taşıma</span><span class="sxs-lookup"><span data-stu-id="dfbc3-194">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="dfbc3-195">Bu bize daha güçlü bir çözüm içinde Microsoft.Data.Sqlite farklı platformlarda yerel SQLite ikili dosyaları dağıtılmasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-195">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="dfbc3-196">Model başına yalnızca bir sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="dfbc3-196">Only one provider per model</span></span>
<span data-ttu-id="dfbc3-197">Önemli ölçüde sağlayıcıları modeli ile nasıl etkileşim kurabilirsiniz güçlendirir ve kuralları, ek açıklamalar ve fluent API'ları farklı sağlayıcıları ile nasıl basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-197">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="dfbc3-198">EF çekirdek 2.0 artık farklı bir oluşturacaksınız [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) kullanılan her farklı bir sağlayıcı için.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-198">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="dfbc3-199">Bu, genellikle uygulamanız için saydamdır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-199">This is usually transparent to the application.</span></span> <span data-ttu-id="dfbc3-200">Erişim herhangi bir şekilde bu alt düzey meta verilerin API'leri basitleştirme sayesinde kolaylaşır *ortak ilişkisel meta verileri kavramlar* her zaman bir çağrı aracılığıyla yapılan `.Relational` yerine `.SqlServer`, `.Sqlite`vb.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-200">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="dfbc3-201">Birleştirilmiş günlüğe kaydetme ve tanılama</span><span class="sxs-lookup"><span data-stu-id="dfbc3-201">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="dfbc3-202">(ILogger üzerinde göre) günlüğe kaydetme ve tanılama (üzerinde DiagnosticSource bağlı olarak) mekanizmaları artık daha fazla kod paylaşır.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-202">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="dfbc3-203">2. 0'bir ILogger gönderilen iletiler için olay kimlikleri değişti.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-203">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="dfbc3-204">Olay kimlikleri, şimdi EF çekirdek kodu arasında benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-204">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="dfbc3-205">Bu iletiler de artık yapılandırılmış günlük kullandığı, örneğin, MVC için standart yol izler.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-205">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="dfbc3-206">Günlükçü kategoriler de değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-206">Logger categories have also changed.</span></span> <span data-ttu-id="dfbc3-207">Kategoriler iyi bilinen bir dizi üzerinden erişilen artık yoktur [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="dfbc3-207">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="dfbc3-208">DiagnosticSource olaylarını şimdi ilgili olarak aynı olay kimliği adları kullanma `ILogger` iletileri.</span><span class="sxs-lookup"><span data-stu-id="dfbc3-208">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>