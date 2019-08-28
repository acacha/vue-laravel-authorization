# vue-laravel-authorization

This is a guide of how to use Laravel Authorization (https://laravel.com/docs/5.4/authorization) system using Laravel permission package ( https://github.com/spatie/laravel-permission ).

# Configuration 

## Backend

Install laravel-permission package. See https://github.com/spatie/laravel-permission#installation.

Add Laravel user object to Javascript with the following javascript snippet in your html header:

In 5.4.23 and later you can use meta tags:

```html
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <meta name="user" content="{{ Auth::user() }}">
```

and in your Vue component do something like this:

```javascript
export default {
    data() {
        return {
            user: JSON.parse(document.head.querySelector('meta[name="user"]').content)
        }
    },
// rest of component
}
```

If you use Laravel default Laravel scaffolding (see command make:auth https://laravel.com/docs/5.8/authentication) you have to only done litle changes tho the existing files.

You can also publish only permissions and/or roles if you don't want to expose all user object to javascript with something like:

```html
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <meta name="user" content="{{ Auth::user() }}">
```

Larave permission package don't expose by default roles and permissions in Json serialization of user object so you have to apply the following changes to App\User class (apply a Trait):

```php
Trait ExposePermissions
   ....
   
    
   /**
     * Get all user permissions.
     *
     * @return bool
     */
    public function getAllPermissionsAttribute()
    {
        return $this->getAllPermissions();
    }
    
     /**
     * Get all user permissions in a flat array.
     *
     * @return array
     */
    public function getCanAttribute()
    {
        $permissions = [];
        foreach (Permission::all() as $permission) {
            if (Auth::user()->can($permission->name)) {
                $permissions[$permission->name] = true;
            } else {
                $permissions[$permission->name] = false;
            }
        }
        return $permissions;
    }
```

At the trait to you User Model and appends the attributes to json:

```php
/**
     * The accessors to append to the model's array form.
     *
     * @var array
     */
    protected $appends = ['all_permissions','can']
```

## Front-end

We want to use Laravel permission package roles and permissions in Javascript with Conditional rendering (https://vuejs.org/v2/guide/conditional.html) using the following:

```javascript
<h1 v-if="Laravel.user.can['manage users']">You have permission to manage users</h1>
<h1 v-else>You dont have permission to manage users</h1>

```

# Security note

Never depends **only** in Javascript for security because Javascript code could be tampered by final users. So only use this technique when you also use backend security.
