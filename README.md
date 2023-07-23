# LaraGigs
Traversy Media - Learn the Laravel PHP framework from scratch by building a job listing application with Laravel 10 and MySQL.

## Task Description
This project is segregated step by step task follow this video link
[Traversy Media](https://www.youtube.com/watch?v=MYyJ4PuL4pY&t=10661s)

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

### Edit Listing Form
Modify in the route file `web.php`
```php
// Show Edit Form
Route::get('/listings/{listing}/edit', [ListingController::class, 'edit'])->middleware('auth');

// Edit Submit to Update
Route::put('/listings/{listing}', [ListingController::class, 'update'])->middleware('auth');
```

Create a method in ListingController `edit`
```php
public function edit(Listing $listing)
{
    // dd($listing);
    return view('listings.edit', ['listing' => $listing]);
}
```

Create a listing edit form file in the `resources > views > listings` folder
```php
 <x-card class="p-10 max-w-lg mx-auto mt-24">
    <header class="text-center">
        <h2 class="text-2xl font-bold uppercase mb-1"> Edit Gig</h2>
        <p class="mb-4">Edit: {{$listing->title}}</p>
    </header>
    
    <form method="POST" action="/listings/{{$listing->id}}" enctype="multipart/form-data">
        @csrf
        @method('PUT')
    
        <div class="mb-6">
            <label for="company" class="inline-block text-lg mb-2">Company Name</label>
            <input type="text" class="border border-gray-200 rounded p-2 w-full" name="company" value="{{$listing->company}}" />
            @error('company')
                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
            @enderror
        </div>
        ......
        ......
        <div class="mb-6">
            <button class="bg-laravel text-white rounded py-2 px-4 hover:bg-black">
                Update Gig
            </button>
        
            <a href="/" class="text-black ml-4"> Back </a>
            </div>
        </form>
    </x-card>
</x-layout>
```

Create an `update` method in ListingController
```php
// Update Listing
public function update(Request $request, Listing $listing)
{
    // dd($request->all());

    // Make sure logged in user is owner
    if($listing->user_id != auth()->id()){
        abort(403, 'Unauthorised Action');
    }

    $formFields = $request->validate([
        'title' => 'required',
        'company' => 'required',
        'location' => 'required',
        'website' => 'required',
        'email' => ['required', 'email'],
        'tags' => 'required',
        'description' => 'required',
    ]);

    // Upload Logo and Storage public folder
    if ($request->hasFile('logo')) {
        $formFields['logo'] = $request->file('logo')->store('logos', 'public');
    }

    // Using Method
    $listing->update($formFields);

    // Return with flash message
    return back()->with('message', 'Listing updated successfully!');
}
```

### Delete Listing
Modify in the view file `resources > listings> show.blade.php`
```php
<x-card class="mt-4 p-2 flex space-x-6">
    .....
    <form method="POST" action="/listings/{{ $listing->id }}">
        @csrf
        @method('DELETE')

        <button class="text-red-500">
            <i class="fa-solid fa-trash"></i> Delete
        </button>
    </form>
</x-card>
```

Modify the route file `web.php`
```php
// Delete Listing
Route::delete('/listings/{listing}', [ListingController::class, 'destroy'])->middleware('auth');
```

Create a `delete` method in ListingController
```php
// Delete Listing
public function destroy(Listing $listing)
{
    // Make sure logged in user is owner
    if($listing->user_id != auth()->id()){
        abort(403, 'Unauthorised Action');
    }

    // Delete
    $listing->delete();
    return redirect('/')->with('message', 'Listing deleted successfully!');
}
```

### User Registration
Modify the route file `web.php`
```php
// Show Register/Create Form
Route::get('/register', [UserController::class, 'create'])->middleware('guest');

// Create New User
Route::post('/users', [UserController::class, 'store']);
```

Create a controller with this command and Import this class in `web.php`
```php
php artisan make:controller UserController 
```

Create a `create` method in UserController
```php
public function destroy(Listing $listing)
{
    // Show Register/Create Form
    public function create()
    {
        return view('users.register');
    }
}
```

Create a `register.blade.php` file in view `user` folder
```php
<x-layout>
    <div class="bg-gray-50 border border-gray-200 p-10 rounded max-w-lg mx-auto mt-24">
        <header class="text-center">
            <h2 class="text-2xl font-bold uppercase mb-1">
                Register
            </h2>
            <p class="mb-4">Create an account to post gigs</p>
        </header>

        <form method="POST" action="/users">
            @csrf

            <div class="mb-6">
                <label for="name" class="inline-block text-lg mb-2">
                    Name
                </label>
                <input type="text" class="border border-gray-200 rounded p-2 w-full" name="name" value="{{old('name')}}" />
                @error('name')
                    <p class="text-red-500 text-xs mt-1">{{$message}}</p>
                @enderror
            </div>
            ......
            ......
            <div class="mb-6">
                <button type="submit" class="bg-laravel text-white rounded py-2 px-4 hover:bg-black">
                    Sign Up
                </button>
            </div>

            <div class="mt-8">
                <p>
                    Already have an account?
                    <a href="/login" class="text-laravel">Login</a>
                </p>
            </div>
        </form>
    </div>
</x-layout>
```

Create a `store` method in UserController for create user
```php
public function store(Request $request)
{
    $formFields = $request->validate([
        'name' => ['required', 'min:3'],
        'email' => ['required', 'email', Rule::unique('users', 'email')],
        'password' => 'required|confirmed|min:6'
    ]);

    // Hash Password
    $formFields['password'] = bcrypt($formFields['password']);

    // Create User
    $user = User::create($formFields);

    // Login with auth helper
    auth()->login($user);

    return redirect('/')->with('message', 'User created and logged in!');
}
```

### Auth Link
Open `layout.blade.php` from `resources > views > components` folder
```php
<nav class="flex justify-between items-center mb-4">
    <a href="/"><img class="w-24" src="{{ asset('images/logo.png') }}" alt="" class="logo" /></a>
    <ul class="flex space-x-6 mr-6 text-lg">
        @auth
                        
            <li>
                <span class="font-bold uppercase">
                    Welcome {{auth()->user()->name}}
                </span>
            </li>
            <li>
                <a href="/listings/manage" class="hover:text-laravel"><i class="fa-solid fa-arrow-right-to-bracket"></i>
                    Manage Listings</a>
            </li>
    
            <li>
                <form class="inline" method="POST" action="/logout">
                    @csrf
                    <button type="submit">
                        <i class="fa-solid fa-door-closed"></i> Logout
                    </button>
                </form>
            </li>

        @else

            <li>
                <a href="/register" class="hover:text-laravel"><i class="fa-solid fa-user-plus"></i> Register</a>
            </li>
            <li>
                <a href="/login" class="hover:text-laravel"><i class="fa-solid fa-arrow-right-to-bracket"></i>
                    Login</a>
            </li>

        @endauth
    </ul>
</nav>

<main>
    {{ $slot }}
</main>
```


### Log User Out
Open `web.php` route file
```php
// Log User Out
Route::post('/logout', [UserController::class, 'logout'])->middleware('auth');
```

Create a `logout` method in UserController
```php
public function logout(Request $request)
{
    auth()->logout();
    $request->session()->invalidate();
    $request->session()->regenerateToken();

    return redirect('/')->with('message', 'You have been logged out!');
}
```

### Login User
Open `web.php` route file
```php
// Show Login Form
Route::get('/login', [UserController::class, 'login'])->name('login')->middleware('guest');
```

Create a `login.blade.php` view file in user folder
```php
<x-layout>
    <div class="bg-gray-50 border border-gray-200 p-10 rounded max-w-lg mx-auto mt-24">
        <header class="text-center">
            <h2 class="text-2xl font-bold uppercase mb-1">
                Login
            </h2>
            <p class="mb-4">Log into your account to post gigs</p>
        </header>

        <form method="POST" action="/users/authenticate">
            @csrf

            <div class="mb-6">
                <label for="email" class="inline-block text-lg mb-2">Email</label>
                <input type="email" class="border border-gray-200 rounded p-2 w-full" name="email"  value="{{old('email')}}" />
                @error('email')
                    <p class="text-red-500 text-xs mt-1">{{$message}}</p>
                @enderror
            </div>

            <div class="mb-6">
                <label for="password" class="inline-block text-lg mb-2">
                    Password
                </label>
                <input type="password" class="border border-gray-200 rounded p-2 w-full" name="password" />
                @error('password')
                    <p class="text-red-500 text-xs mt-1">{{$message}}</p>
                @enderror
            </div>

            <div class="mb-6">
                <button type="submit" class="bg-laravel text-white rounded py-2 px-4 hover:bg-black">
                    Sign In
                </button>
            </div>

            <div class="mt-8">
                <p>
                    Don't have an account?
                    <a href="/register" class="text-laravel">Register</a>
                </p>
            </div>
        </form>
    </div>
</x-layout>
```

Create a `authenticate` in `UserController`
```php
public function authenticate(Request $request)
{
    $formFields = $request->validate([
        'email' => ['required', 'email'],
        'password' => 'required'
    ]);

    if (auth()->attempt($formFields)) {
        $request->session()->regenerate();

        return redirect('/')->with('message', 'You are now logged in!');
    }

    return back()->withErrors(['email' => 'Invalid Credentials'])->onlyInput('email');
}
```

### Auth and Guest Middleware
Open `Authenticate.php` file from `App > Http > Middleware` folder and modify `redirectTo` method
```php
protected function redirectTo(Request $request): ?string
{
    return $request->expectsJson() ? null : route('login');
}
```

### Relationship
Relationship between `Users` and `Listings` table 

Open the `...create_listing_table` migration file
```php
public function up(): void
{
    Schema::create('listings', function (Blueprint $table) {
        $table->id();
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->string('title');
        .....
    });
}
```

Open the `Listing.php` model in relation to the user
```php
// Relationship To User
public function user(){
    return $this->belongsTo(User::class, 'user_id');
}
```

Open the `User.php` model in relation to the listing
```php
// Relationship With Listing
public function listings(){
    return $this->hasMany(Listing::class, 'user_id');
}
```

