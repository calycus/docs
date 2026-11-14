# Dynatrace — CBF Account Services

Dashboard Classic listo para importar que analiza estos microservicios **desde hoy en adelante** (`timeframe: Today`):

| Servicio |
| --- |
| `cbf-loandepo-account-services` |
| `cbf-domaar-generation-account` |
| `cbf-custmgmt-miles-accrual` |
| `cbf-custmgmt-limit-account` |

Archivo: [`cbf-account-services-today.json`](./cbf-account-services-today.json)

## Qué incluye

1. Salud de servicios + problemas abiertos  
2. Throughput (req/min) por micro  
3. Latencia mediana en el tiempo  
4. Tasa de error (%) en el tiempo  
5. Comparativo del día: volumen, errores server, p95  
6. Detalle de latencia por cada micro  

## Importar en la UI

1. En Dynatrace: **Dashboards** → **Import dashboard**  
2. Sube `cbf-account-services-today.json`  
3. Confirma que el filtro global quede en **Today**  

Si algún tile no muestra datos, el nombre del servicio en Dynatrace puede diferir (por ejemplo con sufijo de proceso/host). Ajusta el filtro `entityName` en Data Explorer.

## Crear vía API

Scopes del token: `WriteConfig` (y `ReadConfig` para validar).

```bash
export DT_ENV_URL="https://<environment-id>.live.dynatrace.com"
export DT_API_TOKEN="<token>"

# Validar
curl -sS -o /dev/null -w "%{http_code}\n" \
  -X POST "$DT_ENV_URL/api/config/v1/dashboards/validator" \
  -H "Authorization: Api-Token $DT_API_TOKEN" \
  -H "Content-Type: application/json" \
  --data-binary @cbf-account-services-today.json

# Crear
curl -sS -X POST "$DT_ENV_URL/api/config/v1/dashboards" \
  -H "Authorization: Api-Token $DT_API_TOKEN" \
  -H "Content-Type: application/json" \
  --data-binary @cbf-account-services-today.json
```

## Nota sobre nombres de entidad

Los filtros usan `entityName.equals("<nombre>")`. Si en tu tenant el servicio aparece como `cbf-...` dentro de un process group con otro display name, busca la entidad en **Services** y actualiza el JSON o el filtro del tile.
