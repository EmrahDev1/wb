### Amaç

WBV projelerinin tamamında ortak ve merkezi bir servis altyapısı sağlamak amacıyla, `wbv.api` domaini altında tekil bir API mimarisi oluşturulmuştur. Bu yapı, mevcut ve gelecekteki tüm proje ihtiyaçlarını karşılayacak şekilde genişletilebilir ve sürdürülebilir olarak tasarlanmıştır. (Domain değiştirilebilir.)

#### Kimlik Doğrulama

Tüm projelerdeki kimlik doğrulama işlemleri merkezi olarak aşağıdaki endpoint'ler üzerinden gerçekleştirilir:

- `wbv.api/auth/login`
- `wbv.api/auth/logout`
- ...

Kullanıcılar, kimlik doğrulama sonrasında edindikleri token ile sahip oldukları yetkiler doğrultusunda tüm servis endpoint’lerine erişim sağlayabilirler.

#### Örnek Servis Endpoint’leri

- `wbv.api/users`
- `wbv.api/roles`
- `wbv.api/permissions`
- `wbv.api/bonuses`  
  _Örneğin: Bir kullanıcının herhangi bir siteye ait bonus bilgilerine erişmesi gerektiğinde bu endpoint kullanılır._
- `wbv.api/affs`  
  _Affiliate kullanıcıların erişimine açık yapıdır:_
  - `wbv.api/affs/auth/login`
  - `wbv.api/affs/auth/logout`
- `wbv.api/deposits`  
  _Yatırım verilerine erişim bu endpoint üzerinden sağlanır._
- `wbv.api/withdraws`
- ...

#### Esnek ve Parametrik Veri Erişimi

Servis yapısı, her sayfa veya veri türü için ayrı endpoint tanımlamaya ihtiyaç duymadan, filtreleme parametreleriyle özelleştirilmiş veri sorgularına izin verir. Böylece sade, okunabilir ve esnek bir API mimarisi elde edilmiştir.

**Örnek:**  
Belirli bir sitede, "pending" durumundaki ve belirli bir tutara sahip yatırımları listelemek için yalnızca doğru filtre parametrelerini aşağıdaki gibi göndermek yeterlidir:

```http
GET /deposits?filter[site]=1&filter[status]=pending&filter[amount]=100
```

Bu yapıda, filtreler URL üzerinde filter[...] şeklinde iletilir ve gerektiğinde yeni filtre türleri kolayca eklenebilir. Böylece her farklı ihtiyaç için yeni endpoint tasarlamaya gerek kalmaz; mevcut yapı üzerinde sadeleştirilmiş geliştirmeler yapılabilir.





### Mimari
- Domain Driven Design Architecture

### Kullanım
```sh
  php artisan make:domain {domain} {model},{model}...
```

### Örnek

```sh
  php artisan make:domain Product Product,Category,Brand,Comment,Favorite
```

### Çıktı

```text
root/src
└── Domain
   └── Product <- ** Command çalıştırılırken girilen domain adı **
      ├── Contracts
      |  ├── Repositories
      |  |  ├── BrandRepositoryInterface.php
      |  |  ├── CategoryRepositoryInterface.php
      |  |  ├── CommentRepositoryInterface.php
      |  |  ├── FavoriteRepositoryInterface.php
      |  |  └── ProductRepositoryInterface.php
      |  └── Services
      |     ├── BrandServiceInterface.php
      |     ├── CategoryServiceInterface.php
      |     ├── CommentServiceInterface.php
      |     ├── FavoriteServiceInterface.php
      |     └── ProductServiceInterface.php
      ├── DataTransferObjects
      |  ├── BrandData.php
      |  ├── CategoryData.php
      |  ├── CommentData.php
      |  ├── FavoriteData.php
      |  └── ProductData.php
      ├── Http
      |  ├── Controllers
      |  |  ├── BrandController.php
      |  |  ├── CategoryController.php
      |  |  ├── CommentController.php
      |  |  ├── FavoriteController.php
      |  |  └── ProductController.php
      |  ├── Queries
      |  |  ├── BrandQuery.php
      |  |  ├── CategoryQuery.php
      |  |  ├── CommentQuery.php
      |  |  ├── FavoriteQuery.php
      |  |  └── ProductQuery.php
      |  ├── Requests
      |  |  ├── StoreBrandRequest.php
      |  |  ├── StoreCategoryRequest.php
      |  |  ├── StoreCommentRequest.php
      |  |  ├── StoreFavoriteRequest.php
      |  |  ├── StoreProductRequest.php
      |  |  ├── UpdateBrandRequest.php
      |  |  ├── UpdateCategoryRequest.php
      |  |  ├── UpdateCommentRequest.php
      |  |  ├── UpdateFavoriteRequest.php
      |  |  └── UpdateProductRequest.php
      |  ├── Resources
      |  |  ├── BrandResource.php
      |  |  ├── CategoryResource.php
      |  |  ├── CommentResource.php
      |  |  ├── FavoriteResource.php
      |  |  └── ProductResource.php
      |  └── Routes
      |     └── product.php
      ├── Models
      |  ├── Brand.php
      |  ├── Category.php
      |  ├── Comment.php
      |  ├── Favorite.php
      |  └── Product.php
      ├── Providers
      |  └── ProductServiceProvider.php
      ├── Repositories
      |  ├── BrandRepository.php
      |  ├── Caches
      |  |  ├── BrandCacheDecorator.php
      |  |  ├── CategoryCacheDecorator.php
      |  |  ├── CommentCacheDecorator.php
      |  |  ├── FavoriteCacheDecorator.php
      |  |  └── ProductCacheDecorator.php
      |  ├── CategoryRepository.php
      |  ├── CommentRepository.php
      |  ├── FavoriteRepository.php
      |  └── ProductRepository.php
      └── Services
         ├── BrandService.php
         ├── CategoryService.php
         ├── CommentService.php
         ├── FavoriteService.php
         └── ProductService.php
```

