<h1 align="center">MalScraper</h1>

<p align="center">
  <a href="http://forthebadge.com/" target="_blank">
    <img src="http://forthebadge.com/images/badges/built-with-love.svg"/>
  </a>
</p>

<p align="center">
  <a href="https://standardjs.com/" target="_blank">
    <img src="https://cdn.rawgit.com/feross/standard/master/badge.svg" />
  </a>
</p>

<p align="center">
  <a href="https://travis-ci.org/Kylart/MalScraper" target="_blank">
    <img src="https://travis-ci.org/Kylart/MalScraper.svg?branch=master" alt="Build Status">
  </a>
  <a href="https://codecov.io/gh/Kylart/MalScraper" target="_blank">
    <img src="https://codecov.io/gh/Kylart/MalScraper/branch/master/graph/badge.svg" alt="Codecov" />
  </a>
  <a href="https://opensource.org/licenses/MIT" target="_blank">
    <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License">
  </a>
</p>

At the moment, _MalScraper_ allows one to:
* Gather information about all the anime being releases in a season.
* Gather anime-related news (include light-novels, manga, films...). 160 news available.
* Make an anime search.
* Get different information for this anime.
* Get only the best result for an anime search.
* Access the full official MyAnimeList API (includes search, add, update and delete from your user watch lists).

