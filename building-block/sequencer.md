## Sequencer

1. UUID
2. Twitters snowflake id

### UUID
* 128 bits (16 Bytes)

### Twitter snowflake id
* 64 bits (8 Bytes)
* parity(0) + epoch_timestamap_ms(41) + server_id(10) + sequencer_per_server(12)
* 2^41 => 200 years
