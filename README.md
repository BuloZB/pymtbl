pymtbl: Python bindings for the mtbl sorted string table library
----------------------------------------------------------------

`pymtbl` provides a simple Pythonic wrapper for
[mtbl](https://github.com/farsightsec/mtbl)'s reader, writer, sorter, and
merger interfaces. The `examples/` directory contains scripts demonstrating
each of these interfaces. The following transcript shows the basic reader and
writer interfaces:

    >>> import mtbl
    >>> w = mtbl.writer('example.mtbl', compression=mtbl.COMPRESSION_SNAPPY)
    >>> w['key1'] = 'val1'
    >>> w['key17'] = 'val17'
    >>> w['key2'] = 'val2'
    >>> w['key23'] = 'val23'
    >>> w['key3'] = 'val3'
    >>> w['key4'] = 'val4'
    >>> w['key05'] = 'val05'
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "mtbl.pyx", line 361, in mtbl.writer.__setitem__ (mtbl.c:4108)
    mtbl.KeyOrderError
    >>> w.close()
    >>> r = mtbl.reader('example.mtbl', verify_checksums=True)
    >>> for k,v in r.get_range('key19', 'key23'): print k,v
    ... 
    key2 val2
    key23 val23
    >>> for k,v in r.get_prefix('key2'): print k,v
    ... 
    key2 val2
    key23 val23
    >>> for k,v in r.iteritems(): print k, v
    ... 
    key1 val1
    key17 val17
    key2 val2
    key23 val23
    key3 val3
    key4 val4
    >>>
