# PAGE DISK FORMAT


## BASE HEADER
    magic       :   4 byte
    size        :   4 byte
    version     :   1 byte
    type        :   1 byte
    flags       :   2 byte
    id          :   8 byte
    LSN         :   8 byte
    checksum    :   8 byte

## PAGES TYPE:
    -META:(HEADER)
    -INDEX
    -HEAP:(DATA/LEAF)
    -FREE:(UNUSED)
    -OVERFLOW
    -LOB:(BLOB)

# PAGE MEMORY FORMAT

## HEADER
    id          :   id of the page the same is it in disk
    payload     :   pointer to the page in memory
    pin_count   :   counter of who many thread using this page
    flags       :
        -is_dirty               =>  do whe change on this page. memory version != disk version
        -page_io_in_progress    =>  the page is currently in IO operation so it not stable to deal with 
        -page_read_pending      =>  the page is not yet full loeaded into memory
        ...
    latch       :
    LRU         :   
        -clock bit
        -pointers(prev, next)

