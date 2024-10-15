


                @yield('content')


___________________

@extends('layouts.header')

  

@section('title', 'Головоломки')

  

@section('content')

    <h1>Головоломки</h1>

  

    <div class="min-h-screen flex flex-col sm:justify-center items-center pt-6 sm:pt-0 bg-gray-100">

        @if (Route::has('login'))

            <div class="fixed top-0 right-0 px-6 py-4 sm:block">

                @auth

                    <!-- <a href="{{ url('/dashboard') }}" class="text-sm text-gray-700 underline">Dashboard</a> -->

                @else

                    <a href="{{ route('login') }}" class="text-sm text-gray-700 underline">Log in</a>

  

                    @if (Route::has('register'))

                        <a href="{{ route('register') }}" class="ml-4 text-sm text-gray-700 underline">Register</a>

                    @endif

                @endif

            </div>

        @endif

    </div>

  

@endsection

            