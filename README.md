# PokéAPI

PokéAPI provides data from the Pokémon video games, such as Pokémon, their abilities and their types, in JSON format. This document can be used by software developers or testers. The base URL here is `https://pokeapi.co/api/v2` and it doesn't need any authentication or other setup before being used. Only `GET` requests are available.

PokéAPI asks all users to follow its fair use policy, which includes caching resources locally instead of requesting them again and again.

The following endpoints are covered in this document.

- `GET /pokemon/{id or name}` — find a Pokémon by its ID or name
- `GET /pokemon` — get a paginated list of Pokémon
- `GET /type/{id or name}` — find a type by its ID or name

## Find a Pokémon by ID or Name

**Method:** `GET`

**Path:** `/pokemon/{id or name}`

**Path Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `id or name` | Yes | The ID (for example `132`) or the name (for example `ditto`) of the Pokémon to retrieve |

This endpoint returns the details of one Pokémon. The ID and the name return the same Pokémon: `/pokemon/132` and `/pokemon/ditto` both return Ditto.

**Response Fields:**

The fields `id`, `name`, `base_experience`, `height` and `weight` are self-explanatory. The fields below hold lists of related items.

| Field | Description |
|---|---|
| `abilities` | Array of the Pokémon's abilities. `is_hidden` says whether the ability is hidden, and `ability` holds its name and `url` |
| `types` | Array of the Pokémon's types. Each `type` holds its name and `url` |
| `stats` | Array of the Pokémon's base stats. Each item has a `base_stat` value and a `stat` holding its name and `url` |

The `url` in each of these items points to the resource for that item, for example `https://pokeapi.co/api/v2/type/1/` for the type `normal`.

**Response Example:**

The response also contains other fields, including `base_experience`, `species`, `forms`, `game_indices`, `held_items`, `moves`, `sprites` and `cries`. They are omitted from the example below for brevity, and only one of the two abilities and one of the six stats are shown.

```json
{
  "id": 132,
  "name": "ditto",
  "height": 3,
  "weight": 40,
  "abilities": [
    {
      "is_hidden": false,
      "slot": 1,
      "ability": {
        "name": "limber",
        "url": "https://pokeapi.co/api/v2/ability/7/"
      }
    }
  ],
  "stats": [
    {
      "base_stat": 48,
      "effort": 1,
      "stat": {
        "name": "hp",
        "url": "https://pokeapi.co/api/v2/stat/1/"
      }
    }
  ],
  "types": [
    {
      "slot": 1,
      "type": {
        "name": "normal",
        "url": "https://pokeapi.co/api/v2/type/1/"
      }
    }
  ]
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The Pokémon is returned |
| `404 Not Found` | No Pokémon exists with the given ID or name |

## List Pokémon

**Method:** `GET`

**Path:** `/pokemon`

This endpoint returns a paginated list of Pokémon. Each item in the list has only a `name` and a `url`. To get the full details of a Pokémon, request its `url`, which is the `/pokemon/{id or name}` endpoint described above.

**Query Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `limit` | No | Maximum number of Pokémon to return. If omitted, 20 are returned. |
| `offset` | No | Number of Pokémon to skip before the list begins. If omitted, nothing is skipped (`0`). |

For example, `GET /pokemon?limit=3&offset=3` skips the first 3 Pokémon and returns the next 3 (Pokémon 4, 5 and 6).

**Response Fields:**

| Field | Description |
|---|---|
| `count` | Total number of Pokémon available. This stays the same whatever `limit` and `offset` you use. |
| `next` | URL of the next page of results |
| `previous` | URL of the previous page of results, or `null` on the first page |
| `results` | Array of the Pokémon on this page. Each item has a `name` and a `url` |

The response does not include `limit` or `offset`. Use the `next` and `previous` links to move between pages.

**Response Example:**

The example below is the response for `GET /pokemon?limit=3&offset=3`.

```json
{
  "count": 1351,
  "next": "https://pokeapi.co/api/v2/pokemon?offset=6&limit=3",
  "previous": "https://pokeapi.co/api/v2/pokemon?offset=0&limit=3",
  "results": [
    {
      "name": "charmander",
      "url": "https://pokeapi.co/api/v2/pokemon/4/"
    },
    {
      "name": "charmeleon",
      "url": "https://pokeapi.co/api/v2/pokemon/5/"
    },
    {
      "name": "charizard",
      "url": "https://pokeapi.co/api/v2/pokemon/6/"
    }
  ]
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The list of Pokémon is returned |

## Find a Type by ID or Name

**Method:** `GET`

**Path:** `/type/{id or name}`

**Path Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `id or name` | Yes | The ID (for example `1`) or the name (for example `normal`) of the type to retrieve |

This endpoint returns the details of one Pokémon type, such as `normal`: how the type does against other types, and which Pokémon and moves have it. `/type/1` and `/type/normal` both return the normal type.

**Response Fields:**

The fields `id` and `name` are self-explanatory. The fields below hold lists of related items.

| Field | Description |
|---|---|
| `damage_relations` | How this type does against other types. It has six lists: `no_damage_to`, `half_damage_to`, `double_damage_to`, `no_damage_from`, `half_damage_from` and `double_damage_from`. The `_to` lists are the types that this type attacks, and the `_from` lists are the types that attack this type. Each item is a type with a `name` and a `url`. |
| `pokemon` | Array of the Pokémon that have this type. Each item has a `slot` and a `pokemon` object with its `name` and `url` |
| `moves` | Array of the moves of this type. Each item has a `name` and a `url` |

For example, `double_damage_from` containing `fighting` means that Fighting-type attacks do double damage to the normal type.

**Response Example:**

The response also contains other fields, including `past_damage_relations`, `game_indices`, `generation`, `move_damage_class`, `names` and `sprites`. They are omitted from the example below for brevity. The `pokemon` and `moves` lists contain many items, and only one of each is shown.

```json
{
  "id": 1,
  "name": "normal",
  "damage_relations": {
    "no_damage_to": [
      {
        "name": "ghost",
        "url": "https://pokeapi.co/api/v2/type/8/"
      }
    ],
    "half_damage_to": [
      {
        "name": "rock",
        "url": "https://pokeapi.co/api/v2/type/6/"
      },
      {
        "name": "steel",
        "url": "https://pokeapi.co/api/v2/type/9/"
      }
    ],
    "double_damage_to": [],
    "no_damage_from": [
      {
        "name": "ghost",
        "url": "https://pokeapi.co/api/v2/type/8/"
      }
    ],
    "half_damage_from": [],
    "double_damage_from": [
      {
        "name": "fighting",
        "url": "https://pokeapi.co/api/v2/type/2/"
      }
    ]
  },
  "pokemon": [
    {
      "slot": 1,
      "pokemon": {
        "name": "pidgey",
        "url": "https://pokeapi.co/api/v2/pokemon/16/"
      }
    }
  ],
  "moves": [
    {
      "name": "pound",
      "url": "https://pokeapi.co/api/v2/move/1/"
    }
  ]
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The type is returned |
| `404 Not Found` | No type exists with the given ID or name |