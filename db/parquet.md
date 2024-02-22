# Parquet

### 1. Does parquet store data as row or column ?
Column. Its a columnar file format.

### 2. How to use parquet ?
System of record

### 3. Is it schema on read or schema on write ?
Its schema on read. Schema is appened at the end of the file

### 4. Benefits of parquet being a columnar ?
* Compressesion as data of same type are stored together.
* Only required columns are loaded in-memory allow more data to be loaded.
* Serialization and deserialization of data written in a columnar format is usually much faster due to the fact that a given column's data is stored contiguously.
