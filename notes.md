# Reactive Redis with Java

## Redis

- fast in-memory NoSQL database
- multi-purpose data structure server
- use cases

1. caching
2. pub-sub
3. message queue
4. streaming
5. geospatial

- free & open source
- redis.io -> open source
- redislabs.com -> commercial cloud service

- redis-cli ---> redis-server (6379)
- redis-cli uses imperative commands

```
> ping
pong
```

- Redis: Key-Value
  - Simple key-value store
  - Redis is simple - keys and values are simple strings
  - any binary sequence can be the key (prefer short, readable key)
  - 2 ^ 32 keys (>4 billion keys)
  - 512 MB max size

```sh
# set <key> <value> -  set the key:value pair
set a b
# get <key>
get a
get c # nil
```

- Redis encourages developers to follow their standards for key name, e.g. `/user/1/name` or preferred way `user:1:name`
- access keys

```sh
keys <regex-pattern>
# access all keys; can affect the performance
keys *
# access keys that match the following pattern
keys user*
# scan - newer function; better option
scan 0 # list all keys in a paginated way (10 by 10); use the number it displays to fetch the next page
# scan <page-no> MATCH <pattern> - scans the page no <page-no> by a given pattern
scan 0 MATCH user*
# limit - scan <page-no> MATCH <pattern> count <number>

scan 0 MATCH user* count 3
```

- delete keys

```sh
# delete an entry by key
# del <key>
> del user:1:name
(integer) 1 # num of deleted rows
# if we try to delete a non-existent entry, it will return 0
# del <key-1> <key-12> <key-3> - deletes all given keys (entries by keys)
# it doesn't accept regex pattern
# truncate database
flushdb
```

#### Expiring keys

```sh
# it will automatically expire after 10 seconds
set a b ex 10

# access the TTL of a key
# ttl <key>
ttl a

# extend the expiry
# expire <key> <new-expiry-in-seconds>
extend a 60

# expire at Unix timestamp
set a b exat 1766010100

# expiry in milliseconds
set b c px 3000  # 3000 milliseconds

# if you have a TTL set on a key and you change the value associated with that key, the expiry will be gone
set a b ex 60
ttl a -> 60
set a c
ttl a -> -1

# how to keep TTL?
set a b ex 60
ttl a -> 60
set a c keepttl
ttl a -> 55
```
