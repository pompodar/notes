<?php

  

namespace Database\Seeders;

  

// use Illuminate\Database\Console\Seeds\WithoutModelEvents;

use Illuminate\Database\Seeder;

 use App\Models\Article;

  

class DatabaseSeeder extends Seeder

{

    /**

     * Seed the application's database.

     */

    public function run()

     {

         Article::factory()->times(5000)->create();

     }

}