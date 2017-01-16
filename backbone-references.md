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
	return song.get(“downloads”) > 10;
});

songs.each(function(song){…});
```