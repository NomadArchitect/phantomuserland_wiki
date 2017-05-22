# Phantom kernel disk (block device) subsystem

See ```disk.h```, ```disk_q.h``` and ```pager_io_req.h``` for reference.

Basic disk io is asyncronous and is handled with the help of ```pager_io_request``` structure, which represents one io request to one device (or partition).

## Main entry points

### ```void disk_enqueue( phantom_disk_partition_t *p, pager_io_request *rq );```

This function enqueues request for input/output and returns ASAP. As soon as io is done, callback function (```pager_io_request.pager_callback```) is called with request passed.

There are more entry points for secondary operations:

### ```errno_t disk_dequeue( struct phantom_disk_partition *p, pager_io_request *rq );```

Attempt to dequeue request from disk io queue. Can fail if request is being handled or already done.

### ```errno_t disk_raise_priority( struct phantom_disk_partition *p, pager_io_request *rq );```

Attempt to requeue request to be handled ASAP. Can fail.

### ```errno_t disk_fence( struct phantom_disk_partition *p );```

**NB! Not completely implemented, check sources.** Do not return until all previous requests for this device are done.

### ```errno_t disk_trim( struct phantom_disk_partition *p );```

**NB! Not yet implemented, check sources.** Inform device that this block does not contain any information that is needed any more. Critical for SSD disks.

## pager_io_request

This structure represents an asyncronous block io request.

### ```pager_io_request_init( pager_io_request *me )```

Call to prepare/clean request.

### Fields that must be filled by caller

```
    physaddr_t          phys_page;        	// physmem address
    disk_page_no_t      disk_page;        	// disk address in blocks (4096 bytes usually)

    unsigned char       flag_pagein;            // Read
    unsigned char       flag_pageout;           // Write

    void                (*pager_callback)( struct pager_io_request *req, int write );

    errno_t             rc;                     // Driver return code
```

## phantom_disk_partition_t

Each physical block device or logical part of such device (partition) is described with such a structure.

