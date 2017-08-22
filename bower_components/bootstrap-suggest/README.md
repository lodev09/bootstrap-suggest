bootstrap-suggest
============================
A jQuery plugin for bootstrap that pops-up a dropdown in textarea or input textbox.

![demo](demo.jpg "demo")

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
// using users data
$('#comment').suggest('@', {
  data: users,
  map: function(user) {
    return {
      value: user.username,
      text: '<strong>'+user.username+'</strong> <small>'+user.fullname+'</small>'
    }
  }
});

// using ajax: http://lodev09.github.io/bootstrap-suggest/#init-with-ajax-
$('#comment').suggest('@', {
  data: function(q) {
    if (q && q.length > 3) {
      return $.getJSON("users/lookup.json", { q:q });
    }
  },
  map: function(user) {
    return {
      value: user.username,
      text: '<strong>'+user.username+'</strong> <small>'+user.fullname+'</small>'
    }
  }
});
```

## API
http://lodev09.github.io/bootstrap-suggest/#api

## Credits
All bugs, feature requests, pull requests, feedback, etc., are welcome!

[Bootstrap Components](http://getbootstrap.com/components/)

## License
Released under the [Apache License, Version 2.0](http://opensource.org/licenses/Apache-2.0).
See [LICENSE](LICENSE) file.

Copyright 2014, Jovanni Lo / [@lodev09](http://twitter.com/lodev09) / [www.lodev09.com](http://www.lodev09.com "www.lodev09.com") / [lodev09@gmail.com](mailto:lodev09@gmail.com)
