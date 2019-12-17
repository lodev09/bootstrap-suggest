## Usage

### Markup
```html
<div class="form-group">
   <label for="comment">start typing with @</label>
   <textarea class="form-control" rows="5" id="comment"></textarea>
</div>
```

### Data
```javascript
var users = [
  {username: 'lodev09', fullname: 'Jovanni Lo'},
  {username: 'foo', fullname: 'Foo User'},
  {username: 'bar', fullname: 'Bar User'},
  {username: 'twbs', fullname: 'Twitter Bootstrap'},
  {username: 'john', fullname: 'John Doe'},
  {username: 'jane', fullname: 'Jane Doe'},
];
```

### Init
```javascript
$('#comment').suggest('@', {
  data: users,
  map: function(user) {
    return {
      value: user.username,
      text: '<strong>'+user.username+'</strong> <small>'+user.fullname+'</small>'
    }
  }
})
```

#### via Promise or ajax
Ajax is supported by having the data function return a [jqXHR](http://api.jquery.com/Types/#jqXHR) object. The function takes a single parameter containing the mention text string.

```javascript
$('#comment').suggest('@', {
  data: function(q) {
    if (q) {
      return $.getJSON("users/data.json", { q: q });
    }
  },
  // ...
})
```

#### via "provide" function
A `provide` function is provided for you to call on after securing your data (don't return anything to the `data` option function to avoid conflict)
```javascript
$('#comment').suggest('@', {
  data: function(q, provide) {
    if (q) {
      $.getJSON("users/data.json", { q: q }, function(data) {
        // simply call provide and pass on your data!
        provide.call(data);
      });

      // we aren't returning any
    }
  },
  // ...
})
```

### Advanced
#### Add delay search while typing
```javascript
timeout = null;
$('#comment').suggest('@', {
  data: function(q, lookup) {
    var processData = function() {
      $.getJSON("users/lookup.json", { q: q }, function(data) {
        lookup.call(data);
      });
    };

    if (timeout) {
      clearTimeout(timeout);
      timeout = null;
    }

    timeout = setTimeout(processData, 500);
  },
  // ...
})

```

## API

### Initialize
#### Single suggest
`$('#textarea').suggest(key [, options])`
#### Multiple suggest
`$('#textarea').suggest({key: options, key2: options})`

### Options
| option            | type                   | default                          | description                                                                |
| --                | ---                    | ---                              | ---                                                                        |
| data              | array/function         | []                               | an array (of objects or string) or a callback function                     |
| map               | callback               | _undefined_                      | a callback function that is used to map the display and value of each item |
| filter            | object                 | { casesensitive: true, limit: 5} | option that will let you set the filter behaviour                          |
| onshow(e)         | event callback         | _none_                           | triggered when the dropdown is shown                                       |
| onselect(e, item) | event callback         | _none_                           | triggered after selecting the item                                         |
| onlookup(e, item) | event callback         | _none_                           | triggered when searching for an item                                       |
| dropdownClass     | string                 | ""                               | additional class of the dropdown                                           |
| position          | string/object/function | "caret"                          | position of the dropdown menu. string values: _bottom_, _top_, _caret_     |
| endKey            | string                 | ""                               | string key that will append at the selected values                         |
| respectWhitespace | boolean                | true                             | ignore whitespace restriction so whitespace is included in filtering when backspacing and also doesnt require a space between suggest usage|


### Events
| event  | params      | description                          |
| ---    | ---         | ---                                  |
| show   | _none_      | triggered when the dropdown is shown |
| select | object item | triggered after selecting the item   |
| lookup | object item | triggered when searching for an item |

### Methods
```
$('#comments').suggest('@', 'lookup', '...');
```

| method | params    | description                                        |
| ---    | ---       | ---                                                |
| get    | int index | returns an `object` item                           |
| lookup | string q  | search for an item given by the `q` (query) string |
| show   |           | show dropdown                                      |
| hide   |           | hide dropdown                                      |


### Properties
| property | type    | description                     |
| ---      | ---     | ---                             |
| $element | jQuery  | the element                     |
| $items   | array   | items array (jquery objects)    |
| key      | string  | the main key                    |
| isShown  | boolean | `true` if the dropdown is shown |
| query    | string  | current `q` (query)             |