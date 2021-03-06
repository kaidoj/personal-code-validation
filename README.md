# Estonian person identification code validation

Validation of Estonian natural person identification code according to the 
standard [EVS 585:2007 Personal code. Structure](https://www.evs.ee/products/evs-585-2007).

Yes, the author actually bought the standard and worked it through. 
This package was done because author is very strict and needed a 
validation that states it fallows an official standard and it can 
be tested if there is any doubt.

## Install 

````bash
$ composer require raigu/personal-code-validation
````

## Usage 

```php
use Raigu\PersonalCodeValidation;

$validation = PersonalCodeValidation::create('47101010033');

// Check validity
if ($validation->valid()) {
    echo 'Valid personal identification code';
} else {
    echo 'Invalid personal identification code';  
}
```

If used in declarative programming and the id code is not known at the
construction phase then use closure to defer code fetching:

```php
$validation = PersonalCodeValidation::fromClosure(function () {
    $code = '';
    
    // here implement code fetching

    return $code;
});
```

There are some factory methods for creating Test Doubles for subject under test.

```php
$validation = PersonalCodeValidation::stub(); // not known and cared if it is valid or not
assert(is_bool($validation->valid()));

$validation = PersonalCodeValidation::fakeTrue(); // always valid
assert($validation->valid());

$validation = PersonalCodeValidation::fakeFalse(); // never valid
assert($validation->valid());
```

## Command Line Interface

```bash
$ php ./bin/validate.php
```

Will give codes from stdin and print out if it is valid or not.


Example:
 
```bash
$ echo 47101010033 | php ./bin/validate.php
47101010033...OK
```


You can validate codes in a file this way:

```bash
$ cat codes.txt | php ./bin/validate.php 
00000000000...Invalid!
47101010033...OK!
```

`codes.txt` must contain persona codes, one code per line.


## Testing

```bash
$ composer test
```

How much of code is covered by tests?
```bash
composer coverage
```

## License

Licensed under [MIT](LICENSE)