### Oluşturulan endpoint'ler

```text
GET|HEAD        product/brands ............................................................................................................ brands.index › Domain\Product\Http\Controllers\BrandController@index
POST            product/brands ............................................................................................................ brands.store › Domain\Product\Http\Controllers\BrandController@store
GET|HEAD        product/brands/{brand} ...................................................................................................... brands.show › Domain\Product\Http\Controllers\BrandController@show
PUT|PATCH       product/brands/{brand} .................................................................................................. brands.update › Domain\Product\Http\Controllers\BrandController@update
DELETE          product/brands/{brand} ................................................................................................ brands.destroy › Domain\Product\Http\Controllers\BrandController@destroy
GET|HEAD        product/categories ................................................................................................. categories.index › Domain\Product\Http\Controllers\CategoryController@index
POST            product/categories ................................................................................................. categories.store › Domain\Product\Http\Controllers\CategoryController@store
GET|HEAD        product/categories/{category} ........................................................................................ categories.show › Domain\Product\Http\Controllers\CategoryController@show
PUT|PATCH       product/categories/{category} .................................................................................... categories.update › Domain\Product\Http\Controllers\CategoryController@update
DELETE          product/categories/{category} .................................................................................. categories.destroy › Domain\Product\Http\Controllers\CategoryController@destroy
GET|HEAD        product/comments ...................................................................................................... comments.index › Domain\Product\Http\Controllers\CommentController@index
POST            product/comments ...................................................................................................... comments.store › Domain\Product\Http\Controllers\CommentController@store
GET|HEAD        product/comments/{comment} .............................................................................................. comments.show › Domain\Product\Http\Controllers\CommentController@show
PUT|PATCH       product/comments/{comment} .......................................................................................... comments.update › Domain\Product\Http\Controllers\CommentController@update
DELETE          product/comments/{comment} ........................................................................................ comments.destroy › Domain\Product\Http\Controllers\CommentController@destroy
GET|HEAD        product/favorites ................................................................................................... favorites.index › Domain\Product\Http\Controllers\FavoriteController@index
POST            product/favorites ................................................................................................... favorites.store › Domain\Product\Http\Controllers\FavoriteController@store
GET|HEAD        product/favorites/{favorite} .......................................................................................... favorites.show › Domain\Product\Http\Controllers\FavoriteController@show
PUT|PATCH       product/favorites/{favorite} ...................................................................................... favorites.update › Domain\Product\Http\Controllers\FavoriteController@update
DELETE          product/favorites/{favorite} .................................................................................... favorites.destroy › Domain\Product\Http\Controllers\FavoriteController@destroy
GET|HEAD        product/products ...................................................................................................... products.index › Domain\Product\Http\Controllers\ProductController@index
POST            product/products ...................................................................................................... products.store › Domain\Product\Http\Controllers\ProductController@store
GET|HEAD        product/products/{product} .............................................................................................. products.show › Domain\Product\Http\Controllers\ProductController@show
PUT|PATCH       product/products/{product} .......................................................................................... products.update › Domain\Product\Http\Controllers\ProductController@update
DELETE          product/products/{product} ........................................................................................ products.destroy › Domain\Product\Http\Controllers\ProductController@destroy
```

Routing kısmında oluşturulan route isimleri sisteme otomatik olarak permission olarak eklenir ve admin rolüne ilgili
yetki verilir.

### Belirlenen name standartları;

- Yukarıdaki örnekte Product domain için oluşturulan Brand modeline örnek

| Layer               | Class Name                                     |
|---------------------|------------------------------------------------|
| Model               | **Brand**                                      |
| Repository          | **BrandRepository**                            |
| RepositoryInterface | **BrandRepositoryInterface**                   |
| Service             | **BrandService**                               |
| ServiceInterface    | **BrandServiceInterface**                      |
| Requests            | **StoreBrandRequest** - **UpdateBrandRequest** |
| Resource            | **BrandResource**                              | 
| Cache Layer         | **BrandCacheDecorator**                        |
| Query               | **BrandQuery**                                 |
| DTO                 | **BrandData**                                  |
| Provider            | **ProductServiceProvider**                     |

### Yukarıdakilere ek olarak aşağıdaki dosyalar eklendi.

| File      | Class Name                                                        |
|-----------|-------------------------------------------------------------------|
| Migration | **database/migrations/2025_06_10_190744_create_brands_table.php** |
| Test      | **tests/Unit/Product/BrandTest.php**                              |
| Factory   | **database/factories/Domain/Product/Models/BrandFactory.php**     |                             |

# (Tümü ingilizce)

### Belirlenen standartlar

| -                 | Class Name             |
|-------------------|------------------------|
| Değişkenler       | **camelCase**          |
| Sabit değişkenler | **UPPER_SNAKE_CASE**   |
| Classlar          | **PascalCase**         | 
| Route'lar         | Çoğul - **kebab-case** |
| Tablo isimleri    | **snake_case**         |
| Sütun isimleri    | **snake_case**         |
| Response keyleri  | **snake_case**         |

- Eklenen tüm class'lar için **AdıTürü** şeklinde isimlendirme yapılmıştır.
- Örneğin; **ModelCacheTrait, ModelCacheException, ModelCacheEnum** vs..

## Çıktı detayları:

### ProductServiceProvider.php

