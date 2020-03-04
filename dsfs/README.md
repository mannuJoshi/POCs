Description

HDF5 is a data model, library, and file format for storing and managing data. It supports an unlimited variety of datatypes, and is designed for flexible and efficient I/O and for high volume and complex data. HDF5 is portable and is extensible, allowing applications to evolve in their use of HDF5. The HDF5 Technology suite includes tools and applications for managing, manipulating, viewing, and analyzing data in the HDF5 format. link: https://portal.hdfgroup.org/display/HDF5/HDF5

Heap Buffer overflow
--------------------------------------------
command: ./h5repack -f GZIP=8 -l dset1:CHUNK=5x6 h5repack_fuzz/id\:000002\,src\:000000\,op\:ext_AO\,pos\:1088 /home/aceteam/Downloads/h5/aggr.h5

Version: h5repack: Version 1.13.0


Source code:

``` 2199	 
   2200	     /* check args */
   2201	     HDassert( f );
   2202	     HDassert( f->shared );
   2203	 
 → 2204	     cache_ptr = f->shared->cache;
   2205	 
   2206	     HDassert( cache_ptr );
   2207	     HDassert( cache_ptr->magic == H5C__H5C_T_MAGIC );
   2208	     HDassert( type );
   2209	     HDassert( type->mem_type == cache_ptr->class_table_ptr[type->id]->mem_type );
```
Debug:
```
Starting program: /home/aceteam/hdf5/build/bin/h5repack -f GZIP=8 -l dset1:CHUNK=5x6 h5repack_fuzz/id\:000002\,src\:000000\,op\:ext_AO\,pos\:1088 /home/aceteam/Downloads/h5/aggr.h5
warning: Probes-based dynamic linker interface failed.
Reverting to original interface.


Program received signal SIGSEGV, Segmentation fault.
[ Legend: Modified register | Code | Heap | Stack | String ]
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x00005555563a1450  →  0x0000000000000001
$rbx   : 0x00005555563f2d00  →  0x00005555563f39c0  →  "h5repack_fuzz/id:000002,src:000000,op:ext_AO,pos:1[...]"
$rcx   : 0x00007fffff7ff170  →  0x0000000000000000
$rdx   : 0x60              
$rsp   : 0x7fffff7fef98    
$rbp   : 0x00005555563f2d00  →  0x00005555563f39c0  →  "h5repack_fuzz/id:000002,src:000000,op:ext_AO,pos:1[...]"
$rsi   : 0x00005555563958a0  →  0x0000000000000005
$rdi   : 0x00005555563f2d00  →  0x00005555563f39c0  →  "h5repack_fuzz/id:000002,src:000000,op:ext_AO,pos:1[...]"
$rip   : 0x00005555556ad0cc  →  <H5C_protect+492> mov QWORD PTR [rsp], rdx
$r8    : 0x200             

$r9    : 0x0               
$r10   : 0x0               
$r11   : 0x00007fffff7ff388  →  0x0000000000000008
$r12   : 0x00005555563958a0  →  0x0000000000000005
$r13   : 0x200             
$r14   : 0x60              
$r15   : 0x60              
$eflags: [zero carry parity adjust sign trap INTERRUPT direction overflow RESUME virtualx86 identification]
$cs: 0x0033 $ss: 0x002b $ds: 0x0000 $es: 0x0000 $fs: 0x0000 $gs: 0x0000 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── stack ────
[!] Unmapped address
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── code:x86:64 ────
   0x5555556ad0bf <H5C_protect+479> mov    BYTE PTR [rcx], 0x1
   0x5555556ad0c2 <H5C_protect+482> xchg   ax, ax
   0x5555556ad0c4 <H5C_protect+484> lea    rsp, [rsp-0x98]
 → 0x5555556ad0cc <H5C_protect+492> mov    QWORD PTR [rsp], rdx
   0x5555556ad0d0 <H5C_protect+496> mov    QWORD PTR [rsp+0x8], rcx
   0x5555556ad0d5 <H5C_protect+501> mov    QWORD PTR [rsp+0x10], rax
   0x5555556ad0da <H5C_protect+506> mov    rcx, 0xdc08
   0x5555556ad0e1 <H5C_protect+513> call   0x5555556b7a20 <__afl_maybe_log>
   0x5555556ad0e6 <H5C_protect+518> mov    rax, QWORD PTR [rsp+0x10]
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── source:/home/aceteam/h[...].c+2204 ────
   2199	 
   2200	     /* check args */
   2201	     HDassert( f );
   2202	     HDassert( f->shared );
   2203	 
 → 2204	     cache_ptr = f->shared->cache;
   2205	 
   2206	     HDassert( cache_ptr );
   2207	     HDassert( cache_ptr->magic == H5C__H5C_T_MAGIC );
   2208	     HDassert( type );
   2209	     HDassert( type->mem_type == cache_ptr->class_table_ptr[type->id]->mem_type );
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "h5repack", stopped, reason: SIGSEGV
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── trace ────
[!] Cannot access memory at address 0x7fffff7fefa8
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
0x00005555556ad0cc in H5C_protect (f=0x5555563f2d00, type=0x5555563958a0 <H5AC_OHDR>, addr=0x60, udata=<error reading variable: Cannot access memory at address 0x7fffff7fefa8>, flags=0x200) at /home/aceteam/hdf5/src/H5C.c:2204
2204	    cache_ptr = f->shared->cache;




gef➤  bt
#0  0x00005555556ad0cc in H5C_protect (f=0x5555563f2d00, type=0x5555563958a0 <H5AC_OHDR>, addr=0x60, udata=<error reading variable: Cannot access memory at address 0x7fffff7fefa8>, flags=0x200) at /home/aceteam/hdf5/src/H5C.c:2204
#1  0x0000000000000060 in ?? ()
#2  0x00007fffff7ff170 in ?? ()
#3  0x0000000000000000 in ?? ()

gef➤  i r
rax            0x5555563a1450	0x5555563a1450
rbx            0x5555563f2d00	0x5555563f2d00
rcx            0x7fffff7ff170	0x7fffff7ff170
rdx            0x60	0x60
rsi            0x5555563958a0	0x5555563958a0
rdi            0x5555563f2d00	0x5555563f2d00
rbp            0x5555563f2d00	0x5555563f2d00
rsp            0x7fffff7fef98	0x7fffff7fef98
r8             0x200	0x200
r9             0x0	0x0
r10            0x0	0x0
r11            0x7fffff7ff388	0x7fffff7ff388
r12            0x5555563958a0	0x5555563958a0
r13            0x200	0x200
r14            0x60	0x60
r15            0x60	0x60
rip            0x5555556ad0cc	0x5555556ad0cc <H5C_protect+492>
eflags         0x10202	[ IF RF ]
cs             0x33	0x33
ss             0x2b	0x2b
ds             0x0	0x0
es             0x0	0x0
fs             0x0	0x0
gs             0x0	0x0
```

