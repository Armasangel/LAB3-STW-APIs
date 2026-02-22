# HTTPie Commands - PokéAPI
## Laboratorio #3 | Sistemas y Tecnologías Web | 24714


## Configuración de Variables

# Exportar variables de entorno
export BASE_URL="https://pokeapi.co/api/v2"
export RESOURCE="pokemon"
export ID="1"
export QUERY="pikachu"
export LIMIT="10"
export OFFSET="0"
export TOKEN_VALID=""         # LA PokéAPI no requiere token
export TOKEN_INVALID="Bearer fake_token_xyz_12345"

## Happy Path

### Listar recurso principal (Pokémon)
http GET "$BASE_URL/$RESOURCE" \
  Accept:application/json \
  Content-Type:application/json \
  limit==$LIMIT \
  offset==$OFFSET
> **Status esperado:** 200 OK — Retorna lista paginada de Pokémon.

### Detalle por ID (Bulbasaur - ID: 1)
http GET "$BASE_URL/$RESOURCE/$ID" \
  Accept:application/json \
  Content-Type:application/json \
  User-Agent:"HTTPieLab3/1.0"
> **Status esperado:** 200 OK — Retorna datos completos de Bulbasaur.

### Búsqueda por nombre (Pikachu)
http GET "$BASE_URL/$RESOURCE/$QUERY" \
  Accept:application/json \
  Content-Type:application/json \
  User-Agent:"HTTPieLab3/1.0"
> **Status esperado:** 200 OK — Retorna datos de Pikachu.

---

### Comando 4 - Filtro avanzado por tipo (Fuego)
```bash
http GET "$BASE_URL/type/fire" \
  Accept:application/json \
  Content-Type:application/json
```
> **Status esperado:** 200 OK — Lista todos los Pokémon de tipo Fuego.


### Paginación (Segunda página)
http GET "$BASE_URL/$RESOURCE" \
  Accept:application/json \
  Content-Type:application/json \
  limit==$LIMIT \
  offset==10
> **Status esperado:** 200 OK — Retorna Pokémon del 11 al 20.

### Comando 6 - Segundo recurso: Listar habilidades (Abilities)
http GET "$BASE_URL/ability" \
  Accept:application/json \
  Content-Type:application/json \
  limit==$LIMIT \
  offset==$OFFSET
> **Status esperado:** 200 OK — Lista las habilidades disponibles en la PokéAPI.

---

### Comando 7 - Detalle de habilidad Blaze
http GET "$BASE_URL/ability/blaze" \
  Accept:application/json \
  Content-Type:application/json \
  User-Agent:"HTTPieLab3/1.0"
> **Status esperado:** 200 OK — Retorna detalle de la habilidad Llamarada.

## Errores Intencionales

### 404 Not Found (Pokémon inexistente)
http GET "$BASE_URL/pokemon/pokemon-inexistente-99999" \
  Accept:application/json \
  Content-Type:application/json
> **Status esperado:** 404 Not Found — El Pokémon solicitado no existe.

### 400 Bad Request (parámetros inválidos)
http GET "$BASE_URL/pokemon" \
  Accept:application/json \
  Content-Type:application/json \
  limit==-1 \
  offset==abc
> **Status esperado:** 400 Bad Request — Parámetros de query inválidos.

### Auth fallida (token inválido)
http GET "$BASE_URL/$RESOURCE/$ID" \
  Accept:application/json \
  Content-Type:application/json \
  Authorization:"$TOKEN_INVALID"
> **Status esperado:** 401 Unauthorized — Token inválido o expirado (en APIs con auth real).

### 403 Forbidden (recurso administrativo)
http GET "$BASE_URL/admin/users" \
  Accept:application/json \
  Authorization:"$TOKEN_INVALID"
> **Status esperado:** 404/403 — Recurso protegido o inexistente.


## Solicitud Real — Investigar Charizard
### Datos de Charizard
http GET "$BASE_URL/pokemon/charizard" \
  Accept:application/json \
  Content-Type:application/json \
  User-Agent:"HTTPieLab3/1.0"

### Habilidad de Charizard
http GET "$BASE_URL/ability/blaze" \
  Accept:application/json \
  Content-Type:application/json \
  User-Agent:"HTTPieLab3/1.0"

### Tipo Dragón
http GET "$BASE_URL/type/dragon" \
  Accept:application/json \
  Content-Type:application/json \
  User-Agent:"HTTPieLab3/1.0"

>La PokéAPI (https://pokeapi.co) es una API pública que no requiere API key.
> Para simular errores de autenticación (401/403), se deben probar con APIs que sí requieran auth.

