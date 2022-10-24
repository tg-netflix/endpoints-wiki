# **Endpoint /browse**

<br>

| Links naar mock JSONs | Wordt gebruikt bij |
|-|-|
|[Browse Categories + Banner](./browse_categories_banner.mock.json)| Banner en eerste 3 lanes|
|[Browse Categories](./browse_categories.mock.json)| Overige movie lanes & grid view|

<br>

## Wordt gebruikt voor:
<br>

- ## **Discovery page:**
    #### **Initiële load request:** <sup>_* (Wordt opgevraagd bij pagina load of vanaf login scherm)_</sup>
    - Banner
    - Page 1 van categorieën gespecificeerd in params. **(eerste zichtbare lanes)**

    <br>

    #### **Requests na initiële load:** <sup>_* (losse requests)_</sup>
    - Page 1 van meerdere in params gespecificeerde categorieën.
     **(nieuwe lanes bij pagina scroll)**
    - Page 2 van één in params gespecificeerde categorie bij het scrollen in een lane. **(meer films van 1 lane)**
    
    <br>
- ## **Films page**
    #### **Initiële load:** <sup>_* (Wordt opgevraagd bij pagina load)_</sup>
    - Page 1 van categorieën gespecificeerd in params. **(eerste zichtbare lanes)**

    <br>

    #### **Requests na initiële load:**
    - Page 1 van in params gespecificeerde categorieën, afhankelijk van door.
     **(nieuwe lanes bij pagina scroll)**
    - Page 2 van één in params gespecificeerde categorie bij het scrollen in een lane. **(meer films van 1 lane)**
        
    - #### **Wanneer gebruiker een genre kiest: (grid view)**
        - Page 1 van één specifieke categorie
        - Page 2 en mogelijk daarna nog mee pages **(nieuwe films bij pagina scroll)**

<br>

### *LET OP: 
- TMDB geeft soms films terug met een release date later dan de dag van vandaag (afhankelijk van categorie / filter). `release_date.lte=yyyy-mm-dd` met de datum van vandaag lost dat probleem op bij de TMDB endpoints die deze optie hebben. Geldt als het goed is alleen voor discover bij sorteren op release_date.desc.
- We willen geen films waar de titel niet leesbaar is vanwege een ander alfabet. Niet bij alle films is de title een vertaling van de original_title. `region=US,EN,...` lost dat op. Toepasbaar op meerdere TMDB endpoints. Graag filtreren  op **`US`** indien mogelijk.
<br>

# Parameters:
- ### `Banner` - Vraagt banner op
- ### `Categories` - Vraagt een lijst aan films
- ### `Page` - (bepaalt welke page aan films wordt opgevraagd. geldt alleen voor categories)

<br>

| Variable | Data type | |Standaard waarde|
-|-|-|-:
`categories`    | string    | verplicht | -
`banner`        | boolean   | optioneel | false
`page`          | integer   | optioneel | 1

<br>

## json per parameter:
<br>

- ## **Banner**
    Voorbeeld: `banner=true`
    - Wordt naar verwachting niet aangevraagd op een klein mobiel toestel.
    - Wordt naar verwachting maar één keer opgevraagd.

    <br>

    ```json
    "banner": {   // de banner film is een willekeurige film uit de popular array
            "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",  // movie afbeelding
            "description": "lorem ipsum", //movie beschrijving
            "id": 297761,                 // movie id
            "logo": "?",                  // de eerste afbeelding van fanart.tv
            "title": "Suicide Squad",     // (niet original_title! Deze is in andere talen)
            "trailer": "6JnN1DmbqoU",     // de eerste video met type:trailer en site:Youtube uit TMDB get videos
            "age_certificate": "12"       // de "certification" van  "iso_3166_1": "US" TMDB movie get release dates
        }
    ```
    <br>

