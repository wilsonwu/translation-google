# translation-google 
<a href="https://www.npmjs.com/package/translation-google"><img src="https://img.shields.io/npm/dt/translation-google.svg" alt="Downloads"></a>
<a href="https://www.npmjs.com/package/translation-google"><img src="https://img.shields.io/npm/v/translation-google.svg" alt="Version"></a>
<a href="https://www.npmjs.com/package/translation-google"><img src="https://img.shields.io/npm/l/translation-google.svg" alt="License"></a>

A Google Translate API:

## Features 

- Translate all languages that Google Translate supports.
- Support different area (Support Chinese user, in China Mainland could set the suffix to `'cn'` to make it work)

## Live Demo with Nodejs
[![Edit translation-google-demo](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/youthful-napier-2bi1c?fontsize=14&hidenavigation=1&theme=dark)

## Install 

```
npm install translation-google
```

## Usage

From automatic language detection to English:

``` js
var translate = require('translation-google');

translate('This is Google Translate', {to: 'zh-cn'}).then(res => {
    console.log(res.text);
    //=> 这是Google翻译
    console.log(res.from.language.iso);
    //=> en
}).catch(err => {
    console.error(err);
});
```

For Chinese user, try this:

``` js
var translate = require('translation-google');

translate.suffix = 'cn'; // also can be 'fr', 'de' and so on, default is 'com'

translate('This is Google Translate', {to: 'zh-cn'}).then(res => {
    console.log(res.text);
    //=> 这是Google翻译
    console.log(res.from.language.iso);
    //=> en
}).catch(err => {
    console.error(err);
});
```

Sometimes, the API will not use the auto corrected text in the translation:

``` js
translate('This is Google Translat', {from: 'en', to: 'zh-cn'}).then(res => {
    console.log(res);
    console.log(res.text);
    //=> 这是Google翻译
    console.log(res.from.text.autoCorrected);
    //=> false
    console.log(res.from.text.value);
    //=> This is Google [Translate]
    console.log(res.from.text.didYouMean);
    //=> true
}).catch(err => {
    console.error(err);
});
```

## API

### translate(text, options)

#### text

Type: `string`

The text to be translated

#### options

Type: `object`

##### from

Type: `string` Default: `auto`

The `text` language. Must be `auto` or one of the codes/names (not case sensitive) contained in [languages.js](https://github.com/matheuss/google-translate-api/blob/master/languages.js)

##### to

Type: `string` Default: `en`

The language in which the text should be translated. Must be one of the codes/names (not case sensitive) contained in [languages.js](https://github.com/matheuss/google-translate-api/blob/master/languages.js).

##### raw

Type: `boolean` Default: `false`

If `true`, the returned object will have a `raw` property with the raw response (`string`) from Google Translate.

### Returns an `object`:

- `text` *(string)* – The translated text.
- `from` *(object)*
  - `language` *(object)*
    - `didYouMean` *(boolean)* - `true` if the API suggest a correction in the source language
    - `iso` *(string)* - The [code of the language](https://github.com/matheuss/google-translate-api/blob/master/languages.js) that the API has recognized in the `text`
  - `text` *(object)*
    - `autoCorrected` *(boolean)* – `true` if the API has auto corrected the `text`
    - `value` *(string)* – The auto corrected `text` or the `text` with suggested corrections
    - `didYouMean` *(booelan)* – `true` if the API has suggested corrections to the `text`
- `raw` *(string)* - If `options.raw` is true, the raw response from Google Translate servers. Otherwise, `''`.

Note that `res.from.text` will only be returned if `from.text.autoCorrected` or `from.text.didYouMean` equals to `true`. In this case, it will have the corrections delimited with brackets (`[ ]`):

``` js
translate('This is Google Translat').then(res => {
    console.log(res.from.text.value);
    //=> This is [Google Translate]
}).catch(err => {
    console.error(err);
});
```
Otherwise, it will be an empty `string` (`''`).

## License

MIT
