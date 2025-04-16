# Laravel N+1 Problem vs Eager Loading

This repository demonstrates the difference between the **N+1 problem** and **Eager Loading** in Laravel using Eloquent relationships.

---

## 🔍 N+1 Problem Example

```php
// Fetching main records
$posts = Post::all();

// Fetching related records for each post
foreach ($posts as $post) {
    echo $post->title;

    // This line causes one query per post (N queries total)
    foreach ($post->comments as $comment) {
        echo $comment->body;
    }
}

Total queries = 1 (posts) + N (comments)
In the N+1 problem, the main query runs once to fetch the primary table (e.g., posts), but the related query (e.g., comments) runs once for every record, resulting in N extra queries.


## Laravel Eager Loading

// Fetching main and related records in one go
$posts = Post::with('comments')->get();

// Loop only iterates over already fetched records
foreach ($posts as $post) {
    echo $post->title;

    foreach ($post->comments as $comment) {
        echo $comment->body;
    }
}
Total queries = 1 (posts) + 1 (comments)

In eager loading, both the main and related data are fetched in just 2 queries, regardless of how many records there are. 
This eliminates repeated queries during iteration, which improves performance.


## Use tools like Laravel Debugbar to spot N+1 issues during development.

composer require barryvdh/laravel-debugbar --dev
