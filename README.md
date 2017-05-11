# chash
C Hastable

libfh : fast hash, basic multi threading support, key is only string and is always copied inside (unless FH_SETATTR_DONTCOPYKEY is set), 
opaque data is allocated and copied inside hash and can be string (datalen = FH_DATALEN_STRING), fixed lenght (datalen = sizeof data) or datalen = FH_DATALEN_VOIDP just copies void pointer.
Sample code :

```
#include <stdio.h>
#include <pthread.h>
#include "fh.h"

int main(int argc, char **argv)
{
    fh_t *f = fh_create(1000, -1, NULL); // opaque data is string, using builtin hash function

    int err = fh_insert(f, "thekey", "value");

    char str[64];
    err = fh_search(f, "thekey", str, 64); // search will copy out data

    printf("value for the key : %s\n", str);

	// to get only the pointer to data
	char *value = fh_get(f, "thekey", &err);

    printf("value for the key : %s\n", value);

    fh_del(f, "thekey");

    fh_destroy(f);
}

```
To compile :

```
cd libfh ; make -f libfh.mk clean lib
```

Performance : run on Intel Core i7-4710HQ CPU @ 2.50GHz

```
------------ Testing fixed size opaque data hashtable 
hash real size 65536
Average insert time in nanosecs : 230.40
hash elements 30000
hash collision 6023
searching ..
Average access time in nanosecs : 154.05
deleting ..
hash elements 0
------------ Testing string opaque data 
hash real size 65536
Average insert time in nanosecs : 155.98
hash elements 30000
hash collision 6005
searching ..
Average access time in nanosecs : 111.69
deleting ..
hash elements 0
------------ Testing void pointer opaque data 
hash real size 65536
Average insert time in nanosecs : 147.02
hash elements 30000
hash collision 6005
searching ..
Average access time in nanosecs : 81.54
deleting ..
hash elements 0
------------ end of tests 
```

