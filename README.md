# RDBMS4C

*Copyright (C) 2010  Szabolcs Kiss*

Embedded, in memory database designed for C developers

Learn more from reference guide:
/doc/reference_guide.pdf

 - C API
 - NOSQL
 - No persistent data on disc
 - Non-transactional
 - Open source (GPL)
 - Dynamic tables
 - C data types for columns
 - Foreign key support
 - Primary, unique and normal indexes
 - Column and function based indexes
 - Cursors for DML and query
 - Data safety
 - String support

Deoxyribonucleic acid (DNA) APIs



Examples how to create RDBMS tables with **RDBMS4C**:

```c
#define THERM_EVENTS_id 0
#define THERM_EVENTS_event_type_fk 1

#define THERM_EVENTS_event_type_fk_idx 1

#define get_THERM_EVENTS_id(p_record) db_get_field_as_unsigned_long(p_record, THERM_EVENTS_id)
#define get_THERM_EVENTS_event_type_fk(p_record) db_get_field_as_foreign_key_record(p_record, THERM_EVENTS_event_type_fk)
```

```c
void therm_events_constructor()
{
    __db_primary_key primary_key;
    __db_index event_type_index;
    record_definition = db_create_record_definition(2);
    db_set_record_definition_field(record_definition, THERM_EVENTS_id, "id", __NOT_NULL, __unsigned_long);
    db_set_record_definition_field(record_definition, THERM_EVENTS_event_type_fk, "event_type_fk", __NOT_NULL, __foreign_key);

    primary_key = db_create_pk(1);
    db_set_index_field(primary_key, THERM_EVENTS_id, PRIMARY_KEY, __asc);

    table = db_create_table(record_definition, primary_key);

    db_set_foreign_key_table(table, THERM_EVENTS_event_type_fk, therm_event_type_foreign_key_parent());

    db_table_auto_increment(table);

    event_type_index = db_create_index(1, __normal);
    db_set_index_field(event_type_index, 0, THERM_EVENTS_event_type_fk, __asc);
    db_table_set_index(table, THERM_EVENTS_event_type_fk_idx, event_type_index);

    cursor = db_create_cursor(table, PRIMARY_KEY);
}
```

```c
cursor = db_create_cursor(get_therm_events_table(), THERM_EVENTS_event_type_fk_idx);
__for_cursor_loop(record, cursor)
    printf("Evnt type: %i - %s, Event: %i \n",
       get_THERM_EVENT_TYPES_id(
           get_THERM_EVENTS_event_type_fk(record)),
           get_THERM_EVENT_TYPES_name(
           get_THERM_EVENTS_event_type_fk(record)),
           get_THERM_EVENTS_id(record));
__end_loop(record, cursor)

db_drop_cursor(cursor);
```
