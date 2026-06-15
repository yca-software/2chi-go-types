# 2Chi Go Types

Shared Go types for 2Chi projects.

```go
import chi_types "github.com/yca-software/2chi-go-types"
```

## Geo

WGS84 (SRID 4326) types for PostGIS `geography` / `geometry` columns. Both implement `sql.Scanner` and `driver.Valuer`.

| Type | Description |
| --- | --- |
| `Point` | A single coordinate (`Lng`, `Lat`) |
| `Polygon` | A slice of `Point` values forming a closed ring |

**Read:** accepts PostGIS EWKB as a hex string or raw bytes.

**Write:** emits EWKT (`SRID=4326;POINT(lng lat)` or `SRID=4326;POLYGON((...))`).

### Example

```go
p := chi_types.Point{Lng: 2.3522, Lat: 48.8566}

val, err := p.Value() // "SRID=4326;POINT(2.3522 48.8566)"

var scanned chi_types.Point
err = scanned.Scan(hexEWKBFromDB)
```

## Pagination

Generic JSON shape for paginated list API responses.

| Field | Description |
| --- | --- |
| `Items` | Page of results |
| `HasNext` | Whether another page exists |

### Example

```go
type User struct {
    ID   string `json:"id"`
    Name string `json:"name"`
}

resp := chi_types.PaginatedListResponse[User]{
    Items:   users,
    HasNext: len(users) == pageSize,
}
```