Asan:
```
ASAN:DEADLYSIGNAL
=================================================================
==23994==ERROR: AddressSanitizer: stack-overflow on address 0x7ffea14befe8 (pc 0x7ff25ea9bd2e bp 0x7ffea14bf080 sp 0x7ffea14beff0 T0)
    #0 0x7ff25ea9bd2d  (/usr/lib/x86_64-linux-gnu/libasan.so.4+0x27d2d)
    #1 0x7ff25eb52b1a in __interceptor_malloc (/usr/lib/x86_64-linux-gnu/libasan.so.4+0xdeb1a)
    #2 0x5649b5392adc in H5FL_malloc /home/aceteam/hdf5/src/H5FL.c:243
    #3 0x5649b5393b78 in H5FL_blk_malloc /home/aceteam/hdf5/src/H5FL.c:921
    #4 0x5649b57c0d3e in H5WB_actual /home/aceteam/hdf5/src/H5WB.c:188
    #5 0x5649b53db63c in H5G__traverse_real /home/aceteam/hdf5/src/H5Gtraverse.c:549
    #6 0x5649b53de732 in H5G_traverse /home/aceteam/hdf5/src/H5Gtraverse.c:854
    #7 0x5649b53be215 in H5G_loc_info /home/aceteam/hdf5/src/H5Gloc.c:842
    #8 0x5649b57b2534 in H5VL__native_object_get /home/aceteam/hdf5/src/H5VLnative_object.c:249
    #9 0x5649b57721e6 in H5VL__object_get /home/aceteam/hdf5/src/H5VLcallback.c:5604
    #10 0x5649b57896d5 in H5VL_object_get /home/aceteam/hdf5/src/H5VLcallback.c:5641
    #11 0x5649b54732db in H5Oget_info_by_name3 /home/aceteam/hdf5/src/H5O.c:613
    #12 0x5649b5207f57 in traverse_cb /home/aceteam/hdf5/tools/lib/h5trav.c:209
    #13 0x5649b53b1aa9 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:917
    #14 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #15 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #16 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #17 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #18 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #19 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #20 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #21 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #22 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #23 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #24 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #25 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #26 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #27 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #28 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #29 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #30 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #31 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #32 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #33 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #34 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #35 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #36 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #37 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #38 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #39 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #40 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #41 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #42 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #43 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #44 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #45 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #46 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #47 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #48 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #49 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #50 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #51 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #52 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #53 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #54 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #55 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #56 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #57 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #58 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #59 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #60 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #61 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #62 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #63 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #64 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #65 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #66 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #67 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #68 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #69 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #70 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #71 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #72 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #73 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #74 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #75 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #76 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #77 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #78 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #79 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #80 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #81 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #82 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #83 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #84 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #85 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #86 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #87 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #88 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #89 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #90 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #91 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #92 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #93 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #94 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #95 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #96 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #97 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #98 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #99 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #100 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #101 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #102 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #103 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #104 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #105 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #106 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #107 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #108 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #109 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #110 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #111 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #112 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #113 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #114 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #115 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #116 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #117 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #118 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #119 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #120 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #121 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #122 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #123 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #124 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #125 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #126 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #127 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #128 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #129 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #130 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #131 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #132 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #133 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #134 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #135 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #136 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #137 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #138 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #139 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #140 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #141 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #142 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #143 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #144 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #145 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #146 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #147 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #148 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #149 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #150 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #151 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #152 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #153 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #154 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #155 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #156 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #157 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #158 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #159 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #160 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #161 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #162 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #163 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #164 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #165 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #166 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #167 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #168 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #169 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #170 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #171 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #172 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #173 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #174 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #175 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #176 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #177 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #178 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #179 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #180 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #181 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #182 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #183 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #184 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #185 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #186 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #187 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #188 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #189 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #190 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #191 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #192 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #193 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #194 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #195 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #196 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #197 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #198 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #199 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #200 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #201 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #202 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #203 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #204 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #205 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #206 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #207 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #208 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #209 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #210 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #211 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #212 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #213 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #214 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #215 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #216 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #217 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #218 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #219 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #220 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #221 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #222 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #223 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #224 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #225 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #226 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #227 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #228 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #229 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #230 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #231 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #232 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #233 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #234 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #235 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #236 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #237 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #238 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #239 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #240 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #241 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #242 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #243 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #244 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211
    #245 0x5649b53d58f9 in H5G__stab_iterate /home/aceteam/hdf5/src/H5Gstab.c:556
    #246 0x5649b53ce4a6 in H5G__obj_iterate /home/aceteam/hdf5/src/H5Gobj.c:696
    #247 0x5649b53b2705 in H5G_visit_cb /home/aceteam/hdf5/src/H5Gint.c:1001
    #248 0x5649b53c6da9 in H5G__node_iterate /home/aceteam/hdf5/src/H5Gnode.c:1001
    #249 0x5649b5807308 in H5B__iterate_helper /home/aceteam/hdf5/src/H5B.c:1166
    #250 0x5649b580a7c5 in H5B_iterate /home/aceteam/hdf5/src/H5B.c:1211

SUMMARY: AddressSanitizer: stack-overflow (/usr/lib/x86_64-linux-gnu/libasan.so.4+0x27d2d) 
==23994==ABORTING
```
