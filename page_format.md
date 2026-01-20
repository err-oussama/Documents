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
```
#include <stdint.h>
#include <stdbool.h>
#include <pthread.h>

// --- Disk page header (matches your disk format) ---
typedef struct DiskHeader {
    uint32_t magic;     // 4 bytes
    uint32_t size;      // page size in bytes
    uint8_t  version;   // format version
    uint8_t  type;      // page type (META, INDEX, HEAP, etc.)
    uint16_t flags;     // optional on-disk flags
    uint64_t id;        // unique page id
    uint64_t LSN;       // log sequence number
    uint64_t checksum;  // page checksum
} DiskHeader;

// --- Memory page (buffer pool page) ---
typedef struct MemPage {
    // Identity & pointer
    uint64_t id;        // same as disk page id
    uint8_t* payload;   // pointer to in-memory page data (page size bytes)

    // Concurrency
    uint16_t pin_count;     // number of threads using this page
    pthread_mutex_t latch;  // protects page metadata and payload

    // Page state flags (1 bit each)
    struct {
        uint8_t is_dirty            : 1;  // memory != disk
        uint8_t page_io_in_progress : 1;  // reading/writing to disk
        uint8_t page_read_pending   : 1;  // not fully loaded
        uint8_t clock_bit           : 1;  // LRU/Clock eviction
        uint8_t reserved            : 4;  // future flags
    } flags;

    // LRU / eviction info
    struct MemPage* lru_prev;   // previous page in LRU list
    struct MemPage* lru_next;   // next page in LRU list

    // Optional runtime helpers (can be NULL)
    void* slot_cache;           // fast record lookup
} MemPage;


```
