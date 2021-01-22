## Mixins utéis

> Pseudo-elementos ::before e o ::after

```
@mixin pseudo($display: block, $pos: absolute, $content: ''){
    content: $content;
    display: $display;
    position: $pos;
}
```
> Aplicando o Mixin

```
div::after {
    @include pseudo;
    top: -1rem; left: -1rem;
    width: 1rem; height: 1rem;
}
```

> Proporção responsiva - criar elementos escaláveis (normalmente imagens / backgrounds) que precisem manter a proporção
```
@mixin responsive-ratio($x,$y, $pseudo: false) {
    $padding: unquote( ( $y / $x ) * 100 + '%' );
    @if $pseudo {
        &:before {
            @include pseudo($pos: relative);
            width: 100%;
            padding-top: $padding;
        }
    } @else {
        padding-top: $padding;
    }
}
```
> Aplicando o Mixin

```
div {
    @include responsive-ratio(16,9);
}
```
> Estilos de fonte

```
@mixin font-source-sans($size: false, $colour: false, $weight: false, $lh: false) {
    font-family: 'Source Sans Pro', Helvetica, Arial, sans-serif;
    @if $size { font-size: $size; }
    @if $colour { color: $colour; }
    @if $weight { font-weight: $weight; }
    @if $lh { line-height: $lh; }
}
```

> Placeholders

```
@mixin input-placeholder {
    &.placeholder { @content; }
    &:-moz-placeholder { @content; }
    &::-moz-placeholder { @content; }
    &:-ms-input-placeholder { @content; }
    &::-webkit-input-placeholder { @content; }
}
```
> Aplicando o Mixin

```
input, 
textarea { 
    @include input-placeholder {
        color: $grey;
    }
}
```
> Media queries

```
$breakpoints: (
    "phone":        400px,
    "phone-wide": 480px,
    "phablet":    560px,
    "tablet-small": 640px,
    "tablet":    768px,
    "tablet-wide": 1024px,
    "desktop":    1248px,
    "desktop-wide": 1440px
);

@mixin mq($width, $type: min) {
    @if map_has_key($breakpoints, $width) {
        $width: map_get($breakpoints, $width);
        @if $type == max {
            $width: $width - 1px;
        }
        @media only screen and (#{$type}-width: $width) {
            @content;
        }
    }
}
```

> Aplicando o Mixin

```
.site-header {
    padding: 2rem;
    font-size: 1.8rem;
    @include mq('tablet-wide') {
        padding-top: 4rem;
        font-size: 2.4rem;
    }
}
```
> Aceleração de hardware para animações

```
@mixin hardware($backface: true, $perspective: 1000) {
    @if $backface {
        backface-visibility: hidden;
    }
    perspective: $perspective;
}
```

> Centralizar
```
@mixin push--auto {
    margin: { 
        left: auto;
        right: auto;
    }
}
```
> Aplicando o Mixin

```
.container {
 @include push--auto;
}

```
