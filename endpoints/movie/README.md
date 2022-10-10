# **Endpoint /movie**

<br>

| Links naar mock JSONs | Wordt gebruikt bij |
|-|-|
|[Movie Details](./movie_details.mock.json)| Kleine + grote modal vanaf moviecard|
|[Movie Similar](./movie_similar.mock.json)| Grote modal vanaf kleine modal|
|[Movie Details + Similar](movie_details_similar.mock.json)| Grote modal vanaf banner |

<br>

## Wordt gebruikt voor:

- ## Kleine modal + Grote modal vanaf movie card:
    **Eerste request** <sup>* (Wordt opgevraagd wanneer de kleine modal getoond wordt. Dit gebeurt na een delay van 0.5 seconde onhover)</sup>
    - Details van 1 film op basis van doorgegeven movie id.

    <br>

    **Tweede request** <sup>* (Wordt opgevraagd wanneer de gebruiker klikt op de "more info" knop in de kleine modal)</sup>

    - Similar movies op basis van de doorgegeven movie id.

    <br>


- ## Grote modal vanaf banner
    **<sup>* (Wordt opgevraagd wanneer de gebruiker klikt op de more "info knop" in de banner)</sup>**
    - Details van 1 film op basis van doorgegeven movie id.
    - Similar movies op basis van de doorgegeven movie id.

    <br>

# Parmeters: (`nog bespreken`)

- `Id` - Geeft movie id door.
- `Details` - Vraagt film details op voor de modals.
- `Similar` - Vraagt similar movies aan. Altijd alleen van page 1 (max 6 films).

| Variable | Data type || Standaard waarde |
-|-|-|-:
| `id`      | integer | verplicht | -
| `details` | boolean | optioneel | true
| `similar` | boolean | optioneel | false

<br>

## json per parameter:

- ## Details
    Voorbeeld: `details=true`

    <br>

```json
{
    "id": 550,                  // movie id
    "trailer": "6JnN1DmbqoU",   // de eerste video met type:trailer en site:Youtube uit TMDB get videos
    "logo": "een afbeelding url",    // de eerste afbeelding van fanart.tv (Geen prioriteit)
    "genres": [                 // Kunnen meerdere genres zijn
        "Drama",
        "Action",
    ],
    "title": "Fight Club",
    "description": "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion.",         // movie overview
    "keywords": [               // altijd de eerste 3 uit TMDB movie get keywords
        "based on novel or book",
        "support group",
        "dual identity"
    ]
    "release_year": 1999,       // het jaar van de "release_date" uit TMDB movie
    "runtime": 139,             // film tijdsduur
    "age_certificate": 16,      // de "certification" van  "iso_3166_1": "US" TMDB movie get release dates
    "actors": [ // array met name van de cast (eerste max. 10) die een key "character" heeft uit TMDB movie get credits
        "Brad Pitt",
        "Edward Norton",
        "Helena Bonham Carter",
        "Meat Loaf"
        // meer namen
    ],
    "writers": [ // array met name van de cast met "job":"screenplay" uit TMDB movie get credits
        "Een Naam",
        // etc...
    ],
    "directors": [ // array met name van de cast met "job":"director" uit TMDB movie get credits
        "Een Naam",
        // etc...
    ]
}
```

<br>

- ## Similar
    Voorbeeld: `similar=true`

    <br>

```json
{
    "similar": [
        {
            "backdrop_path": "/heKW7bP7m2MtvQaGXtH13LfQ8Q8.jpg",    // movie afbeelding
            "description": "lorem ipsum",                           // movie overview
            "id": 2926,                   // movie id
            "title": "Suicide Squad",
            "runtime": 139,               // film tijdsduur
            "release_year": 2020,         // het jaar van de "release_date" uit TMDB movie
            "age_certificate": 18,        // de "certification" van "iso_3166_1": "US" TMDB movie get release dates
        },
        {
            // meer movies (max 6)
        }
    ]
}
```

<br>

# Responce per request:

- ## Request grote modal vanaf banner

### URL voorbeeld
> `netflixbackend.com/movie?id=550&similar=true`

of

> `netflixbackend.com/movie?id=550&details=true&similar=true`

```json
{
    "id": 550,
    "trailer": "6JnN1DmbqoU",
    "logo": "een afbeelding url"
    "genres": [
        "Drama",
        "Action",
    ],
    "title": "Fight Club",
    "description": "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion.",
    "keywords": [
        "based on novel or book",
        "support group",
        "dual identity"
    ],
    "release_year": 1999,
    "runtime": 139,
    "age_certificate": 16,
    "actors": [
        "Brad Pitt",
        "Edward Norton",
        "Helena Bonham Carter",
        "Meat Loaf"
    ],
    "writers": [
        "Een Naam",
    ],
    "directors": [
        "Een Naam",
    ],
    "similar": [
        {
            "backdrop_path": "/heKW7bP7m2MtvQaGXtH13LfQ8Q8.jpg",
            "description": "lorem ipsum",
            "id": 2926,
            "title": "Suicide Squad",
            "runtime": 139,
            "release_year": 2020,
            "age_certificate": 18,
        },
        {
            "backdrop_path": "/heKW7bP7m2MtvQaGXtH13LfQ8Q8.jpg",
            "description": "lorem ipsum",
            "id": 2926,
            "title": "Suicide Squad",
            "runtime": 139,
            "release_year": 2020,
            "age_certificate": 18,
        },
        {
            "backdrop_path": "/heKW7bP7m2MtvQaGXtH13LfQ8Q8.jpg",
            "description": "lorem ipsum",
            "id": 2926,
            "title": "Suicide Squad",
            "runtime": 139,
            "release_year": 2020,
            "age_certificate": 18,
        },
        {
            // meer movies (max 6)
        }
    ]
}
```

 <br>


