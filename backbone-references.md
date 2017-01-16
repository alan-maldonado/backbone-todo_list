# Backbone Cheat Sheet

# Models

## Creating Models

```javascript
var Song = Backbone.Model.extend({
  idAttribute: 'songId',
  urlRoot: '/api/songs',
  defaults: {
    downloads: 0
  },

  validate: function(attrs){
    if (!attrs.title)
      return 'Title is required.';
    }
});

var song = new Song({ songId: 1, title: 'Blue in Green' });
```

## Working with Attributes

```javascript
song.set('genre', 'Jazz');
var genre = song.get('genre');
song.unset('genre');
var hasGenre = song.has('genre');
song.clear();
```

## Validation

```javascript
var isValid = song.isValid();
var lastError = song.validationError;
```

## Syncing with the Server

```javascript
song.fetch({
  success: function(){…}
  error: function(){…}
});
song.save({}, {
  success: function(){…}
  error: function(){…}
});
song.destroy({
  success: function(){…}
  error: function(){…}
});
```

# Collections

## Creating Collections

```javascript
var Songs = Backbone.Collection.extend({
  model: Song,
  url: '/api/songs'
});

var songs = new Songs([
  new Song({ title: 'Song 1' }),
  new Song({ title: 'Song 2' })
]);
```

## Working with Collections

```javascript
songs.add(new Song({…});

var firstSong = songs.at(0);
var songWithIdC1 = songs.get('c1');

songs.remove(firstSong);
songs.push(new Song({…});

var lastSong = songs.pop();
var jazzSongs = songs.where({ genre: 'Jazz' });
var firstJazzSong = songs.findWhere({ genre: 'Jazz' });
var popularSongs = songs.filter(function(song){
	return song.get('downloads') > 10;
});

songs.each(function(song){…});
```

# Views

## Creating Views

```javascript
var SongView = Backbone.View.extend({
  tagName: 'span',
  className: 'song',
  id: '1234',
  attributes: {
    'data-genre': 'Jazz'
  },

  render: function() {
    this.$el.html(…);
    return this;
  }
});

var songView = new SongView();
$('#container').html(songView.render().$el);
```

## Passing Data to Views

```javascript
var song = new Song({…});
var songView = new SongView({ model: song });

var SongView = Backbone.View.extend({
  render: function() {
    this.$el.html(this.model.get('title'));
    return this;
  }
});
```

## Handling DOM Events

```javascript
var SongView = Backbone.View.extend({
  events: {
    'click .bookmark': 'onClickBookmark'
  },

  onClickBookmark: function(e){
    var $targetElement = $(e.target);
    //…
  }
});
```

## Handling Model events

```javascript
var SongView = Backbone.View.extend({
  initialize: function(){
    this.model.on('change', this.render, this);
  }
});
```

## Handling Collection Events

```javascript
var SongView = Backbone.View.extend({
  initialize: function(){
    this.model.on('add', this.add, this);
  }
  add: function(model) {
    //…
  }
});
```

## Templating

```javascript
var SongView = Backbone.View.extend({
  render: function(){
    var source = $('#songTemplate').html();
    var template = _.template(source);
    this.$el.html(template(this.model.toJSON());
    return this;
  }
});
```

```html
<script type=“text/html” id=“songTemplate”>
  <%= title %>
  <% if (downloads > 1000) { %>
    Popular
  <% } %>
</script>
```

# Events

## Binding and Triggering Custom Events

```javascript
var person = {
  walk: function(){
    this.trigger('walking', { speed: 10 });
  }
};

_.extend(person, Backbone.Events);

person.on('walking', function(e) {
  console.log(e.speed);
});

person.off('walking');
```
