# PokéAPI

PokéAPI provides data from the Pokémon video games, such as Pokémon, their abilities and their types, in JSON format. This document can be used by software developers or testers. The base URL here is `https://pokeapi.co/api/v2` and it doesn't need any authentication or other setup before being used. Only `GET` requests are available.

PokéAPI asks all users to follow its fair use policy, which includes caching resources locally instead of requesting them again and again.

The following endpoints are covered in this document.

- `GET /pokemon/{id or name}` — find a Pokémon by its ID or name

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
| `404 Not Found` | This Pokemon with mentioned name or Id wasn't found | 

