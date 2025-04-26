## Source for hermits.fyi

This includes all the frontend for [hermits.fyi](https://hermits.fyi), the backend is just a simple wrapper around [solr](https://solr.apache.org/).

## Search tools

### Phrases
If you want to search for the phrase "mumbo for mayor" you can enclose it in quotes:
```
"mumbo for mayor"
```
This will only display videos where the whole phrase was used.

You can filter the results by removing words/phrases from the query:
```
"mumbo for mayor" -poster
```
This will display videos with the phrase "mumbo for mayor", where the word "poster" wasn't used.

### Title
You can filter the results by the title:
```
title:hermitcraft
```
This will only display videos that have the word "hermitcraft" in the title.

If you want to search for a phrase you can enclose it in quotes:
```
title:"decked out"
```
You can do this multiple times, this will only display videos that have both "hermitcraft" and "decked out" in the title:
```
title:hermitcraft title:"decked out"
```

You can also remove words/phrases from the title, this will only display videos that have "decked out" in the title, but not "hermitcraft":
```
title:-hermitcraft title:"decked out"
```

### Length
You can filter the results by length. This will only display videos that are over 30 minutes in length:
```
length:>1800
```

This will only display videos that are less than an hour in length:
```
length:<3600
```

You can combine these to filter between a range of lengths:
```
length:>1800 length:<3600
```

Alternately you can search for specific length videos:
```
length:1284
```
Will display videos that are exactly 1284 seconds in length (sometimes the YouTube display is off by one second).

### Upload date
You can filter the results by upload date. This will display videos that were uploaded on January 31st 2023 or earlier:
```
date:<2023-01-31
```

This will display videos that were uploaded on June 1st 2023 or later:
```
date:>2016-06-01
```

You can combine these to filter between a range of dates:
```
date:>2016-06-01 date:<2023-01-31
```
Note: the date must be formatted exactly like this (<year>-<month>-<day>) with the month being 2 digits long, add a 0 if necessary.

### Sorting
You can sort the videos:
```
sort:date
```
This will sort the videos by upload date in descending order.

```
sort:length
```
This will sort the videos by their length in descending order.


If you want to sort them in ascending order you can use:
```
sort:<date
sort:<length
```

There is also random sorting:
```
sort:random
```
This will sort the videos in a random order (there will be duplicates if you scroll long enough!).

## Get the subtitles and build a search engine yourself

You can download all the subtitles for all the videos indexed [here](http://hermits.fyi/subs.tar.zst). It's updated once per day.

One file per video, the name of the file is the id of the video.

Example of using them:
```python
from pickle import load
from sys import argv

# Get filename from first argument
file = argv[1]

# Open the file (as binary) and load the pickle content
with open(file, 'rb') as f:
    pickle_content = load(f)

# Print out the video information
print('Channel: {}'.format(pickle_content['channel']))
print('Upload date: {}'.format(pickle_content['upload_date']))
print('Title: {}'.format(pickle_content['title']))
print('Video length: {}'.format(pickle_content['video_length']))
print()

# Print out the first 10 subtitles
for subtitle in pickle_content['subs'][:10]:
    print('Time: {:3}, Text: {}'.format(subtitle['start'], subtitle['text']))
```

Run with python:
```shell
python example.py 4YpvMzpD1lM.pickle
python example.py TSl5D4QOA7E.pickle
```
