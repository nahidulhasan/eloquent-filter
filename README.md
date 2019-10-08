# Laravel Eloquent Filter  

  [![Latest Stable Version](https://poser.pugx.org/nahidulhasan/eloquent-filter/v/stable)](https://packagist.org/packages/nahidulhasan/eloquent-filter)
   [![Total Downloads](https://poser.pugx.org/nahidulhasan/eloquent-filter/downloads)](https://packagist.org/packages/nahidulhasan/eloquent-filter)
  [![Latest Unstable Version](https://poser.pugx.org/nahidulhasan/eloquent-filter/v/unstable)](https://packagist.org/packages/nahidulhasan/eloquent-filter)
   [![License](https://poser.pugx.org/nahidulhasan/eloquent-filter/license)](https://packagist.org/packages/nahidulhasan/eloquent-filter)
  


  This simple package helps you filter Eloquent data using query filters.  
  
## Installation  
 
Run  the following command:  
 
```  
$ composer require nahidulhasan/eloquent-filter  
```  
  
  
## Getting started  
  
Use the trait `NahidulHasan\EloquentFilter\Filterable` in your eloquent model.

Create a new class by extending the class `NahidulHasan\EloquentFilter\QueryFilters` and define your custom filters as methods with one argument. Where function names are the filter argument name and the arguments are the value.   
  
Let's assume you want to allow to filter articles data. Please see the following code.  
  
```php 
<?php  

namespace App\Models;

use Illuminate\Database\Eloquent\Model;  
use NahidulHasan\EloquentFilter\Filterable;  
  
class Article extends Model  
{  
	use Filterable; 
	 
	/*
	* The attributes that are mass assignable. 
	*  @var array 
	*/ 
	protected $fillable = ['title', 'body'];
}  
  
```  
  
Create ArticleFilter class extending QueryFilters.  
  
```php  
<?php

namespace App\Filters;  
  
use Illuminate\Database\Eloquent\Builder;  
use NahidulHasan\EloquentFilter\QueryFilters;  
  
class ArticleFilters extends QueryFilters  
{  
  	/*  
	* Filter by Title. 
	* @param $title 
	* @return Builder 
	* @internal param $name 
	* @internal param string $level 
	*/ 
	public function title($title) { 
		return $this->builder->where('title', 'like', '%' .$title.'%'); 
	}
}  
```  
  
With this class we can use the http query string : `title=article_name` or any combination of these filters. It is up to you to define if you will use AND wheres or OR.   

Now in the controller you can apply these filters like as described in below  :    
  
  
```php  
<?php

namespace App\Http\Controllers;  
  
use App\Filters\ArticleFilters;  
use App\Models\Article;  
use Illuminate\Http\Request;  
  
/**  
* Class ArticleController 
* @package App\Http\Controllers 
*/
class ArticleController extends Controller  
{  
	/* 
	* Display a listing of the resource. 
	*  @param ArticleFilters $filters 
	*  @return \Illuminate\Http\Response 
	*  @internal param Request $request 
	*/ 
	public function index(ArticleFilters $filters) 
	{  
		$articles = Article::filter($filters)->paginate(5);  
		
		return view('articles.index', compact('articles'))->with('i', (request()->input('page', 1) - 1) * 5); 
	}
}    
```  

If you go to this link you will get all code: 
https://github.com/nahidulhasan/laravel-eloquent-query-filtering


### Thanks to :  
https://github.com/laracasts/Dedicated-Query-String-Filtering  
  
  
### License  
  
Eloquent-Filter for Laravel is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)

