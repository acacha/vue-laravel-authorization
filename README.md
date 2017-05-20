# vue-laravel-authorization

This is a guide of how to use Laravel Authorization (https://laravel.com/docs/5.4/authorization) system using Laravel permission package ( https://github.com/spatie/laravel-permission ).

# Configuration 

## Backend

Install laravel-permission package. See https://github.com/spatie/laravel-permission#installation .

## Front-end

We want to use Laravel permission package roles and permissions in Javascript with Conditional rendering (https://vuejs.org/v2/guide/conditional.html) using the following:

```javascript
<h1 v-if="Laravel.user.permission['manage users']">You have permission to manage users</h1>
<h1 v-else>You don't have permission to manage users</h1>

```

Example with roles:

```javascript
<h1 v-if="Laravel.user.roles['manager']">You have the role manager</h1>
<h1 v-else>You don't have the role manager</h1>
```

# Security note

Never depends **only** in Javascript for security because Javascript code could be tampered by final users. So only use this technique when you also use backend security.
