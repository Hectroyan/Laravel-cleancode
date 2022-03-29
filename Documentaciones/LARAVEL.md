# Laravel-cleancode

- [Laravel-cleancode](#laravel-cleancode)
  - [2. Laravel coding standards](#2-laravel-coding-standards)
    - [2.1 La forma de sintaxis mas corta](#21-la-forma-de-sintaxis-mas-corta)
    - [2.2. Principio de responsabilidad unica](#22-principio-de-responsabilidad-unica)
    - [2.3. FAT MODELS, SKINNY CONTROLLERS](#23-fat-models-skinny-controllers)
    - [2.4. Validacion](#24-validacion)
    - [2.5. La logica de negocio deve ir en una clase de Servicio](#25-la-logica-de-negocio-deve-ir-en-una-clase-de-servicio)
    - [2.6. No te repitas](#26-no-te-repitas)
    - [2.7. Haz preferente Elocuent vs Query Builder o rawSQL. Tanto como colecciones por encima de arrays.](#27-haz-preferente-elocuent-vs-query-builder-o-rawsql-tanto-como-colecciones-por-encima-de-arrays)
    - [2.8. Asignacion en masa](#28-asignacion-en-masa)
    - [2.9. NO EJECUTES QUERIES EN LOS ARCHIVOS BLADE CAUSAN EL (N + 1)](#29-no-ejecutes-queries-en-los-archivos-blade-causan-el-n--1)
    - [2.10. Comenta tu codigo, pero haz preferente que el metodo y variables sean descriptivos](#210-comenta-tu-codigo-pero-haz-preferente-que-el-metodo-y-variables-sean-descriptivos)
    - [2.11. Usa archivos de configuracion, lenguaje y constantes en lugar de hardcodear texto en el codigo](#211-usa-archivos-de-configuracion-lenguaje-y-constantes-en-lugar-de-hardcodear-texto-en-el-codigo)
    - [2.12. Usa dentro de lo posible los paquetes y herramientas aceptadas por la comunidad](#212-usa-dentro-de-lo-posible-los-paquetes-y-herramientas-aceptadas-por-la-comunidad)
    - [2.13. Usa IoC containers o facades en lugar de nuevas clases](#213-usa-ioc-containers-o-facades-en-lugar-de-nuevas-clases)
    - [2.14. No traigas datos directamente del .env](#214-no-traigas-datos-directamente-del-env)
    - [2.15. No traigas datos directamente del .env](#215-no-traigas-datos-directamente-del-env)
    - [2.16. No traigas datos directamente del .env](#216-no-traigas-datos-directamente-del-env)
    - [2.17. Guarda las fechas en formato standard. Usa mutadores y accesos para modificarlo.](#217-guarda-las-fechas-en-formato-standard-usa-mutadores-y-accesos-para-modificarlo)
    - [2.18. Nunca pongas logica dentro de los archivos de ruta y nombralas](#218-nunca-pongas-logica-dentro-de-los-archivos-de-ruta-y-nombralas)
    - [2.19. Minimiza el uso de PHP vanilla en las plantillas blade](#219-minimiza-el-uso-de-php-vanilla-en-las-plantillas-blade)

## 2. Laravel coding standards

Se deben usar siempre los standares PSR.

| What                          | How                                                                                 | good                                    | bad                                             |
| ----------------------------- | ----------------------------------------------------------------------------------- | --------------------------------------- | ----------------------------------------------- |
| Controller                    | singular                                                                            | ArticleController                       | ArticlesController                              |
| Rutas                         | plural                                                                              | articles/1                              | article/1                                       |
| Nombres de rutas              | snake_case con notacion de punto                                                    | users.show_active                       | users.show-active, show-active-users            |
| Model                         | singular                                                                            | User                                    | Users                                           |
| relaciones hasOne o belongsTo | singular, camelCase                                                                 | articleComment                          | articleComments, article_comment                |
| Todas las otras relaciones    | plural, camelCase                                                                   | articleComments                         | articleComment, article_comments                |
| Tablas                        | plural, snake_case                                                                  | article_comments                        | article_comment, articleComments                |
| Tabla pivote                  | singular, snake_case, nombres de modelo por orden alfabetico                        | article_user                            | user_article, articles_users                    |
| Columna tabla                 | snake_case sin nombre de modelo                                                     | meta_title                              | MetaTitle; article_meta_title                   |
| Foreign key                   | modelo en singular con sufijo _id                                                   | article_id                              | ArticleId, id_article, articles_id              |
| Primary key                   |                                                                                     | id                                      | custom_id                                       |
| Migracion                     |                                                                                     | 2017_01_01_000000_create_articles_table | 2017_01_01_000000_articles                      |
| Metodo                        | camelCase                                                                           | getAll                                  | get_all                                         |
| Funcion                       | snake_case                                                                          | abort_if                                | abortIf                                         |
| Metodo en controlador         | mas info: [table](https://laravel.com/docs/master/controllers#resource-controllers) | store                                   | saveArticle                                     |
| Metodo en clase de Test       | camelCase                                                                           | testGuestCannotSeeArticle               | test_guest_cannot_see_article                   |
| Propiedad de modelo           | snake_case                                                                          | $model->model_property                  | $model->modelProperty                           |
| Variable                      | camelCase                                                                           | $anyOtherVariable                       | $any_other_variable                             |
| Coleccion                     | descriptivo, plural                                                                 | $activeUsers = User::active()->get()    | $active, $data                                  |
| Objeto                        | descriptivo, singular                                                               | $activeUser = User::active()->first()   | $users, $obj                                    |
| Index archivos config lang    | snake_case                                                                          | articles_enabled                        | ArticlesEnabled; articles-enabled               |
| Vistas                        | kebab-case                                                                          | show-filtered.blade.php                 | showFiltered.blade.php, show_filtered.blade.php |
| Configuracion                 | kebab-case                                                                          | google-calendar.php                     | googleCalendar.php, google_calendar.php         |
| Interfaz                      | adjetivo o nombre                                                                   | Authenticatable                         | AuthenticationInterface, IAuthentication        |
| Trait                         | adjetivo                                                                            | Notifiable                              | NotificationTrait                               |
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.1 La forma de sintaxis mas corta

Intanta siempre que sea posible usar la sintaxis mas corta (mas legible)

**Mal**
``` php
   $request->session()->get('cart');
   $request->input('name');
```
**Bien**
``` php
   session('cart');
   $request->name;
```

Mas ejemplos:
| Sintaxis comun                                | Mas corta y legible             |
| --------------------------------------------- | ------------------------------- |
| Session::get('cart')                          | session('cart')                 |
| $request->session()->get('cart')              | session('cart')                 |
| Session::put('cart', $data)                   | session(['cart' => $data])      |
| $request->input('name'), Request::get('name') | $request->name, request('name') |
| return Redirect::back()                       | return back()                   |

| is_null($object->relation) ? $object->relation->id : null }          | optional($object->relation)->id                  |
| return view('index')->with('title', $title)->with('client', $client) | return view('index', compact('title', 'client')) |
| $request->has('value') ? $request->value : 'default';                | $request->get('value', 'default')                |
| Carbon::now(), Carbon::today()                                       | now(), today()                                   |
| App::make('Class')                                                   | app('Class')                                     |
| ->where('column', '=', 1)                                            | ->where('column', 1)                             |
| ->orderBy('created_at', 'desc')                                      | ->latest()                                       |
| ->orderBy('age', 'desc')                                             | ->latest('age')                                  |
| ->orderBy('created_at', 'asc')                                       | ->oldest()                                       |
| ->select('id', 'name')->get()                                        | ->get(['id', 'name'])                            |
| ->first()->name                                                      | ->value('name')                                  |
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.2. Principio de responsabilidad unica

Una clase y metodo solo deben tener una responsabilidad.

**Mal**
``` PHP
   public function getFullNameAttribute()
   {
      if (auth()->user() && auth()->user()->hasRole('client') && auth()->user()->isVerified()) {
            return 'Mr. ' . $this->first_name . ' ' . $this->middle_name . ' ' . $this->last_name;
      } else {
            return $this->first_name[0] . '. ' . $this->last_name;
      }
   }
```
**Bien**
``` PHP
   public function getFullNameAttribute()
   {
      return $this->isVerifiedClient() ? $this->getFullNameLong() : $this->getFullNameShort();
   }
 
   public function isVerifiedClient()
   {
      return auth()->user() && auth()->user()->hasRole('client') && auth()->user()->isVerified();
   }

   public function getFullNameLong()
   {
      return 'Mr. ' . $this->first_name . ' ' . $this->middle_name . ' ' . $this->last_name;
   }

   public function getFullNameShort()
   {
      return $this->first_name[0] . '. ' . $this->last_name;
   }
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.3. FAT MODELS, SKINNY CONTROLLERS

Pon tooooooooodo el DB binding en los modelos de elocuent o en una clase Repositorio ya sea usando querybuilder o rawsql:


**Mal**
``` PHP
   public function index()
   {
      $clients = Client::verified()
         ->with(['orders' => function ($q) {
               $q->where('created_at', '>', Carbon::today()->subWeek());
         }])
         ->get();

      return view('index', ['clients' => $clients]);
   }
```
**Bien**
``` PHP
   public function index()
   {
      return view('index', ['clients' => $this->client->getWithNewOrders()]);
   }

   class Client extends Model
   {
      public function getWithNewOrders()
      {
         return $this->verified()
               ->with(['orders' => function ($q) {
                  $q->where('created_at', '>', Carbon::today()->subWeek());
               }])
               ->get();
      }
   }
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.4. Validacion

No dejes las validaciones en el controlador, crea clases para ello:

**Mal**
``` PHP
   public function store(Request $request)
   {
      $request->validate([
         'title' => 'required|unique:posts|max:255',
         'body' => 'required',
         'publish_at' => 'nullable|date',
      ]);

      ....
   }
```
**Bien**
``` PHP
   public function store(PostRequest $request)
   {
      ....
   }

   class PostRequest extends Request
   {
      public function rules()
      {
         return [
               'title' => 'required|unique:posts|max:255',
               'body' => 'required',
               'publish_at' => 'nullable|date',
         ];
      }
   }
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.5. La logica de negocio deve ir en una clase de Servicio

Un controlador solo debe tener una responsabilidad, asi que mueve esa logica a una clase de Servicio

**Mal**
``` PHP
   public function store(Request $request)
   {
      if ($request->hasFile('image')) {
         $request->file('image')->move(public_path('images') . 'temp');
      }

      ....
   }
```
**Bien**
``` PHP
   public function store(Request $request)
   {
      $this->articleService->handleUploadedImage($request->file('image'));

      ....
   }

   class ArticleService
   {
      public function handleUploadedImage($image)
      {
         if (!is_null($image)) {
               $image->move(public_path('images') . 'temp');
         }
      }
   }
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.6. No te repitas

Reusa el codigo todo lo posible. SPR te ayuda a evitar la duplicacion. Por cierto reusa en todos lugares (Blade templates, Eloquent scopes etc.)

**Mal**
``` PHP
   public function getActive()
   {
      return $this->where('verified', 1)->whereNotNull('deleted_at')->get();
   }

   public function getArticles()
   {
      return $this->whereHas('user', function ($q) {
               $q->where('verified', 1)->whereNotNull('deleted_at');
         })->get();
   }
```
**Bien**
``` PHP
   public function scopeActive($q)
   {
      return $q->where('verified', 1)->whereNotNull('deleted_at');
   }

   public function getActive()
   {
      return $this->active()->get();
   }

   public function getArticles()
   {
      return $this->whereHas('user', function ($q) {
               $q->active();
         })->get();
   }
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.7. Haz preferente Elocuent vs Query Builder o rawSQL. Tanto como colecciones por encima de arrays.

Eloquent te ayuda a escribir codigo mas legible y mantenible. Ademas teniendo funcionalidades especificas que te ayudan a realizar tus tareas (softdeletes, events, scopes, etc.)

**Mal**
``` SQL
   SELECT *
    FROM `articles`
    WHERE EXISTS (SELECT *
                  FROM `users`
                  WHERE `articles`.`user_id` = `users`.`id`
                  AND EXISTS (SELECT *
                              FROM `profiles`
                              WHERE `profiles`.`user_id` = `users`.`id`)
                  AND `users`.`deleted_at` IS NULL)
    AND `verified` = '1'
    AND `active` = '1'
    ORDER BY `created_at` DESC
```
**Bien**
``` PHP
   Article::has('user.profile')->verified()->latest()->get();
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.8. Asignacion en masa

**Mal**
``` PHP
   $article = new Article;
   $article->title = $request->title;
   $article->content = $request->content;
   $article->verified = $request->verified;
   // Add category to article
   $article->category_id = $category->id;
   $article->save();
```
**Bien**
``` PHP
   $category->article()->create($request->all());
```
[🔝 Volvamos arriba](#laravel-cleancode)

### 2.9. NO EJECUTES QUERIES EN LOS ARCHIVOS BLADE CAUSAN EL (N + 1)

**Mal (para 100 usuarios la query se ejecutará 101 veces 😭)**
``` PHP
   @foreach (User::all() as $user)
 
   @endforeach
```
**Bien (para 300 usuarios la query se ejecutara 2 veces 😎)**
``` PHP
   $users = User::with('profile')->get();
 
   ...
   @foreach ($users as $user)

   @endforeach
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.10. Comenta tu codigo, pero haz preferente que el metodo y variables sean descriptivos

**Mal**
``` PHP
   if (count((array) $builder->getQuery()->joins) > 0)
```
**Mejor**
``` PHP
   // Determine if there are any joins.
   if (count((array) $builder->getQuery()->joins) > 0)

```
**Bien**
``` PHP
   if ($this->hasJoins())
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.11. Usa archivos de configuracion, lenguaje y constantes en lugar de hardcodear texto en el codigo

**Mal**
``` PHP
   public function isNormal()
   {
      return $article->type === 'normal';
   }

   return back()->with('message', 'Your article has been added!');
```
**Bien**
``` PHP
   public function isNormal()
   {
      return $article->type === Article::TYPE_NORMAL;
   }

   return back()->with('message', __('app.article_added'));
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.12. Usa dentro de lo posible los paquetes y herramientas aceptadas por la comunidad

| Tarea                     | Standard                               |
| ------------------------- | -------------------------------------- |
| Authorization             | Policies                               |
| Compiling assets          | Laravel Mix                            |
| Development Environment   | Homestead                              |
| Deployment                | Laravel Forge                          |
| Unit testing              | PHPUnit, Mockery                       |
| Browser testing           | Laravel Dusk                           |
| DB                        | Eloquent                               |
| Templates                 | Blade                                  |
| Working with data         | Laravel collections                    |
| Form validation           | Request classes                        |
| Authentication            | Built-in                               |
| API authentication        | Laravel Passport                       |
| Creating API              | Built-in                               |
| Working with DB structure | Migrations                             |
| Localization              | Built-in                               |
| Realtime user interfaces  | Laravel Echo, Pusher                   |
| Generating testing data   | Seeder classes, Model Factories, Faker |
| Task scheduling           | Laravel Task Scheduler                 |
| DB                        | MySQL, PostgreSQL, SQLite, SQL Server  |
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.13. Usa IoC containers o facades en lugar de nuevas clases

La sintaxis new Class crea acoplamiento entre clases y complicados testeos. Usa IoC containers o facades en lugar de ello.

**Mal**
``` PHP
   $user = new User;
   $user->create($request->all());
```
**Bien**
``` PHP
   public function __construct(User $user)
   {
      $this->user = $user;
   }

   ....

   $this->user->create($request->all());
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.14. No traigas datos directamente del .env

En laravel puedes pasar los datos usando el config() helper para usar datos en la aplicacion.

**Mal**
``` PHP
   $apiKey = env('API_KEY');
```
**Bien**
``` PHP
   // config/api.php
      'key' => env('API_KEY'),

   // Use the data
      $apiKey = config('api.key');
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.15. No traigas datos directamente del .env

En laravel puedes pasar los datos usando el config() helper para usar datos en la aplicacion.

**Mal**
``` PHP
   $apiKey = env('API_KEY');
```
**Bien**
``` PHP
   // config/api.php
      'key' => env('API_KEY'),

   // Use the data
      $apiKey = config('api.key');
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.16. No traigas datos directamente del .env

En laravel puedes pasar los datos usando el config() helper para usar datos en la aplicacion.

**Mal**
``` PHP
   $apiKey = env('API_KEY');
```
**Bien**
``` PHP
   // config/api.php
      'key' => env('API_KEY'),

   // Use the data
      $apiKey = config('api.key');
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.17. Guarda las fechas en formato standard. Usa mutadores y accesos para modificarlo.

**Bien**
``` PHP
   // Model
    protected $dates = ['ordered_at', 'created_at', 'updated_at']
    public function getCreatedAttribute($date)
    {
        return $date->format('m-d');
    }
 
    // View
    {{$created}}
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.18. Nunca pongas logica dentro de los archivos de ruta y nombralas

**Mal**
``` PHP
   Route::get('/users', function(){
      return view('users.index',['users',User::all()]);
   });
```
**Bien**
``` PHP
   // UserController
      public function index() {
         return view('users.index',['users',User::all()]);
      }
   // Route Web
      Route::get('/users', 'UserController@index')->name('users-index');
```
[🔝 Volvamos arriba](#laravel-cleancode)
### 2.19. Minimiza el uso de PHP vanilla en las plantillas blade

**Mal**
``` PHP
   <head>
   </head>
   <body>
      <?= 'hola mundo' ?>
      <?php 
         $contador = 1;
      ?>
      <?php 
      if($contador == 1){
      ?>
      <p><?= $contador ?></p>
      <?php } ?>
   </body>
```
**Bien**
``` PHP
   <head>
   </head>
   <body>
      {{ 'hola mundo' }}
      @php
         $contador = 1;
      @endphp
      @if($contador == 1)
         <p>{{$contador}}</p>
      @endif

   </body>
```
[🔝 Volvamos arriba](#laravel-cleancode)

Esta en proceso de estructuracion así que se seguiran añadiendo cositas

...