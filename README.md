# sync data from pg 2 pg

## how to running

* tap pg python venv

```code
cd tap-pg
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

* target pg python venv

```code
cd target-pg
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

* start demo pg with docker-compose

```code
docker-compose up -d
```

* init tap pg with some demo datas

```code
tables:

CREATE TABLE userapps (
    id SERIAL PRIMARY KEY,
    username text,
    userappname text
);

import some datas

INSERT INTO "public"."userapps"("id","username","userappname")
VALUES
(1,E'dalong',E'app'),
(2,E'first',E'login');
```

* discover schema from tap

```code
./tap-pg/venv/bin/tap-postgres -c ta-pg.json -d > catalog.json
```

* select table

```diff
        {
          "breadcrumb": [],
          "metadata": {
            "table-key-properties": [
              "id"
            ],
+            "selected": true,
+            "replication-method": "FULL_TABLE",
            "schema-name": "public",
            "database-name": "postgres",
            "row-count": 0,
            "is-view": false
          }
        },
```

* do sync

```code
 ./tap-pg/venv/bin/tap-postgres -c tap-pg.json --catalog catalog.json | ./target-pg/venv/bin/target-postgres -c target-pg.json
```

## some notes

you can also use pgloader  && dbt also a better one