```php
<?php
namespace Domain\Product\Providers;

class ProductServiceProvider extends ServiceProvider
{
    /**
     * Register services.
     *
     * @return void
     */
    public function register(): void
    {
        $this->app->bind(BrandRepositoryInterface::class, fn () =>
            new BrandCacheDecorator(
                new BrandRepository(new Brand)
            )
        );
        $this->app->bind(BrandServiceInterface::class, BrandService::class);
    }

    /**
     * Bootstrap services.
     *
     * @return void
     */
    public function boot(): void
    {
        Route::middleware('api')->group(function () {
            $this->loadRoutesFrom(__DIR__ . '/../Http/Routes/product.php');
        });
    }
}
```

## Base

### BaseRepositoryInterface.php
Repository katmanının amacı sadece model ile ilgili DB işlemlerinin yapılmasıdır DB işlemleri haricindeki işlemler repository katmanında olmamalıdır.
```php
<?php

namespace Base\Contracts;


interface BaseRepositoryInterface
{
    use AutoSaveGlobals; // <- Birden fazla modelde kullanılan relation'lar varsa bu trait relationların otomatik olarak sync işlemini gerçekleştirir ve kod tekrarının önüne geçer.
    // Örneğin;
    // Çoklu dil desteklerinde
    // Birden fazla model ile ilişkili olabilen medya ilişkileri vs. vs.

    /**
     * Verilen parametrelere göre sonucları sayfalayarak döndür
     *
     * @param array $conditions Where parametreleri
     * @param integer $perPage Her sayfada listelenecek kayıt sayısı default:10
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return LengthAwarePaginator
     */
    public function paginate(array $conditions = [], int $perPage = 10, array $with = [], array $columns = ['*']): LengthAwarePaginator;

    /**
     * Verilen query ve parametrelere göre sonucları sayfalayarak döndür
     *
     * @param BaseQuery $baseQuery Request parametrelerinden oluşturulan query'e parametreleri ekleyerek döndür
     * @param array $conditions Where parametreleri
     * @param integer $perPage Her sayfada listelenecek kayıt sayısı default:10
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return LengthAwarePaginator
     */
    public function queryPaginate(BaseQuery $baseQuery, array $conditions = [], int $perPage = 10, array $with = [], array $columns = ['*']): LengthAwarePaginator;

    /**
     * Verilen parametrelere göre tüm sonucları döndür
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return Collection
     */
    public function get(array $conditions = [], array $with = [], array $columns = ['*']): Collection;

    /**
     * Verilen query ve parametrelere göre tüm sonucları döndür
     *
     * @param BaseQuery $baseQuery Request parametrelerinden oluşturulan query'e parametreleri ekleyerek döndür
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return Collection
     */
    public function queryGet(BaseQuery $baseQuery, array $conditions = [], array $with = [], array $columns = ['*']): Collection;

    /**
     * Verilen parametrelere göre ilk sonucu bul
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @return Model|static|null
     */
    public function first(array $conditions = [], array $with = []): Model|static|null;

    /**
     * Parametrelere göre veriyi bul yada durdur
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @throw ModelNotFoundException<Model>
     * @return Model
     */
    public function firstOrFail(array $conditions = [], array $with = []): Model;

    /**
     * Varsa güncelle yoksa ekle
     *
     * @param array $attributes Kontrolü yapılacak veriler
     * @param array $values Yeni değerler
     * @return Model
     */
    public function updateOrCreate(array $attributes, array $values = []): Model;

    /**
     * Verilen query ve model id değerine göre modeli döndür
     *
     * @param BaseQuery $baseQuery Request parametrelerinden oluşturulan query'e parametreleri ekleyerek döndür
     * @param int|string $modelId Sorgulanacak model id
     * @param bool $unsetWheres Request parametreleri ile oluşturulan query'de eğer varsa where parametreleri kaldır
     * @return Model
     */
    public function queryFindOrFail(BaseQuery $baseQuery, int|string $modelId, bool $unsetWheres = true): Model;

    /**
     * Verilen modeli verilen değerler ile güncelle
     *
     * @param Model $model Güncelleme işlemi yapılacak model
     * @param array $attributes Güncellenecek veriler
     * @param BaseQuery|null $baseQuery Request parametrelerinden oluşturulan query'e parametreleri ekleyerek döndür
     * @param bool $unsetWheres Request parametreleri ile oluşturulan query'de eğer varsa where parametreleri kaldır
     * @return Model
     */
    public function update(Model $model, array $attributes, ?BaseQuery $baseQuery = null, ?bool $unsetWheres = true): Model;

    /**
     * Add new kayıt
     *
     * @param array $attributes Eklenecek veriler
     * @param BaseQuery|null $baseQuery Request parametrelerinden oluşturulan query'e parametreleri ekleyerek döndür
     * @param bool $unsetWheres Request parametreleri ile oluşturulan query'de eğer varsa where parametreleri kaldır
     * @throws Exception
     * @return Model
     */
    public function store(array $attributes, ?BaseQuery $baseQuery = null, ?bool $unsetWheres = true): Model;

    /**
     * Aktif model eloquent builder
     *
     * @return Builder|Collection
     */
    public function getBuilder(): Builder|Collection;

    /**
     * Id değeri verilen modeli bul
     *
     * @param int|string $modelId Sorgulanacak model id
     * @return Model
     */
    public function findOrFail(int|string $modelId): Model;

    /**
     * Id değeri verilen softdelete edilmiş modeli geri yükler
     *
     * @param int|string $modelId Sorgulanacak model id
     * @return Model
     * @throws ModelRestoreException
     */
    public function restore(int|string $modelId): Model;

    /**
     * Verilen where parametreleri için sonuç kontrolü
     *
     * @param array $conditions Where parametreleri
     * @return bool
     */
    public function exists(array $conditions = []): bool;

    /**
     * Verilen modeli sil
     *
     * @param Model $model Silinecek model
     * @return bool
     * @throws Throwable
     */
    public function destroy(Model $model): bool;

    /**
     * Translate edilecek sütun listesi
     *
     * @return array
     */
    public function translatables(): array;

    /**
     * Media collections
     *
     * @return array
     */
    public function mediables(): array;

    /**
     * Modelin cache i temizlenir
     *
     * @param Model|string|null $clear
     * @return void
     */
    public function cacheClear(Model|string|null $clear = null): void;
}
```

