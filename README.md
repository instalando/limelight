# Limelight   
[![Build Status](https://travis-ci.org/nihongodera/limelight.svg?branch=master)](https://travis-ci.org/nihongodera/limelight)
[![Latest Stable Version](https://poser.pugx.org/nihongodera/limelight/version.svg)](//packagist.org/packages/nihongodera/limelight) 
[![License](https://poser.pugx.org/nihongodera/limelight/license.svg)](//packagist.org/packages/nihongodera/limelight)  
##### A php Japanese language analyzer and parser.  
  - Split Japanese text into individual, full words
  - Find parts of speech for words
  - Find dictionary entries (lemmas) for conjugated words
  - Get readings and pronunciations for words
  - Build fuirgana for words
  - Convert Japanese to romaji (English lettering)

### Quick Guide
  - [Install Limelight](#install-limelight)
  - [Initialize Limelight](#initialize-limelight)
  - [Parse Text](#parse-text)
  - [Get Results](#get-results)
  - [Full Documentation](#full-documentation)
  - [Sources, Contributions, and Contributing](#sources-contributions-and-contributing)

### Install Limelight
##### Requirements
  - php > 5.6

Before installing Limelight, you must install both mecab and the php extension php-mecab on your system.   
!!Important!!   
php-mecab, the MeCab bindings Limelight uses, were updated to version 0.6.0 in Dec. 2015 for php 7 support. The pre-0.6.0 bindings no longer work with the master branch of Limelight. If you are using an older version of php-mecab, please use the [php-mecab_pre_0.6.0](https://github.com/nihongodera/limelight/tree/php-mecab_pre_0.6.0) version.

##### Linux Ubuntu Users
Use the install script included in this repository.    
Download the script:
```
curl -O https://raw.githubusercontent.com/nihongodera/limelight/master/install_mecab_php-mecab.sh
```
Make the file executable:
```
chmod +x install_mecab_php-mecab.sh
```
Execute the script:
```
./install_mecab_php-mecab.sh
```
For information about what the script does, see [here](https://github.com/nihongodera/limelight/wiki/Install-Script).

##### Other Systems

Please see [this page](https://github.com/nihongodera/php-mecab-documentation) to learn more about installing on your system.   
  
Install Limelight through composer.
```
composer require nihongodera/limelight
```

### Initialize Limelight
Make a new instance of Limelight\Limelight.  Limelight takes no arguments.
```php
$limelight = new Limelight();
```

### Parse Text
Use the parse() method on the Limelight object to parse Japanese text.
```php
$results = $limelight->parse('庭でライムを育てています。');
```
The returned object is an instance of Limelight\Classes\LimelightResults.

### Get Results
Get results for the entire text using methods available on [LimelightResults](https://github.com/nihongodera/limelight/wiki/LimelightResults).
```php
$results = $limelight->parse('庭でライムを育てています。');

echo 'Words: ' . $results->words() . "\n";
echo 'Readings: ' . $results->readings() . "\n";
echo 'Pronunciations: ' . $results->pronunciations() . "\n";
echo 'Lemmas: ' . $results->lemmas() . "\n";
echo 'Parts of speech: ' . $results->partsOfSpeech() . "\n";
echo 'Hiragana: ' . $results->toHiragana()->words() . "\n";
echo 'Katakana: ' . $results->toKatakana()->words() . "\n";
echo 'Romaji: ' . $results->toRomaji()->words() . "\n";
echo 'Furigana: ' . $results->toFurigana()->words() . "\n";
```
> **Output:**    
> Words: 庭でライムを育てています。   
> Readings: ニワデライムヲソダテテイマス。   
> Pronunciations: ニワデライムヲソダテテイマス。   
> Lemmas: 庭でライムを育てる。   
> Parts of speech: noun postposition noun postposition verb symbol   
> Hiragana: にわでらいむをそだてています。   
> Katakana: ニワデライムヲソダテテイマス。  
> Romaji: Niwa de raimu o sodateteimasu.   
> Furigana: <ruby><rb>庭</rb><rp>(</rp><rt>にわ</rt><rp>)</rp></ruby>でライムを<ruby><rb>育</rb><rp>(</rp><rt>そだ</rt><rp>)</rp></ruby>てています。   

Note: instead of calling words() you can call get().  
```
$results->toRomaji()->get();
```
   
Get individual words off the LimelightResults object by selecting them by either word or index and using methods available on the returned [LimelightWord](https://github.com/nihongodera/limelight/wiki/LimelightWord) object.
```php
$results = $limelight->parse('庭でライムを育てています。');

$word1 = $results->findIndex(2);

$word2 = $results->findWord('庭');

echo $word1->toRomaji()->word() . "\n";

echo $word2->toFurigana()->word() . "\n";
```
> **Output:**  
> raimu   
> <ruby>庭<rt>にわ</rt></ruby>   
   
Notice that methods on the LimelightResults object and the LimelightWord object follow the same conventions, but LimelightResults methods are plural (word**s**()) while LimelightWord methods are singular (word()).

Note: instead of calling word() you can call get().  
```
$word1->toRomaji()->get();
```
  
Alternatively, loop through all the words on the LimelightResults object using the next() method.
```php
$results = $limelight->parse('庭でライムを育てています。');

foreach ($results->next() as $word) {
    echo $word->word() . ' is a ' . $word->partOfSpeech() . ' read like ' . $word->reading() . "\n";
}
```
> **Output:**    
> 庭 is a noun read like ニワ   
> で is a postposition read like デ   
> ライム is a noun read like ライム   
> を is a postposition read like ヲ   
> 育てています is a verb read like ソダテテイマス   
> 。 is a symbol read like 。   
   
### Full Documentation

Full documentation for Limelight can be found on the [Limelight Wiki page](https://github.com/nihongodera/limelight/wiki).

### Sources, Contributions, and Contributing

The Japanese parsing logic used in Limelight was adapted from Kimtaro's excellent Ruby program [Ve](https://github.com/Kimtaro/ve).  A big thank you to him and all the others who contributed on that project. 
   
Limelight relies heavily on both [MeCab](http://taku910.github.io/mecab/) and [php-mecab](https://github.com/rsky/php-mecab).

Contributors more than welcome.
  
[Top](#contents)
