---
title: PHP Trait method returning <code>self</code>
slug: php-traits-self-return
date: 2023-11-05
year: 2023
status: published
---

Today I stumbled upon a little _gotcha_ when writing traits in PHP. Namely, I wanted re-usable 
methods that would return `self`, so that I could chain method calls. However, I found that 
the Intelephense LSP (for VS Code, and Sublime Text) would not recognise the return type as
the class that was using the trait, but instead as the trait. 

So for the code of:

```php
trait Foo {
    public function bar(): self {
        return $this;
    }
}

class Bar {
    use Foo;
}

(new Bar)->bar());
```

Intelephense would report that the return type of `bar()` was `Foo`, the trait, not `Bar`, the class. 
This is because the trait is not aware of the class that is using it, and so cannot return the 
correct type. Oddly enough returning `static` in the trait fixes the issue for Intelephense.

However, PHPStorm IDE doesn't seem to have a problem understanding the return type of `self` in
traits, and resolves it as the union of all classes that are using the trait. I'm pretty sure this 
type of static analysis can be done with LSP's as well, so it just seems like a limitation in Intelephense. 

Either way, if I want my code to be compatible with both, I'll have to use `static` instead of `self`.