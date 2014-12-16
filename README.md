# Advanced SASS Component
An elegant way to write sandbox SASS component.
Compile only what you need, and offer a possibility to skin.

## Installation

    bower install advanced-sass-component --save

## The sass component concept
Imagine two similar components like this

```css
.buttonLogin {
    display: block;
    width: 100%;
    padding: 10px;
    color: #000;
    background: yellow;
    border-color: #000;
}

.buttonReset {
    display: block;
    width: 100%;
    padding: 10px;
    color: #000;
    background: green;
    border-color: #000;
}
```

You can see a simple improvement

```css
.buttonLogin,
.buttonReset {
    display: block;
    width: 100%;
    padding: 10px;
    color: #000;
    border-color: #000;
}

.buttonLogin {
    background: yellow;
}

.buttonReset {
    background: green;
}
```

Now imagine this in sass

```css
%button {
    display: block;
    width: 100%;
    padding: 10px;
    color: #000;
    border-color: #000;
}

.buttonLogin {
    @extend %button;
    background: yellow;
}

.buttonReset {
    @extend %button;
    background: green;
}
```

That's great but not really modular. Imagine to write it like this:

```css
%button {
    display: block;
    width: 100%;
    padding: 10px;
    color: #000;
    border-color: #000;
}

%button-skin-default {
    background: yellow;
}

@mixin button($selector: '.button', $skin: 'default') {
    #{$selector} {
        @extend %button;
        @extend %button-skin-#{$skin} !optional;
    }
}

@include button('.buttonLogin');

%button-skin-green {
    background: green;
}
@include button('.buttonReset', 'green');
```

This is totaly modular and your component is totaly skin friendly

## How to use Advanced Sass Component

### @ASCExtend
To refine the code ASCExtend mixin extend the component default properties and the skin parameter

Instead of this:

```scss
@extend %button;
@extend %button-skin-#{$skin} !optional;
```

You only write this:
```scss
@include ASCExtend('%button', $skin);
```

### @ASCRegister
In case of standard plugin or white label project, this is realy important to declare a component and add the possibility to configure it later.

__BE CAREFULL__ You need to use exposition mixin to use this fonctionnality

This mixin add a map to `$ASComponents` global variable.

It take 3 parameters
* componentName: ex. 'button'
* variationName: ex. '.buttonLogin' (it become the selector if selector is undefined in the next parameter)
* optionMap: _default: ()_ the configuration of the componentVariation (don't forget to escape the dollar in key for example: `$selector` will be `selector`)

```scss
@include ASCRegister('button', '.buttonLogin');
@include ASCRegister('button', '.buttonReset', (skin: 'green'));
```

### @ASCConfig
This method allow the possibility to configure your previously registered component.
For example `.buttonReset` become like the default button, you will write this:

```scss
@include ASCConfig('button', '.buttonReset', (skin: 'default'));
```
__BE CAREFULL__ You need to use exposition mixin to use this fonctionnality
