# The cost of `copy.deepcopy`

Before:

```
Total time: 0.22088 s
File: music_sync_utils.py
Function: clean_unicode at line 42

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
    42                                               @profile
    43                                               def clean_unicode(self):
    44         1            2      2.0      0.0          clean_collection = {}
    45       267          294      1.1      0.1          for artist_name in self.collection:
    46       266         2479      9.3      1.1              print 'cleaning artist: ', artist_name
    47       266         3106     11.7      1.4              clean_artist_name = codecs.utf_8_decode(artist_name.encode('utf-8'))[0]
    48       266       116029    436.2     52.5              clean_collection[clean_artist_name] = copy.deepcopy(self.collection[artist_name])
    49       266          554      2.1      0.3              clean_collection[clean_artist_name].albums = []
    50       727          896      1.2      0.4              for album in self.collection[artist_name].albums:
    51       461         4819     10.5      2.2                  print 'cleaning album: ', album.name
    52       461         5956     12.9      2.7                  clean_album_name = codecs.utf_8_decode(album.name.encode('utf-8'))[0]
    53       461        85059    184.5     38.5                  clean_album = copy.deepcopy(album)
    54       461          764      1.7      0.3                  clean_album.name = clean_album_name
    55       461          754      1.6      0.3                  clean_collection[clean_artist_name].albums.append(clean_album)
    56         1          167    167.0      0.1          self.collection = clean_collection
    57         1            1      1.0      0.0          return self
```

After:

```
Total time: 0.017708 s
File: music_sync_utils.py
Function: clean_unicode at line 42

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
    42                                               @profile
    43                                               def clean_unicode(self):
    44         1            2      2.0      0.0          clean_collection = {}
    45       267          262      1.0      1.5          for artist_name in self.collection:
    46       266         1393      5.2      7.9              print 'cleaning artist: ', artist_name
    47       266         2055      7.7     11.6              clean_artist_name = codecs.utf_8_decode(artist_name.encode('utf-8'))[0]
    48       266         1870      7.0     10.6              clean_collection[clean_artist_name] = ArtistItem(artist_name)
    49       727          998      1.4      5.6              for album in self.collection[artist_name].albums:
    50       461         2463      5.3     13.9                  print 'cleaning album: ', album.name
    51       461         3675      8.0     20.8                  clean_album_name = codecs.utf_8_decode(album.name.encode('utf-8'))[0]
    52       461         3580      7.8     20.2                  new_album_item = AlbumItem(clean_album_name, album.file_size)
    53       461          525      1.1      3.0                  if album.drive_file is not None:
    54                                                               new_album_item.drive_file = album.drive_file
    55       461          722      1.6      4.1                  clean_collection[clean_artist_name].albums.append(new_album_item)
    56         1          162    162.0      0.9          self.collection = clean_collection
    57         1            1      1.0      0.0          return self
```