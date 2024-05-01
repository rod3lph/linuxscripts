- show filesystem cluster size 
  ``` 
  fsutil fsinfo ntfsinfo l:
  ```
- Cluster Size Reference NTFS
  #+BEGIN_QUOTE
  Volume size	          |Windows NT 3.51	|Windows NT 4.0	|Windows 7, Windows Server 2008 R2, Windows Server 2008, Windows Vista, Windows Server 2003, Windows XP, Windows 2000
  7 MB–512 MB	          | 512 bytes          	| 4 KB                        | 4 KB
  512 MB–1 GB	          | 1 KB                   	| 4 KB                    	  | 4 KB
  1 GB–2 GB	              | 2 KB                  	| 4 KB                     	  | 4 KB
  2 GB–2 TB	              | 4 KB                  	| 4 KB                    	  | 4 KB
  2 TB–16 TB	             Not Supported*	Not Supported* 	  | 4 KB
  16TB–32 TB	         Not Supported*	Not Supported* 	  | 8 KB
  32TB–64 TB       	 Not Supported*	Not Supported* 	  | 16 KB
  64TB–128 TB      	 Not Supported*	Not Supported* 	  | 32 KB
  128TB–256 TB     	 Not Supported*	Not Supported* 	  | 64 KB
  > 256 TB	                 Not Supported	    Not Supported   	Not Supported
  
  #+END_QUOTE