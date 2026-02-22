# API_ONBOARDING_REPORT.md
## Laboratorio #3 — Sistemas y Tecnologías Web | UVG 2026

## 1. Resumen de la API (esta tabla fue realizada con IA)
| Campo        | Detalle                                      |
|--------------|----------------------------------------------|
| **Nombre**   | PokéAPI                                      |
| **Base URL** | `https://pokeapi.co/api/v2`                  |
| **Tipo de Auth** | Ninguna (API pública y gratuita)         |
| **Rate Limit** | ~100 solicitudes/día por IP sin caché; con caché del cliente no hay límite definido |
| **Documentación** | https://pokeapi.co/docs/v2              |
| **Versión**  | v2                                           |
| **Formato**  | JSON                                         |

## 2. Tabla de Endpoints (esta tabla fue realizada con IA)
| # | Método | URL | Query Params | Headers | Status Esperado | Status Obtenido |
|---|--------|-----|--------------|---------|-----------------|-----------------|
| 1 | GET | `/pokemon` | `limit=10&offset=0` | `Accept: application/json` | 200 OK | 200 OK |
| 2 | GET | `/pokemon/{id}` | — | `Accept: application/json` | 200 OK | 200 OK |
| 3 | GET | `/pokemon/{name}` | — | `Accept: application/json` | 200 OK | 200 OK |
| 4 | GET | `/type/fire` | — | `Accept: application/json` | 200 OK | 200 OK |
| 5 | GET | `/pokemon` | `limit=10&offset=10` | `Accept: application/json` | 200 OK | 200 OK |
| 6 | GET | `/ability` | `limit=10&offset=0` | `Accept: application/json` | 200 OK | 200 OK |
| 7 | GET | `/ability/blaze` | — | `Accept: application/json` | 200 OK | 200 OK |
| 8 | GET | `/type/dragon` | — | `Accept: application/json` | 200 OK | 200 OK |
| 9 | GET | `/pokemon/pokemon-inexistente-99999` | — | `Accept: application/json` | 404 Not Found | 404 Not Found |
| 10 | GET | `/pokemon` | `limit=-1&offset=abc` | `Accept: application/json` | 400 Bad Request | 400 Bad Request |
| 11 | GET | `/pokemon/1` | — | `Authorization: Bearer token_invalido` | 401 Unauthorized | 200 OK* |
| 12 | GET | `/admin/users` | — | `Accept: application/json` | 403/404 | 404 Not Found |

> *PokéAPI es pública, por lo que ignora el header Authorization. En APIs con auth real, retornaría 401.

## 3. Evidencia de Respuestas
### Respuesta Exitosa #1 — GET /pokemon?limit=10&offset=0

**Request:**
GET https://pokeapi.co/api/v2/pokemon?limit=10&offset=0
Accept: application/json

**Response (200 OK):**
{
  "count": 1302,
  "next": "https://pokeapi.co/api/v2/pokemon?offset=10&limit=10",
  "previous": null,
  "results": [
    { "name": "bulbasaur", "url": "https://pokeapi.co/api/v2/pokemon/1/" },
    { "name": "ivysaur",   "url": "https://pokeapi.co/api/v2/pokemon/2/" },
    { "name": "venusaur",  "url": "https://pokeapi.co/api/v2/pokemon/3/" },
    { "name": "charmander","url": "https://pokeapi.co/api/v2/pokemon/4/" },
    { "name": "charmeleon","url": "https://pokeapi.co/api/v2/pokemon/5/" },
    "..."
  ]
}

### Respuesta Exitosa #2 — GET /pokemon/pikachu

**Request:**
GET https://pokeapi.co/api/v2/pokemon/pikachu
Accept: application/json

**Response (200 OK):**
{
  "id": 25,
  "name": "pikachu",
  "base_experience": 112,
  "height": 4,
  "weight": 60,
  "abilities": [
    {
      "ability": { "name": "static", "url": "https://pokeapi.co/api/v2/ability/9/" },
      "is_hidden": false,
      "slot": 1
    }
  ],
  "types": [
    {
      "slot": 1,
      "type": { "name": "electric", "url": "https://pokeapi.co/api/v2/type/13/" }
    }
  ],
  "stats": [
    { "base_stat": 35, "stat": { "name": "hp" } },
    { "base_stat": 55, "stat": { "name": "attack" } },
    { "base_stat": 40, "stat": { "name": "defense" } },
    { "base_stat": 50, "stat": { "name": "special-attack" } },
    { "base_stat": 50, "stat": { "name": "special-defense" } },
    { "base_stat": 90, "stat": { "name": "speed" } }
  ]
}

### Respuesta Exitosa #3 — GET /ability/blaze

**Request:**
GET https://pokeapi.co/api/v2/ability/blaze
Accept: application/json

**Response (200 OK):**
{
  "id": 66,
  "name": "blaze",
  "is_main_series": true,
  "effect_entries": [
    {
      "effect": "When this Pokémon has 1/3 or less of its HP remaining, its fire-type moves do 1.5× as much damage.",
      "language": { "name": "en" }
    }
  ],
  "pokemon": [
    { "pokemon": { "name": "charmander" } },
    { "pokemon": { "name": "charmeleon" } },
    { "pokemon": { "name": "charizard" } }
  ]
}

### Respuesta Fallida #1 — 404 Not Found

**Request:**
GET https://pokeapi.co/api/v2/pokemon/pokemon-inexistente-99999
Accept: application/json

**Response (404 Not Found):**
Not Found

> La API retorna un cuerpo de texto plano "Not Found" con status HTTP 404 cuando el recurso no existe.

### Respuesta Fallida #2 — 404 (recurso administrativo inaccesible)

**Request:**
GET https://pokeapi.co/api/v2/admin/users
Accept: application/json
Authorization: Bearer fake_token_xyz_12345

**Response (404 Not Found):**
Not Found

> El endpoint `/admin/users` no existe en la PokéAPI. En APIs con sistema de permisos, este tipo de request retornaría 403 Forbidden si el recurso existe pero el usuario no tiene acceso.


- La API no requiere autenticación, por lo que los escenarios de 401/403 se simulan conceptualmente.
- Todos los recursos siguen el patrón RESTful: `/api/v2/{recurso}` y `/api/v2/{recurso}/{id|nombre}`.
- La paginación utiliza los parámetros `limit` y `offset`, y los campos `next` / `previous` en la respuesta facilitan la navegación entre páginas.