### BaseServiceInterface.php
- Service katmanının amacı model ile ilgili DB işlemleri haricindeki tüm işlemlerin geliştirilmesidir.

#### Örneğin;
- Repository deki bir kaç metottan alınarak farklı bir data üretilmesi.
- DB işlemi olmayan hesaplamalar, dış veri kaynaklarından veri alınması / gönderilmesi vs. vs.

#### Not: Service ve Repository katmanları aşağıdaki gibi çalışır. Bu sebeple eğer cache'e yazılması istenen bir response üretilecekse bu metot Repository katmanına eklenmelidir.

### Service <-> Cache <-> Repository

```php
<?php

namespace Base\Contracts;

interface BaseServiceInterface
{
    /**
     * Verilen parametrelere göre tüm sonucları döndür
     *
     * @param array $conditions Where parametreleri
     * @param int $perPage Her sayfada listelenecek kayıt sayısı default:10
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return LengthAwarePaginator
     */
    public function paginate(array $conditions = [], int $perPage = 10, array $with = [], array $columns = ['*']): LengthAwarePaginator;

    /**
     * Verilen query ve parametrelere göre sonucları sayfalayarak döndür
     *
     * @param array $conditions Where parametreleri
     * @param int $perPage Her sayfada listelenecek kayıt sayısı default:10
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return LengthAwarePaginator
     */
    public function queryPaginate(array $conditions = [], int $perPage = 10, array $with = [], array $columns = ['*']): LengthAwarePaginator;

    /**
     * Verilen query ve parametrelere göre tüm sonucları döndür
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return Collection
     */
    public function get(array $conditions = [], array $with = [], array $columns = ['*']): Collection;

    /**
     * Verilen query ve parametrelere göre tüm sonucları döndür
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return Collection
     */
    public function queryGet(array $conditions = [], array $with = [], array $columns = ['*']): Collection;

    /**
     * Verilen query ve parametrelere göre sonucları döndür
     * Sayfalama default: true
     * paginate:true gönderilirse sayfalayarak döndürür
     * paginate:false gönderilirse tüm listeyi döndürür
     *
     * @param array $conditions Where parametreleri
     * @param int $perPage Her sayfada listelenecek kayıt sayısı default:10
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return Collection|LengthAwarePaginator
     */
    public function getOrPaginate(array $conditions = [], int $perPage = 10, array $with = [], array $columns = ['*']): Collection|LengthAwarePaginator;

    /**
     * Sayfalama default: true
     * paginate:true gönderilirse sayfalayarak döndürür.
     * paginate:false gönderilirse tüm listeyi döndürür.
     *
     * @param array $conditions Where parametreleri
     * @param int $perPage Her sayfada listelenecek kayıt sayısı default:10
     * @param array $with Yüklenecek relation listesi
     * @param array $columns Select parametreleri
     * @return Collection|LengthAwarePaginator
     */
    public function queryGetOrPaginate(array $conditions = [], int $perPage = 10, array $with = [], array $columns = ['*']): Collection|LengthAwarePaginator;

    /**
     * Verilen parametrelere göre ilk sonucu bul
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @return Model|null
     */
    public function first(array $conditions = [], array $with = []): Model|null;

    /**
     * Parametrelere göre veriyi bul yada durdur
     *
     * @param array $conditions Where parametreleri
     * @param array $with Yüklenecek relation listesi
     * @return Model
     */
    public function firstOrFail(array $conditions = [], array $with = []): Model;

    /**
     * Varsa güncelle yoksa ekle
     *
     * @param array $attributes Kontrolü yapılacak veriler
     * @param array $values Yeni değerler
     * @return Model
     */
    public function updateOrCreate(array $attributes, array $values = []): Model;

    /**
     * Verilen query ve model id değerine göre modeli döndür
     *
     * @param int|string $modelId Sorgulanacak model id
     * @param bool $unsetWheres Request parametreleri ile oluşturulan query'de eğer varsa where parametreleri kaldır
     * @return Model
     */
    public function queryFindOrFail(int|string $modelId, bool $unsetWheres = true): Model;

    /**
     * Id değeri verilen softdelete edilmiş modeli geri yükler
     *
     * @param int|string $modelId Sorgulanacak model id
     * @return Model
     */
    public function restore(int|string $modelId): Model;

    /**
     * Verilen modeli verilen değerler ile güncelle
     *
     * @param Model $model Güncelleme işlemi yapılacak model
     * @param BaseData $baseData DTO
     * @param bool $unsetWheres Request parametreleri ile oluşturulan query'de eğer varsa where parametreleri kaldır
     * @return Model
     */
    public function update(Model $model, BaseData $baseData, bool $unsetWheres = true): Model;

    /**
     * Add new kayıt
     *
     * @param BaseData $baseData
     * @param bool $unsetWheres Request parametreleri ile oluşturulan query'de eğer varsa where parametreleri kaldır
     * @return Model
     */
    public function store(BaseData $baseData, bool $unsetWheres = true): Model;

    /**
     * Verilen where parametreleri için sonuç kontrolü
     *
     * @param array $conditions Where parametreleri
     * @return bool
     */
    public function exists(array $conditions = []): bool;

    /**
     * Id değeri verilen modeli bul
     *
     * @param int|string $modelId
     * @return Model
     */
    public function findOrFail(int|string $modelId): Model;

    /**
     * Verilen modeli sil
     *
     * @param Model $model Silinecek model
     * @return bool
     * @throws Throwable
     */
    public function destroy(Model $model): bool;

    /**
     * Modele ait cache i temizle
     *
     * @param Model|string|null $clear
     * @return void
     * @throws Throwable
     */
    public function cacheClear(Model|string|null $clear = null): void;

    /**
     * İşlemin yapılacağı repository döndürmeli
     *
     * @abstract
     * @return BaseRepositoryInterface
     */
    public function repository(): BaseRepositoryInterface;

    /**
     * İşlemin yapılacağı query döndürmeli
     *
     * @abstract
     * @return BaseQuery
     */
    public function query(): BaseQuery;
}
```

