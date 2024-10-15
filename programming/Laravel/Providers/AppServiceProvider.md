<?php

  

namespace App\Providers;

  

use Illuminate\Support\ServiceProvider;

  

class AppServiceProvider extends ServiceProvider

{

    /**

     * Register any application services.

     */

    public function register(): void

    {

        $this->app->bind(SearchRepository::class, EloquentSearchRepository::class);

    }

  

    /**

     * Bootstrap any application services.

     */

    public function boot(): void

    {

        //

    }

}