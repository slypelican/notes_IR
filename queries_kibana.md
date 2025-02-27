# Kibana Queries
This is a collection of Kibana queries that are useful for incident response.

## ES|QL
### Simple INNER JOIN implementation

#### Sample Data
Row 1: `{ "_index": "source1", "src.ip": "127.0.0.1", "dst.ip": "169.254.169.254" }`\
Row 2: `{ "_index": "source2", "source": "127.0.0.1", "username": "local" }`

#### Objective
For records from source IP `127.0.0.1`, combine the records around the source ID to correlate username to destination IP.

#### Query
```
FROM source1, source2
| EVAL source_ip = COALESCE(TO_IP(src.ip), TO_IP(source))
| WHERE source_ip == "127.0.0.1"
| STATS destination_ip = VALUES(dst.ip), user = VALUES(username) BY source_ip
```