### BrandService.php
- Model ile ilgili işlemler için sınıf genişletilebilir, overwrite edilebilir. 
```php
<?php

namespace Domain\Product\Services;

class BrandService extends BaseService implements BrandServiceInterface
{
    /**
     * BrandRepositoryInterface
     *
     * @param BrandRepositoryInterface $repository
     * @return void
     */
    public function __construct(protected BrandRepositoryInterface $repository)
    {
    }

    /**
     * {@inheritDoc}
     */
    public function repository(): BrandRepositoryInterface
    {
        return $this->repository;
    }

    /**
     * {@inheritDoc}
     */
    public function query(): BrandQuery
    {
        return new BrandQuery();
    }
}
```

### BrandRepository.php
- Modelin veritabanı işlemleri için sınıf genişletilebilir, overwrite edilebilir.
```php
<?php

namespace Domain\Product\Repositories;

class BrandRepository extends BaseRepository implements BrandRepositoryInterface
{
    /**
     * Brand
     *
     * @param Brand $model
     * @return void
     */
    public function __construct(Brand $model)
    {
        parent::__construct($model);
    }
}
```

### BrandController.php
- Burada ekstra bir endpoint eklenmesi gerekmiyorsa herhangi bir değişiklik yapılmasına gerek yoktur.
```php
<?php

namespace Domain\Product\Http\Controllers;

class BrandController extends BaseController
{
    /**
     * BrandServiceInterface
     *
     * @param BrandServiceInterface $service
     * @return void
     */
    public function __construct(protected BrandServiceInterface $service)
    {
    }

     /**
      * Paginate Brand list according to query and parameters
      *
      * @see Brand
      * @see BrandQuery
      * @return AnonymousResourceCollection
      */
    public function index(): AnonymousResourceCollection
    {
        return BrandResource::collection(
            $this->service->queryPaginate()
        );
    }

    /**
     * Add new Brand
     *
     * @param StoreBrandRequest $request
     * @see StoreBrandRequest
     * @return JsonResponse
     */
    public function store(StoreBrandRequest $request): JsonResponse
    {
        return BrandResource::make(
            $this->service->store(BrandData::from($request->validated()))
        )->response()->setStatusCode(201);
    }

    /**
     * According to the model id value with query Brand
     *
     * @param int $brandId
     * @see Brand
     * @throws ModelNotFoundException
     * @return BrandResource
     */
    public function show(int $brandId): BrandResource
    {
        return BrandResource::make(
            $this->service->queryFindOrFail($brandId)
        );
    }

    /**
     * Update Brand
     *
     * @param UpdateBrandRequest $request
     * @param Brand $brand
     * @see UpdateBrandRequest
     * @return BrandResource
     */
    public function update(UpdateBrandRequest $request, Brand $brand): BrandResource
    {
        return BrandResource::make(
            $this->service->update($brand, BrandData::from($request->validated()))
        );
    }

    /**
     * Delete Brand
     *
     * @param Brand $brand
     * @throws Throwable
     * @return Response
     */
    public function destroy(Brand $brand): Response
    {
        $this->service->destroy($brand);
        return response()->noContent();
    }
}

```

### BrandCacheDecorator.php
- Repository katmanındaki tüm metotlar için cache mekanizması eklenmiştir. Yeni metot eklenirse ilgili Cache katmanına eklenen metotun cache mekanizması eklenmelidir.
- Var olan metotlarda farklı bir cache mekanizması kullanmak istenirse ilgili metotlar override edilebilir.
- Örneğin çok yoğun create update yada delete işlemi yapılan bir modeller için cache mekanizması kullanılmaması istenebilir.
```php
<?php
    // ...
    
    public function findOrFail(int|string $modelId): Model
    {
        // Repositorydeki findOrFail metodunu çağırmadan önce cache'de kontrol et varsa sonucu response et.
        // Eğer cache'te yoksa repositorydeki metodu çağır sonucu al Cache'e yaz ve response et.
        return $this->getDataIfExistCache(__FUNCTION__, func_get_args());
    }

    public function update(Model $model, array $attributes, ?BaseQuery $baseQuery = null, ?bool $unsetWheres = true): Model
    {
        // Repositorydeki update metodunu çağırmadan önce cache'i sil
        // Repositorydeki update metonu çağır sonucu response et.
        return $this->flushCacheAndUpdateData(__FUNCTION__, func_get_args());
    }
    
    // ...
```

