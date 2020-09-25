+++
date = "2018-11-24"
title = "Getting Snipcart custom data attributes into WordPress + Elementor Pricing Table"
tags = ["WordPress", "Elementor", "Snipcart"]
categories = ["tech"]
summary = "After shunning pagebuilders in WordPress long enough, I gave Elementor a try for a landing page layout and found it quite useful. I also realized how terribly far behind Gutenberg is! So, Elementor got me 90% of the way there, as is often the case in WordPress - simple, until it isn't! Custom code in the theme's `functions.php` got the last piece of the puzzle."
+++

After shunning pagebuilders in WordPress long enough, I gave Elementor a try for a landing page layout and found it quite useful. I also realized how terribly far behind Gutenberg is!

So, Elementor got me 90% of the way there, as is often the case in WordPress - simple, until it isn't!

Custom code in the theme's `functions.php` got the last piece of the puzzle:

```php
function wp2static_snipcart_callback($buffer) {
   $html_output = $buffer;

   $dom = new DOMDocument();
   $dom->loadHTML($html_output);

   $finder = new DomXPath($dom);
   $classname="elementor-price-table__button";
   $nodes = $finder->query("//*[contains(@class, '$classname')]");

   $yearly_buy_button = $nodes->item(1);

   $yearly_buy_button->setAttribute('class','elementor-price-table__button elementor-button elementor-size-md snipcart-add-item');                                                                
   $yearly_buy_button->setAttribute('data-item-name','PowerPack, unlimited sites, yearly');
   $yearly_buy_button->setAttribute('data-item-id','powerpack-unlimited-yearly');
   $yearly_buy_button->setAttribute('data-item-url','https://securityandperformance.com/');
   $yearly_buy_button->setAttribute('data-item-price','99.00');
   $yearly_buy_button->setAttribute('data-item-payment-interval','Year');
   $yearly_buy_button->setAttribute('data-item-payment-interval-count','1');

   $unlimited_buy_button = $nodes->item(2);

   $unlimited_buy_button->setAttribute('class','elementor-price-table__button elementor-button elementor-size-md snipcart-add-item');                                                             
   $unlimited_buy_button->setAttribute('data-item-name','PowerPack, unlimited sites, lifetime');
   $unlimited_buy_button->setAttribute('data-item-id','powerpack-unlimited-lifetime');
   $unlimited_buy_button->setAttribute('data-item-url','https://securityandperformance.com/');
   $unlimited_buy_button->setAttribute('data-item-price','299.00');

   return $dom->saveHTML();
}

add_filter( 'the_content', 'wp2static_snipcart_callback' );

```
// duplicate with wp_styles for loaded CSS in frontend
function insite_inspect_scripts() {
    global $wp_scripts;
    echo PHP_EOL.'<!-- Script Handles: ';
    foreach( $wp_scripts->queue as $handle ) :
        echo $handle . ' || ';
    endforeach;
    echo ' -->'.PHP_EOL;
}

add_action( 'wp_print_scripts', 'insite_inspect_scripts' );

```



[back](/)
