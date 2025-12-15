# Incremental Copy CLI

A C implementation of a fast **incremental file and directory copy utility**.
The tool traverses a source filesystem tree and synchronizes it with a destination
by copying only new or modified files, avoiding unnecessary I/O.

The implementation relies on filesystem metadata (e.g. inode information) to
detect identical files efficiently.

**Goals/Tasks**

The goal of this project is to copy directories/files quicker than the simple `cp` command. 

## Summary

The program takes two arguments: a **source** path (A) and a **destination**
path (T). It traverses the source filesystem tree and incrementally
synchronizes it with the destination by comparing filesystem metadata
(inodes).

For each entry in the source tree, the corresponding entry in the destination
is examined and one of the following actions is taken:

1. **Entry does not exist in T**  
   The entry is created in T. If it is a file, its contents are copied.

2. **Entry exists and is identical**  
   No action is taken. This is the case where incremental copying outperforms
   a naive `cp`.

3. **Entry exists in T but not in A**  
   The entry has been removed from the source and is deleted from T.

4. **Entry exists but differs**  
   The destination entry is updated by copying the source contents.

File equality is determined without reading file contents, using metadata such
as file type, size, and modification time. If both entries are directories,
the comparison is applied recursively.


**Compilation**

`./quic -v -d origindir destdir`

- `origindir` is the source directory
- `destdir` is the destination directory
- `v` is a flag and prints info about choices/actions of the programm regarding to copying/deleting files.
- `d` is a flag that defines that the files that were deleted from the source directory must not exist in the destination folder.