### BrandQuery.php
[Spatie - Query Builder](https://github.com/spatie/laravel-query-builder) paketi üzerine inşa edildi. BaseQuery query builder sınıfından türetilmiştir.
Gerekli görülürse query katmanı GraphQL ile çalışacak hale getirilebilir ancak geliştirme süresini uzatacaktır.
```php
<?php

namespace Domain\Product\Http\Queries;

class BrandQuery extends BaseQuery
{
    public function __construct(?Request $request = null)
    {
        $repository = resolve(BrandRepositoryInterface::class);

        parent::__construct($repository->getBuilder(), $request);

        $this
            ->allowedFields([
                // Response'da görünmesine izin verilen field'lar
            ])
            ->allowedSorts([
                // Sıralama yapılmasına izin verilen field'lar
            ])
            ->allowedIncludes([
                // Yüklenmesine izin verilen relation'lar
            ])
            ->allowedFilters([
                // Filtrelemeler
            ]);
    }
}
```

#### Örnek Request [GET]

```text
/brands?filter[created_by]=1&sort=-id&filter[trashed]=with&include=updatedBy,createdBy,deletedBy
```

### Brand.php
- Fillable ve casts alanları modelin özelliklerine göre doldurulmalıdır.
```php
<?php

namespace Domain\Product\Models;

class Brand extends BaseModel
{
    use HasFactory;

    protected $fillable = [
        //
    ];

    /**
     * Get the attributes that should be cast.
     *
     * @return array<string, string>
     */
    protected function casts(): array
    {
        return [
        ];
    }
}
```

### StoreBrandRequest.php
```php
<?php

namespace Domain\Product\Http\Requests;

class StoreBrandRequest extends BaseFormRequest
{
    public function rules(): array
    {
        return [
            //
        ];
    }
}
```

### UpdateBrandRequest.php
```php
<?php

namespace Domain\Product\Http\Requests;

class UpdateBrandRequest extends BaseFormRequest
{
    public function rules(): array
    {
        return [
            //
        ];
    }
}
```

### BrandData.php
[Spatie - Data](https://github.com/spatie/laravel-data) paketi üzerine inşa edildi.
```php
<?php

namespace Domain\Product\DataTransferObjects;

class BrandData extends BaseData
{
    // Field'lar türleri ile birlikte burada eklenmeli
    public int $id
    public string $name
    // vs. vs.
}
```

## Yetkilendirme
[Spatie - Permissions](https://github.com/spatie/laravel-permission) paketi üzerine inşa edildi
```php
<?php

namespace Base\Concretes;

abstract class BasePolicy
{
    use HandlesAuthorization;

    public static function getModel(): string
    {
        throw new RuntimeException("Model unimplemented !");
    }

    /**
     * Determine whether the user can view any posts.
     *
     * @param User|null $user
     * @return Response
     */
    public function viewAny(?User $user): Response
    {
        $model = new (static::getModel());

        return $this->check($user)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . ' için liste görüntülemesi</strong> yetkiniz yok.');
    }

    /**
     * Determine whether the user can view the post.
     *
     * @param User|null $user
     * @param Model $model
     * @return Response
     */
    public function view(?User $user, Model $model): Response
    {
        return $this->check($user, $model)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . ' - #' . ($model?->id ?? $model?->uuid) . '</strong> için detay görüntüleme yetkiniz yok.');
    }

    /**
     * Determine whether the user can create posts.
     *
     * @param User|null $user
     * @return Response
     */
    public function create(?User $user): Response
    {
        $model = new (static::getModel());

        return $this->check($user)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . '</strong> için ekleme yetkiniz yok.');
    }

    /**
     * Determine if the given post can be updated by the user.
     *
     * @param User|null $user
     * @param Model $model
     * @return Response
     */
    public function update(?User $user, Model $model): Response
    {
        return $this->check($user, $model)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . ' - #' . $model->id . '</strong> için güncelleme yetkiniz yok.');
    }

    /**
     * Determine whether the user can delete the post.
     *
     * @param User|null $user
     * @param Model $model
     * @return Response
     */
    public function delete(?User $user, Model $model): Response
    {
        return $this->check($user, $model)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . ' - #' . $model->id . '</strong> için silme yetkiniz yok.');
    }

    /**
     * Determine whether the user can restore the post.
     *
     * @param User|null $user
     * @param Model $model
     * @return Response
     */
    public function restore(?User $user, Model $model): Response
    {
        return $this->check($user, $model)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . ' - #' . $model->id . '</strong> için restore yetkiniz yok.');
    }

    /**
     * Determine whether the user can permanently delete the post.
     *
     * @param User|null $user
     * @param Model $model
     * @return Response
     */
    public function forceDelete(?User $user, Model $model): Response
    {
        return $this->check($user, $model)
            ? Response::allow()
            : Response::deny('<strong>' . Route::currentRouteName() . ' - ' . $model->getMorphClass() . ' - #' . $model->id . '</strong> için forceDelete yetkiniz yok.');
    }

    public function check(?User $user, ?Model $model = null): bool
    {
        if ($model instanceof Media && $model->public) {
            return true;
        }

        if (!$user) {
            return false;
        }

        return $user->hasAuthorized(Route::currentRouteName());
    }
}
```

### User modelindeki hasAuthorized metodu
```php
public function hasAuthorized(string $permission): bool
{
    return Cache::remember(
        $this->id . '-' . $permission,
        Carbon::now()->addMinute(),
        fn() => $this->hasPermissionTo($permission)
    );
}
```

- Böylece özel bir durum yoksa standart aşağıdaki policy ile permission kontrolünü sağlayabiliyoruz.
- Eğer özel bir durum varsa BrandPolicy içerisinde BasePolicy metotlarını ezerek farklı kurallar da eklenebilir.

```php
<?php

namespace Domain\Product\Policies;

class BrandPolicy extends BasePolicy
{
    public static function getModel(): string
    {
        return Brand::class;
    }
}
```

### Model event'leri ile cache ve socket yönetimi
- Model event'leri ile model üzerinde yapılan işlemlerde cache temizleme ve live ekranlar için socket üzerinden refresh event'leri gönderme işlemleri yapılabilir.
```php
<?php

namespace App\Traits\Eloquent;

use Domain\Product\Models\Comment;trait ModelRefreshTrait
{
    public static function bootModelRefresh(): void
    {
        try {
            static::saved(fn(Model $model) => static::runEvents($model, ModelEventEnum::SAVED));
            static::deleted(fn(Model $model) => static::runEvents($model, ModelEventEnum::DELETED));
            static::deleting(fn(Model $model) => static::runEvents($model, ModelEventEnum::DELETING));
            static::restored(fn(Model $model) => static::runEvents($model, ModelEventEnum::RESTORED));

            static::pivotAttached(fn(Model $model) => static::runEvents($model, ModelEventEnum::PIVOT_ATTACHED));
            static::pivotDetached(fn(Model $model) => static::runEvents($model, ModelEventEnum::PIVOT_DETACHED));
            static::pivotSynced(fn(Model $model) => static::runEvents($model, ModelEventEnum::PIVOT_SYNCED));
            static::pivotUpdated(fn(Model $model) => static::runEvents($model, ModelEventEnum::PIVOT_UPDATED));
        } catch (Throwable $throwable) {
        }
    }

    protected static function runEvents(Model $model, ModelEventEnum $event): void
    {
        match ($event) {
            ModelEventEnum::DELETED => (function () use ($model): void {
                static::clearCache($model);
            })(),
            default => (function () use ($model, $event): void {
                static::clearCache($model);
                static::sendRefreshEvent($model, $event == ModelEventEnum::DELETING);
            })(),
        };
    }

    private static function clearCache(Model $class): void
    {
        RepositoryCacheHelper::clearCurrentCache($class);

        match (true) {
            $class instanceof Brand => (function () use ($class): void {
                static::cache(ProductRepositoryInterface::class, ['index']);
            })(),
            $class instanceof Comment => (function () use ($class): void {
                static::cache(CategoryRepositoryInterface::class, ['index', $class->category]);
                static::cache(ProductRepositoryInterface::class, ['index', $class->product]);
            })(),
            default => (function () {
            })(),
        };
    }

    protected static function sendRefreshEvent(Model $model, bool $sync): void
    {
        if (App::environment('testing')) {
            return;
        }

        $key = 'refresh-' . get_class($model) . '-' . $model->getKey();

        if (Cache::get($key, false)) {
            return;
        }

        Cache::put($key, time(), now()->addSeconds(2));

        match (true) {
            $model instanceof Product => (function () use ($model, $sync): void {
                ProductRefreshEvent::dispatch($model->id);
            })(),
            $model instanceof Comment => (function () use ($model, $sync): void {
                ProductRefreshEvent::dispatch($model->product_id);
            })(),
            default => null,
        };
    }
}
```

### Sunucu maliyetlerini düşürmek için swoole web server kullanılabilir
Böylece php nodejs gibi async çalışacak ve performans olarak 10 kat'a kadar, cpu maaliyeti olarakta %90'a kadar tasarruf
sağlanabilir.
Laravel'in official octane paketi swoole ile tam uyumlu çalışmaktadır.
Supervisor conf file'da aşağıdaki gibi swoole web server kolayca başlatılabilir.

```ini
[program:php]
command = /usr/bin/php -d variables_order=EGPCS /var/www/html/artisan octane:start --watch --server=swoole --host=0.0.0.0 --max-requests=1000 --workers=4 --task-workers=12 --rpc-port=6001 --port=8000
user = sail
autostart = true
autorestart = true
environment = LARAVEL_SAIL="1"
logfile = /dev/null
logfile_maxbytes = 0
stdout_logfile = /dev/stdout
stderr_logfile = /dev/stderr
stdout_maxbytes = 0
stderr_maxbytes = 0
stdout_logfile_maxbytes = 0
stderr_logfile_maxbytes = 0
```
---
- Bu uygulama şu anda RESTful api olarak çalışmaktadır. Eğer daha fantastik geliştirmelere girelim istersek gRPC çalışacak
hale getirebiliriz.
gRPC geliştirme süreçlerini bir miktar yavaşlatabilir ancak performans artışı ve trafik tasarrufu sağlayabilir. ChatGPT
karşılaştırması aşağıdaki gibi.

| Test Türü          | REST (JSON)      | gRPC (ProtoBuf)     | Kazanç                   |
|--------------------|------------------|---------------------|--------------------------|
| JSON decode/encode | 3–10x yavaş      | Çok hızlı           | ✅ %70-90 daha hızlı      |
| Payload boyutu     | 1 KB             | \~200–300 byte      | ✅ 3–5 kat daha küçük     |
| İstek süresi       | 100ms            | 10–20ms             | ✅ 5–10x daha hızlı       |
| Paralel istek      | HTTP/1.1 sınırlı | HTTP/2 multiplexing | ✅ Daha yüksek throughput |

### Dokümantasyon
- Sistem'de POSTMAN collection'u [Swagger API Documentation](https://petstore.swagger.io/#/)'a dönüştürmek için basit bir
  script'te bulunmaktadır. Geliştirilen tüm endpointler bu dokümantasyon içerisine tüm detayları ile eklenecektir.
- [Doctum Documentation](https://github.com/code-lts/doctum) entegre edilecek ve [Bu örnekteki gibi](https://api.laravel.com/docs/12.x/index.html) class dokümantasyonu da her deploy'da otomatik güncellenecek şekilde tasarlanacaktır.
----- 

### Test
Test'ler otomatik doldurulmaz her endpoint için özel olarak yazılacaktır. Deploy sırasında testler çalıştırılacak ve hata oluşması durumunda işlem durdurularak deploy yapılmayacaktır.

-----






## Code Review Önemi

Tecrübe düzeyinden bağımsız olarak, *code review* (kod incelemesi), yazılım geliştirme süreçlerinin en kritik aşamalarından biridir. Her bir **pull request (PR)**, merge edilmeden önce mutlaka başka bir geliştirici tarafından incelenmelidir.

Bu sayede:

- Yazılım kalitesi artırılır,
- Potansiyel hatalar veya güvenlik açıkları daha erken tespit edilir,
- Proje standartlarının (mimari yapı, kod yazım kuralları vb.) dışına çıkılması engellenir,
- Yazılımın sürdürülebilirliği sağlanır.

Commit’i gönderen kişi, review sürecini yürüten kişi dahi olsa, farklı bir geliştirici tarafından kontrol edilmesi bu sürecin temel prensibidir.

---

## Sonuç ve Önerilen Yapı

Verinin hızla büyüdüğü günümüzde, ileriye dönük hedefler göz önünde bulundurulduğunda, veritabanı ve sistem mimarisinin yeniden tasarlanması kritik öneme sahiptir. Bu sayede:

- **Veri tutarlılığı**,
- **Güvenlik**,
- **Kod kalitesi** gibi temel alanlarda ciddi iyileştirmeler mümkün olacaktır.

Mevcut sistem, dağınık ve karmaşık bir yapıdadır. Bu yapı merkezi bir mimariyle yeniden tasarlandığında:

- Veri yönetimi kolaylaşacak,
- Sistem daha sürdürülebilir hale gelecek,
- Güvenlik ihlali gibi durumlarda sadece merkezi sistem inceleneceği için tespit ve müdahale süreci sadeleşecektir.

Ayrıca, kullanıcı istekleri sadece token bazlı bir servis üzerinden yönlendirileceğinden, “kim ne yaptı” sorusu net şekilde izlenebilir olacaktır.

---

## Command'ın Sistem İçindeki Rolü

Command’ın temel geliştirme amacı:

- Temiz kod üretimini teşvik etmek,
- Kod tekrarını engellemek,
- Geliştiricilerin ortak standartlarla çalışmasını sağlamak,
- Geliştirme süreçlerini hızlandırmaktır.

Ancak, mevcut karmaşık ve tutarsız veritabanı yapısı, bu hedeflerin gerçekleştirilmesinde ciddi engeller oluşturmaktadır. Bu nedenle:

- Veritabanı tek bir yapıya indirgenmeli veya en azından veri tekrarına neden olan tablo ve veritabanları sistem dışına çıkarılmalıdır.
- Gereksiz tablolar ve alanlar temizlenmeli, veritabanı mevcut ihtiyaçlara göre yeniden tasarlanmalıdır.
- Hatalı veriler temizlenmeli, ilişkiler açık ve net şekilde kurulmalıdır.

Veritabanı tercihlerine göre:

- **İlişkisel yapı** kullanılıyorsa tüm ilişkiler foreign key’lerle desteklenmelidir.
- **NoSQL** tercih edilirse, referans yapıları ile veri tutarlılığı sağlanmalıdır.

---

## Mimari ve Operasyonel Faydalar

- Tüm projeler veritabanına doğrudan değil, yalnızca bu merkezi servis üzerinden erişim sağlayacaktır.
- Kullanıcılar, oturum açarken bu servisten token alacak ve tüm istekleri bu token ile iletecektir.
- Model olayları (CRUD işlemleri) kullanıcı bazlı izlenebilecek; alan bazında, işlem öncesi-sonrası loglama yapılabilecektir.
- Dış veri kullanan tüm projeler yine bu servis üzerinden veri çekecektir. Bu sayede dış sistemlerdeki bir değişiklik tüm projeleri etkilemeyecek; sadece servis güncellenerek tüm sistemler güncel hale getirilecektir.
- Dış verilerdeki alan değişiklikleri otomatik olarak kontrol edilip nokta atışı bildirim yapılabilecek; sorun anında fix uygulanıp merkezi olarak çözüm sağlanabilecektir.
- Sistem yoğunluk durumuna göre **ECS ile otomatik olarak ölçeklenebilir**; **serverless** mimaride container’a doğrudan erişim ihtiyacı ortadan kalkacaktır.
- **ENV değişkenleri**, **AWS Secrets Manager** üzerinden güvenli şekilde yönetilecektir.
- Docker ile çalışma ortamı sabitlenerek versiyon kaynaklı hataların önüne geçilecek, tek komutla geliştirme ortamı (nginx, swoole, redis, MySQL, MongoDB vb.) ayağa kaldırılabilecektir.

---

## DB veya Sunucu Erişimi Konusundaki İtirazlara Yanıt

- Veritabanında yapılacak veri değişiklikleri *command* üzerinden,
- Yapısal değişiklikler ise *migration* yoluyla gerçekleştirildiği için tüm işlemler `git history` üzerinden adım adım takip edilebilecektir.
- Daha önce bu ihtiyaç için EC2 üzerinde kapalı durumda bir sunucu hazır bulundurmuştuk. Gerektiğinde açarak, bu sunucu üzerinde kurulu olan projeyi hazırladığımız bir shell script ile `master` branch'ten güncelleterek gereken komutları çalıştırdık. İşimiz tamamlanınca da sunucuyu kapattık, böylece maaliyeti düşürmüş olduk.
- Bu yöntemle ECS üzerinde doğrudan işlem yapma ihtiyacı da ortadan kaldırılmış oldu.

---

## Özetle

- Güvenlik açığı durumunda onlarca farklı sunucuda inceleme yapmak gerekmeyecek.
- Tüm kullanıcı ve servis erişimleri tek noktadan yönetilecek ve izlenebilir olacaktır.
- Temiz, sade, dokümantasyonu tam bir yapı oluşturulacak.
- Yeni geliştiriciler çok daha hızlı projeye adapte olabilecek.
- Bu yapı, WBV sisteminin SOLID prensiplerine bağlı, en güncel ve güçlü teknolojilerle, sürdürülebilir bir mimariye kavuşturulması hedefiyle tasarlanmıştır. 
- WBV'yi yeniden inşaa etmek, sorunları çözmekle kalmayacak, aynı zamanda gelecekteki gelişmelere de açık düzenli bir yapı sunacaktır.

----------