_MalScraper_ is being developed mainly for [_KawAnime_](https://github.com/Kylart/KawAnime) but anyone can use it for
 its own purpose.

Any contribution is welcomed.

Tables of content:
* [Installation](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#installation)
* [Use](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#use)
* [Methods](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#methods)
- * [getInfoFromName()](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#getinfofromname())
- * [getInfoFromURL()](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#getinfofromurl())
- * [getResultsFromSearch()](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#getresultsfromsearch())
- * [getWatchListFromUser()](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#getwatchlistfromuser())
- * [getSeason()](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#getseason())
- * [getNewsNoDetails()](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#getnewsnodetails())
* [Data models](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#data-models)
* [Contributing](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#contributing)
* [License](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#license)

## Installation
```npm install --save mal-scraper```

## Use
```javascript
const malScraper = require('mal-scraper')
```

## Methods

### getInfoFromName()

| Parameter | Type | Description |
| --- | --- | --- |
| Name | string | The name of the anime to search, the best match corresponding to that name will be returned |

Usage example:

```js
const malScraper = require('mal-scraper');

const name = 'Sakura Trick';

malScraper.getInfoFromName(name)
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```

Returns: A [Anime data model](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#anime-data-model) object

### getInfoFromURL()

This method is faster than `getInfoFromName()` as it only make one HTTP request

| Parameter | Type | Description |
| --- | --- | --- |
| URL | string | The URL to the anime |

Usage example:

```js
const malScraper = require('mal-scraper');

const url = 'https://myanimelist.net/anime/20047/Sakura_Trick';

malScraper.getInfoFromURL(url)
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```

Returns: A [Anime data model](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#anime-data-model) object (same as `getInfoFromName()`)

### getResultsFromSearch()

| Parameter | Type | Description |
| --- | --- | --- |
| query | string | The search query |

Usage example:

```js
const malScraper = require('mal-scraper');

const query = 'sakura';

malScraper.getResultsFromSearch(query)
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```

Returns: An array of a maximum length of 10 containing [Search result data model](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#search-result-data-model) objects

### Get a user watch list
```javascript
const malScraper = require('mal-scraper')

const username = 'Kylart'

// Get you an object containing all the entries with status, score... from this user's watch list
malScraper.getWatchListFromUser(username)
  .then((data) => console.log(data))
  .catch((err) => console.log(err))
```

### Get seasonal information
```javascript
const malScraper = require('mal-scraper')

const year = 2017
const season = 'fall'

malScraper.getSeason(year, season)
  // `data` is an object containing the following keys: 'TV', 'OVAs', 'Movies'
  .then((data) => console.log(data))
  .catch((err) => console.log(err))
```

### Get news
```javascript
const malScraper = require('mal-scraper')

malScraper.getNewsNoDetails()
  // `data` is an array containing 160 entries
  .then((data) => console.log(data))
  .catch((err) => console.log(err))
```

### Use the official MyAnimeList API
> This requires a valid MyAnimeList account

```javascript
const malScraper = require('mal-scraper')

const api = new malScraper.officialApi({
  username: 'my_super_username',
  password: 'my_super_secret_password'
})

const name = 'Sakura Trick'
const id = 20047  // ID of this anime on MyAnimeList

// This api offers three methods: search, actOnList and checkCredentials
api.checkCredentials()
  .then((data) => console.log(data))
  .catch((err) => console.log(err))

// type can be either 'anime' or 'manga'
api.search(type = 'anime', name)
  .then((data) => console.log(data))
  .catch((err) => console.log(err))

api.actOnList({
  support: 'anime', // Can be either 'anime' or 'manga'
  action: 'add' // Can be either 'add', 'update' or 'delete'
}, id, {
  // All the opts are as described at this link: https://myanimelist.net/modules.php?go=api#animevalues
  status: 1,
  score: 10
})
```

## Data models

### Anime data model

> You should treat all properties as possibly undefined/empty, the only guaranteed properties are `title`, `type` and `id`

| Property | Type | Description |
| --- | --- | --- |
| title | string | The title of the anime |
| synopsis | string | The synopsis of the anime |
| picture | string | The URL of the cover picture of the anime |
| characters | array | An array of [Character data model](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#character-data-model) objects |
| staff | array | An array of [Staff data model](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#staff-data-model) objects |
| trailer | string | URL to the embedded video |
| englishTitle | string | The english title of the anime |
| synonyms | string | A list of synonyms of the anime title (other languages names, related ovas/movies/animes) separated by commas, like "Sakura Trick, Sakura Trap" |
| type | string | The type of the anime, can be either `TV`, `OVA`, `Movie` or `Special` |
| episodes | string | The number of aired episodes |
| status | string | The status of the anime (whether it is airing, finished...) |
| aired | string | The date from which the airing started to the one from which it ended, this property will be empty if one of the two dates is unknown | 
| premiered | string | The date of when the anime has been premiered |
| broadcast | string | When the anime is broadcasted |
| producers | array | An array of the anime producers |
| studios | array | An array of the anime producers |
| source | string | On what the anime is based on (e.g: based on a manga...) |
| genres | array | An array of the anime genres (Action, Slice of Life...) |
| duration | string | Average duration of an episode (or total duration if movie...) |
| rating | string | The rating of the anime (e.g: R18+..), see the [List of possible ratings](https://github.com/ParadoxOrigins/MalScraper/blob/master/README.md#list-of-possible-ratings) |
| score | string | The average score |
| scoreStats | string | By how many users this anime has been scored, like "scored by 255,693 users" |
| ranked | string | The rank of the anime |
| popularity | string | The popularity of the anime |
| members | string | How many users are members of the anime (have it on their list) |
| favorites | string | Count of how many users have this anime as favorite |
| id | number | The unique identifier of the anime |
| url | string | the URL to the page |

#### List of possible ratings

Anime ratings can be either:

* `G - All Ages`
* `PG - Children`
* `PG-13 - Teens 13 or older`
* `R - 17+ (violence & profanity)`
* `R+ - Mild Nudity`
* `Rx - Hentai`

#### Staff data model

| Property | Type | Description |
| --- | --- | --- |
| link | string | Link to the MAL profile of this person |
| name | string | Their name and surname, like `Surname, Name` |
| role | string | The role this person has/had in this anime (Director, Sound Director...) |

#### Character data model

| Property | Type | Description |
| --- | --- | --- |
| link | string | Link to the MAL profile of this character |
| name | string | Their name and surname, like `Surname, Name` |
| role | string | The role this person has/had in this anime (Main, Supporting...) |
| seiyuu | object | An object containing additional data about who dubbed this character |
| seiyuu.link | string | Link to the MAL profile of who dubbed this character |
| seiyuu.name | string | Their name and surname, like `Surname, Name` | 

### Search result data model

| Property | Type | Description |
| --- | --- | --- |
| id | number | The unique identifier of this result |
| type | string | The type of the result (e.g: anime...) |
| name | string | The title of the anime |
| url | string | The URL to the anime |
| image_url | string | URL of the image |
| thumbnail_url | string | URL of the thumbnail image |
| es_score | number | ? | 
| payload | object | An object containing additional data about the anime |
| payload.media_type | string | The type of the anime, can be either `TV`, `Movie`, `OVA` or `Special` |
| payload.start_year | number | The year the airing of the anime started |
| payload.aired | string | The date from which the airing started to the one from which it ended |
| payload.score | string | The average score given to this anime |
| payload.status | string | The current status of the anime (whether it is still airing, finished...) |

## Contributing
1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request.

## License
MIT License

Copyright (c) Kylart