- ## **Categories**
    Categories wordt gebruikt om zowel "categoriën", als genres op te vragen. Omdat alleen genres een eigen id hebben in TMDB, gaan we strings gebruiken als parameters. Dit betekent dat de backend de strings zelf moet omzetten naar id's om de genres op te vragen.

    Verschillende hoeveelheid categories worden opgevraagd op verschillende momenten.

    <br>

    | Keuze opties (geen genres) | TMDB endpoint | + extra params 
    | - | - | -
    |popular     | movie/popular |
    |top_rated   | movie/top_rated | 
    |latest      | discover | <p>sort_by=release_date.desc</p><p>release_date.lte=_**{huidige datum}**_</p>
    |disney      | discover | with_company=2
    |classic     | discover | <p>primary_release_year=_**{randomize tussen 1980 - 1999}**_</p> | 
    |my_list    | (*Afhankelijk van de list functionaliteit*)
    <br>


    | Keuze opties (genres) | _TMDB bron: `discover` with_genres={genre id}_
    :- | :-
    action | 
    adventure
    animation
    comedy
    crime
    documentary
    drama
    family
    fantasy
    history
    horror
    music
    mystery
    romance
    science_fiction
    tv_movie
    thriller
    war
    action
    adventure
    western
    <br>

    *De lijst met alle genres en genre_id's zijn te verkrijgen met TMDB get /genre/movie/list

    <br>


    ## Responce voorbeeld met parameters: 
    >`categories=popular,top_rated,action`

    - Om de volgende 20 movies aan te vragen voor een genre, wordt een geselecteerde genre gbruikt met daarbij `page=2`
    >`categories=popular&page=2`

    ```json
    "categories":[              // Array met alle opgevraagde categories
        {
            "page": 1,          // Page nummer
            "name": "Popular",  // Categorie naam
            "movies": [         // Alle (20) movies van de page 
                {   // Movie data 
                    "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                    "id": 297761,
                    "title": "Suicide Squad"
                },
                {
                    "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                    "id": 297761,
                    "title": "Suicide Squad"
                },
                {
                    // Meer movies...
                }
            ]
        },
        {
            "page": 1,
            "name": "Top Rated",
            "movies": [   
                {
                    "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                    "id": 297761,
                    "title": "Suicide Squad"
                },
                { 
                    "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                    "id": 297761,
                    "title": "Suicide Squad"
                },
                {
                    // Meer movies...
                }
            ]
        },
        {
            "page": 1,
            "name": "Action",
            "movies": [
                {
                    "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                    "id": 297761,
                    "title": "Suicide Squad"
                },
                { 
                    "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                    "id": 297761,
                    "title": "Suicide Squad"
                },
                {
                    // Meer movies...
                }
            ]
        },
        {
            // Meer categories...
        }
    ]
    ```
    <br>


# **Response per request**
<br>

- ## **Request: Categories & banner** (Discover page)
    In de eerste request die wordt gemaakt willen wij categories en de banner tegelijkertijd ontvangen. Gebruik voor de banner een willekeurige film uit de "popular" categorie nadat die is opgevraagd. Mocht alleen de banner worden opgevraagd, zou de popular lijst alsnog opgevraagd moeten worden om daaruit 1 willekeurige film te kiezen.

    <br>

    ### **URL voorbeeld:**
    > `netflixbackend.com/initial/categories=popular,top_rated,latest&banner=true`

    ```json
    {
        "banner": {
            "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
            "description": "lorem ipsum",
            "id": 297761,
            "logo": "?",
            "title": "Suicide Squad",
            "trailer": "6JnN1DmbqoU",
            "age_certificate": "12"
        },
        "categories":[
            {
                "page": 1,
                "name": "Popular",
                "movies": [
                    {
                        "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                        "id": 297761,
                        "title": "Suicide Squad"
                    },
                    {
                        "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                        "id": 297761,
                        "title": "Suicide Squad"
                    },
                    {
                        // meer movies... 
                    }
                ]
            },
            {
                "page": 1,
                "name": "Top Rated",
                "movies": [
                    {
                        "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                        "id": 297761,
                        "title": "Suicide Squad"
                    },
                    { 
                        "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                        "id": 297761,
                        "title": "Suicide Squad"
                    },
                    {
                        // meer movies... 
                    }
                ]
            },
            {
                "page": 1,
                "name": "Latest",
                "movies": [
                    {
                        "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                        "id": 297761,
                        "title": "Suicide Squad"
                    },
                    { 
                        "backdrop_path": "/rr7E0NoGKxvbkb89eR1GwfoYjpA.jpg",
                        "id": 297761,
                        "title": "Suicide Squad"
                    },
                    {
                        // meer movies... 
                    }
                ]
            }
        ]
    }
    ```

    <br><hr>
