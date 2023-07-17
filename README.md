# LaraGigs
Traversy Media - Learn the Laravel PHP framework from scratch by building a job listing application with Laravel 10 and MySQL.

## Task Description
This project is segregated step by step task follow this.

### 🛠 Setting Up Laravel
A new project create from laravel.com
```
composer global require laravel/installer
```
```
laravel new laragigs
```

### 🔗 Routing & Responses
```php
// Custom Header
Route::get('/hello', function(){
    return response('<h1>Hello World</h1>', 200)
    ->header('Content-Type', 'text/plain')
    ->header('foo', 'bar'); //Custom Header
});

// Wilecard
Route::get('/posts/{id}', function($id){
    // ddd($id); // Dump, Die, Debug
    return response('Post '.$id);
})->where('id', '[0-9]+');

// Request and Query String/Params
Route::get('/search', function(Request $request){
    return $request->name .' '.$request->city;
});
```
### 📱 API Routing
```php
Route::get('/posts', function(){
    return response()->json([
        'posts' => [
            [
                'title' => 'Post One'
            ]
        ]
    ]);
});

// Access API endpoint form URL or Postman
http://laragigs.test/api/posts
````

### 👁 Basic View & Passing Data
```php
Route::get('/', function () {
    return view('listings', [
        'heading' => 'Latest Listing',
        'listings' =>  [
            [
              'id' => 1,
              'title' => Title One,
              'description' => 'Some text some text some text some text some text'
            ],
            [
              'id' => 2,
              'title' => Title Two,
              'description' => 'Some text some text some text some text some text'
            ]
        ]
    ]);
    // return view('listing.php');
});
```

Create a view page in the resource folder `resources > views > listings.php`
```php
<h1><?php echo $heading; ?></h1>
<?php foreach($lisitng as $listing): ?>
    <h2><?php echo $listing['title]; ?></h2>
    <p><?php echo $listing['description]; ?></p>
<?php endforeach; ?>
```

### 🥽 Blade Templates & Basic Directives
Rename listing.php to `resources > views > listings.blade.php`
```php
<h1>{{$heading}}</h1>

@unless(count($listing) == 0)

@foreach($lisitng as $listing)
  <h2> <a href="/listings/{{$listing['id']}}"> {{$listing['title]}} </a> </h2>
  <p>{{$listing['description]}}</p>
@endforeach

@else
<p>No listing found</p>
@endunless
```

### Creating a Basic Model
Create a new file in Models folder `app > Models > Listings.php`

```php
namespace App\Models;

class Listing {
  // All Listing
  public static function all(){
    return [
      [
        'id' => 1,
        'title' => Title One,
        'description' => 'Some text some text some text some text some text'
      ],
      [
        'id' => 2,
        'title' => Title Two,
        'description' => 'Some text some text some text some text some text'
      ]
    ]
  }

  // Single Listing
  public static function find(){
    $listing = self::all();

    foreach($listing as $listing) {
      if($listing['id] == $id){
        return $listing;
      }
    }
  }
}
```

Modify in routes folder `routes > web.php`
```php
use App\Models\Listing;
.....
.......
// All Listing
Route::get('/', function () {
    return view('listings', [
        'heading' => 'Latest Listing',
        'listings' =>  Listing::all() 
    ]);
});

// Single Listing
Route::get('/listings/{id}', function($id) {
  return view('listing', [
    'listing' => Listing::find($id)
  ]);
});
```

Create a new file in the View folder `resources > view > Listing.blade.php` for details listing
```php
<h2>{{$listing['title]}}</h2>
<p>{{$listing['description]}}</p>
```

### Create a Database & User
We can db connection setup in `config > database.php` or `.env` files modify

Use MySQL Cheat Sheet [Brad Traversy](https://gist.github.com/bradtraversy/c831baaad44343cc945e76c2e30927b3)

### Creating Database Migration
Create listing table migration
```
php artisan make:migration create_listings_table
```
Modify `create_listings_table` migration file from `database > migrations > filename`
```php
public function up(): void
    {
        Schema::create('listings', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->string('title');
            $table->string('tags');
            $table->string('logo')->nullable();
            $table->string('company');
            $table->string('location');
            $table->string('email');
            $table->string('website');
            $table->longText('description');
            $table->timestamps();
        });
    }
```
And run this command
```
php artisan migrate
```

### Database Seeding
User seeder from `database > seeders > DatabaseSeeder.php`
```php
\App\Models\User::factory(10)->create();
```
Create 10 users using from `database > factories > UserFactory.php`
```php
public function definition(): array
    {
        return [
            'name' => $this->faker->name(),
            'email' => $this->faker->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
            'remember_token' => Str::random(10),
        ];
    }
```
Run this command for seed
```
php artisan db:seed
```

### Create an Eloquent Model
```
php artisan make:model Listing
```

Modify seeder from `database > seeders > DatabaseSeeder.php` file
```php
// Manually create listing data
Listing::create([
        'title' => 'Laravel Senior Developer',
        'tags'  => 'Laravel, JavaScript',
        'company'  => 'Acme Corp',
        'location'  => 'Boston, MA',
        'email'  => 'email@email.com',
        'website'  => 'https://www.acme.com',
        'description'  => 'Lorem Ipsum is simply dummy text'
    ]);
Listing::create([
        'title' => 'Full-Stack Engineer',
        'tags'  => 'Laravel, JavaScript, API, Backend',
        'company'  => 'Acme Corp',
        'location'  => 'Boston, MA',
        'email'  => 'email@email.com',
        'website'  => 'https://www.acme.com',
        'description'  => 'Lorem Ipsum is simply dummy text'            
    ]);
```

Using this command line for seeding
```
php artisan migrate:refresh --seed
```

### Creating a Factory
```
php artisan make:factory ListingFactory
```

Modify the listing factory from the `database > factories > ListingFactory.php` file
```php
public function definition(): array
    {
        return [
            'title' => $this->faker->sentence(),
            'tags'  => 'Laravel, JavaScript, API, Backend',
            'company'  => $this->faker->company(),
            'email'  => $this->faker->companyEmail(),
            'location'  => $this->faker->city(),
            'website'  => $this->faker->url(),
            'description'  => $this->faker->paragraph(5)
        ];
    }
```

Creating fake data modify `database > seeders > DatabaseSeeder.php` file in the `run` method
```php
use App\Models\Listing;
....
// This line use `run()` mehtod
Listing::factory(6)->create();
```

### Create a Layout & Sections
Create a layout file in `views > layout.blade.php`
```php
<html>
    <body>
        @yield('content)
    </body>
</html>
```

Modify listings and listing blade files from the `views`
```php
@extends('layout')
    @section('content')
    ......
    @endsection
```
Adding the Theme HTML in the blade file, And creating `header.blade.php`, `footer.blade.php`, `search.blade.php`  in the `partials` folder

Include `search.blade.php` in the `listings.blade.php` files using this directives
```php
@include('partials.search)
```

### Route Model Binding
```php
// Single Listing
Route::get('listings/{listing}', function(Listing $listing){
    return view('listings', ['listing' => $listing]);
});
```

### Blade Components
Create `listing-card.blade.php` component file in `database > views > components` folder

```php
{{-- listing props access the variables --}}
@props(['listing'])

{{-- Blade Component --}}
<x-card class="">
.....
</x-card>
```

Modify the `listings.blade.php` file foreach loop content in the `listing-card.blade.php` component
```php
{{-- listing props access the valuables --}}
@props(['listing'])

{{-- Blade Component --}}
<x-card class="">
...
    @foreach($listings as $listing)
        {{-- component @props dirctivs valiable passing --}}
        <x-listing-card :listing="$listing" />
    @endforeach
...
</x-card>
```

Create `card.blade.php` in the components folder for using css class attributes content
```php
{{-- Component Attributes  --}}
<div {{ $attributes->merge(['class' => 'bg-gray-50 border border-gray-200 rounded p-6']) }}>
    {{ $slot }}
</div>
```
Tags Components same as above

### Controllers
```
php artisan make:controller ListingController
```

Modify ListingController class
```php
// Import model
use App\Models\Listing;
...
class ListingController extends Controller
{
    // Show all listing
    public function index() {        
        return view('listings', [            
            'listings' => Listing::all()
        ]);
    }

    // Show single listing
    public function show(Listing $listing) {
        return view('listing', [
            'listing' => $listing
        ]);
    }
}
```

Modify routes file `web.php`
```php
// All Listing
Route::get('/', [ListingController::class, 'index']);
// Single Listing
Route::get('/listings/{listing}', [ListingController::class, 'show']);
```

### Tag Filter
Modify `app > Models > Listing.php` file
```php
class Listing extends Model
{
    use HasFactory;

    public function scopeFilter($query, array $filters)
    {
        if($filters['tag'] ?? false) {
            $query->where('tags', 'like', '%'.request('tag').'%');
        }
    }
}
```

Modify ListingController class
```php
// Import model
use App\Models\Listing;
...
class ListingController extends Controller
{
    // Show all listing
    public function index() {        
        return view('listing.index', [            
            'listings' => Listing::latest()->filter(request(['tag']))->get()
        ]);
    }
}
```

### Search Filter
Modify `app > Models > Listing.php` file
```php
class Listing extends Model
{
    use HasFactory;

    public function scopeFilter($query, array $filters)
    {
        // Tag filter
        if($filters['tag'] ?? false) {
            $query->where('tags', 'like', '%'.request('tag').'%');
        }

        // Search Filter
        if($filters['search'] ?? false){ // It this is not false
            $query->where('title', 'like', '%'.request('search').'%')
            ->orWhere('description', 'like', '%'.request('search').'%')
            ->orWhere('tags', 'like', '%'.request('search').'%');
        }
    }
}
```

### Clockwork Package
Add chrome extension from chrome web store `Clockwork`

After extension add run this command
```
$ composer require itsgoingd/clockwork
```
Open dev tools and click Clockwork tab


### Create a Listing Form
Create a method in ListingController `create`
```php
public function create() {
    return view('listing.create);
}
```

Modify in the route file `web.php`
```php
// Show Create Form
Route::get('/listings/create', [ListingController::class, 'create']);
```

Create a listing form file in the `resources > views > listings` folder
```php
<form method="POST" action="/listings" enctype="multipart/form-data">
    @csrf

    <input type="text" class="border border-gray-200 rounded p-2 w-full" name="title" value="{{old('title')}}"
                    placeholder="Example: Senior Laravel Developer" />
    @error('title')
        <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
    @enderror
</form>
```
            
### Validation & Store Listing
Modify in the route file `web.php`
```php
// Store Listing Data
Route::post('/listings', [ListingController::class, 'store']);
```

Create a listing validation form in ListingController class
```php
// Store listing
public function store(Request $request) {        
    $formFields = $request->validate([
        'title' => 'required',
        'company' => ['required', Rule::unique('listings', 'company')],
        'location' => 'required',
        'website' => 'required',
        'email' => ['required', 'email'],
        'tags' => 'required',
        'description' => 'required',
    ]);

    // Insert to DB
    Listing::create($formFields);

    return redirect('/');
}
```

### Mass Assignment Rule
Modify the `Listing` Model class
```php
protected $fillable = ['title', 'company', 'location', 'email', 'website', 'tags', 'description'];
```

If you do not guard to model class go to `app > Providers > AppServiceProvider.php` class `boot()`
```php
use Illuminate\Database\Eloquent\Model;
....
class ...
    public function boot(): void
    {
        Model::unguard();
    }
```

### Flash Messages
```php
// Return with a flash message set
return redirect('/')->with('message', 'Listing created successfully!');
```
Create `flash-message.blade.php` in the components folder
```php
@if (session()->has('message'))
    <div class="fixed top-0 left-1/2 tranform -translate-x-1/2 bg-laravel text-white px-48 py-3">
        <p>
            {{ session('message') }}
        </p>
    </div>
@endif
```
Put it anywhere in the layout within the footer of the body tag
```php
<x-flash-message>
```

### Alpine.js For Message Removal
Put this script in the header tag
```php
<script src="//unpkg.com/alpinejs" defer></script>
```
Modify `flash-message.blade.php` in the components folder
```php
@if (session()->has('message'))
    <div x-data="{show: true}" x-init="setTimeout(() => show = false, 3000)" x-show="show" class="fixed top-0 left-1/2 tranform -translate-x-1/2 bg-laravel text-white px-48 py-3">
        <p>
            {{ session('message') }}
        </p>
    </div>
@endif
```

### Pagination
Modify the `ListingController` class `index` method
```php
return view('listings.index', [
            'listings' => Listing::latest()->filter(request(['tag', 'search']))->paginate(6)
            // 'listings' => Listing::simplePaginate()
        ]);
```
Modify the `index.blade.php` view file in `app > resources > listing` folder
```php
<x-layout>
    ....
    <div class="">
        {{$listing->links()}}
    </div>
</x-layout>
```
Pagination Service Provider Publish, Choose a number & enter
```
php artisan vendor:publish
```
Generate `vendor/pagination` folder pagination style

Using this style open the `AppServiceProvider.php` class in the `boot()` method
```php
public function boot() {
    // See Documentation
    Paginator::useBootstrapFive(); 
}
```

### File Upload
Open the `create.blade.php` file and modify this code
```php
<form method="POST" action="/listings" enctype="multipart/form-data">
    @csrf

    <div class="mb-6">
        <label for="logo" class="inline-block text-lg mb-2">
            Company Logo
        </label>
        <input type="file" class="border border-gray-200 rounded p-2 w-full" name="logo" />
        @error('logo')
            <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
        @enderror
    </div>
</form>
```
To store image files in the storage public folder `config > filesystems.php` 
```php
'default' => env('FILESYSTEM_DISK', 'public')
```

Modify the `ListingController` class in the `store` method after the `$formFields` array
```php
// Upload Logo and Storage public folder
if ($request->hasFile('logo')) {
    $formFields['logo'] = $request->file('logo')->store('logos', 'public');
}
```

Public folder directly accessible command
```
php artisan storage:link
```

