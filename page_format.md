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
    
### META:(HEADER)
    


### INDEX



### HEAP:(DATA/LEAF)



### FREE:(UNUSED)



### OVERFLOW



### LOB:(BLOB)

# PAGE MEMORY FORMAT
## HEADER
    id          :
    base_header :
    payload     :
    pin_count   :
    is_dirty    :
    is_loading  :
    latch       :
    LRU         :
    flags       :


