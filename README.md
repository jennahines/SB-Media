# SB-Media
A dead-simple way to do module-based media queries and styles.

## How it works
The two files `no-mq.scss` and `mq.scss` generate two separate stylesheets, one for browsers that do not support Media Queries and one for browsers that do. These files are then included in the `index.html` file using conditional comments to specify which file gets served to which browsers.

**index.html**
```html
<!--[if (lt IE 9) & (!IEMobile)]>
  <link href="c/no-mq.css" rel="stylesheet">
<![endif]-->

<!--[if (gte IE 9) | (IEMobile)]><!-->
  <link href="c/mq.css" rel="stylesheet">
<!--<![endif]-->
```

## The mixin
**_sb-media.scss**
```scss
$no-mq-support: false !default;
$serve-to-nomq-max-width:60em;

@mixin sb-media($query) {
  @if $no-mq-support{
    @if $query < $serve-to-nomq-max-width{
      @content;
    }
  } @else {
    @media ( 'min-width:' + $query ) {
      @content;
    }
  }
}
```
The mixin is pretty straightforward. It defaults to `min-width` media queries and does not support no-mq browsers. To support no-mq browsers, simply add `$no-mq-support: true;` in your `no-mq.scss` file and import the rest of your project like normal.

The $no-mq-threshold hides media queries over a certain value from no-mq browsers. In the mixin above it's set to 60em, or 960px. Any media queries over 60ems will get dropped from no-mq.css

## Example
Here is a default min-width media query. Notice no need to pass min or max-width on the query. Just pass the width value.

**_primary.scss**
```scss
body{
  background-color:red;

  @include sb-media(40em){
    background-color: blue;
  }
}
```

The generated min-width css is below.

**no-mq.css**
```css
body {
  background-color: red;
  background-color: blue;
}
```
**mq.css**
```css
body {
  background-color: red;
}
@media (min-width: 40em) {
  body {
    background-color: blue;
  }
}
```
