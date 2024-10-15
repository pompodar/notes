
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class YourController extends Controller
{
    public function __construct()
    {
        $this->middleware('your-middleware-name');
    }

    // Your controller methods here
}
