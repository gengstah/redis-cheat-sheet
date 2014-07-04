# Redis - key/value store

## Setting server:name as key to the value of "fido"

`SET server:name "fido"`  
`GET server:name => "fido"`

## Increment and Delete

`SET connections 10`  
`INCR connections => 11`  
`INCR connections => 12`  
`DEL connections`  
`INCR connections => 1`  
`// SETNX - SET-if-not-exists`  
`// INCR - is an atomic operation`

## Expire and Time To Live

`SET resource:lock "Redis Demo"`  
`EXPIRE resource:lock 120`

`TTL resource:lock => 113`  
`// after 113s`  
`TTL resource:lock => -2` `(-2 means that the key does not exist, -1 means that the key will never expire)`

`SET resource:lock "Redis Demo 1"`  
`EXPIRE resource:lock 120`  
`TTL resource:lock => 119`  
`SET resource:lock "Redis Demo 2"`	`// Setting a key will reset the key's TTL`  
`TTL resource:lock => -1`

## Lists

#### RPUSH puts the new value at the end of the list.

`RPUSH friends "Alice"`  
`RPUSH friends "Bob"`

#### LPUSH puts the new value at the start of the list.

`LPUSH friends "Sam"`

#### LRANGE gives a subset of the list. A value of -1 for the second parameter means to retrieve elements until the end of the list.

`LRANGE friends 0 -1 => ["Sam","Alice","Bob"]`  
`LRANGE friends 0 1 => ["Sam","Alice"]`  
`LRANGE friends 1 2 => ["Alice","Bob"]`

#### LLEN returns the current length of the list.

`LLEN friends => 3`

#### LPOP removes the first element from the list and returns it.

`LPOP friends => "Sam"`

#### RPOP removes the last element from the list and returns it.

`RPOP friends => "Bob"`

## Sets

#### SADD adds the given value to the set.

`SADD superpowers "flight"`  
`SADD superpowers "x-ray vision"`  
`SADD superpowers "reflexes"`

#### SREM removes the given value from the set.

`SREM superpowers "reflexes"`

#### SISMEMBER tests if the given value is in the set.

`SISMEMBER superpowers "flight" => true`  
`SISMEMBER superpowers "reflexes" => false`

#### SMEMBERS returns a list of all the members of this set.

`SMEMBERS superpowers => ["flight","x-ray vision"]`

#### SUNION combines two or more sets and returns the list of all elements.

`SADD birdpowers "pecking"`  
`SADD birdpowers "flight"`  
`SUNION superpowers birdpowers => ["flight","x-ray vision","pecking"]`

## Sorted Sets - with score

#### ZADD adds the given score and value to the sorted set

`ZADD hackers 1940 "Alan Kay"`  
`ZADD hackers 1906 "Grace Hopper"`  
`ZADD hackers 1953 "Richard Stallman"`  
`ZADD hackers 1965 "Yukihiro Matsumoto"`  
`ZADD hackers 1916 "Claude Shannon"`  
``ZADD hackers 1969 "Linus Torvalds"`  
`ZADD hackers 1957 "Sophie Wilson"`  
`ZADD hackers 1912 "Alan Turing"`  

#### ZRANGE gives a subset of the sorted set

`ZRANGE hackers 2 4 => ["Claude Shannon", "Alan Kay","Richard Stallman"]`

# Hashes

Hashes are maps between string fields and string values, so they are the perfect data type to represent objects (eg: A User with a number of fields like name, surname, age, and so forth):

`HSET user:1000 name "John Smith"`  
`HSET user:1000 email "john.smith@example.com"`  
`HSET user:1000 password "s3cret"`

To get back the saved data use HGETALL:

`HGETALL user:1000`

You can also set multiple fields at once:

`HMSET user:1001 name "Mary Jones" password "hidden" email "mjones@example.com"`

If you only need a single field value that is possible as well:

`HGET user:1001 name => "Mary Jones"`

Numerical values in hash fields are handled exactly the same as in simple strings and there are operations to increment this value in an atomic way.

`HSET user:1000 visits 10`  
`HINCRBY user:1000 visits 1 => 11`  
`HINCRBY user:1000 visits 10 => 21`  
`HDEL user:1000 visits`  
`HINCRBY user:1000 visits 1 => 1`
