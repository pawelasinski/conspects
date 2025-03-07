## 1. General Structure and Hints

- **Checking available commands and options**  
  ```bash
  hdfs dfs -help
  hdfs dfs -help <command>
  ```  

- **Viewing the version**  
  ```bash
  hadoop version
  ```

---

## 2. Working with Files and Directories

### Viewing Contents (LS)

- **Listing the contents of a directory**  
  ```bash
  hdfs dfs -ls /path/to/directory
  ```  
  With the `-R` (recursive) option:  
  ```bash
  hdfs dfs -ls -R /path/to/directory
  ```

- **Checking quota and usage information**  
  ```bash
  hdfs dfs -count -q /path/to/directory
  ```

---

This command displays HDFS usage information in terms of quotas. Unlike the simple  
```bash
hdfs dfs -count /path/to/directory
```
which shows only the number of directories, files, and the total size of data, the **`-q`** (quota) option additionally outputs columns related to quotas:

1. **QUOTA** — the limit on the number of **subdirectories and files** (Directory Quota).  
2. **REM_QUOTA** — the remaining quota for object count (how many more can be created).  
3. **SPACE_QUOTA** — the limit on the total volume of data that can be stored in this directory (Space Quota).  
4. **REM_SPACE_QUOTA** — the remaining space quota.  
5. **DIR_COUNT** — the total number of directories (including subdirectories).  
6. **FILE_COUNT** — the number of files (and symbolic links, if supported).  
7. **CONTENT_SIZE** — the total size of the content in bytes (the actual disk space used by the file structure in HDFS).  
8. **PATHNAME** — the path to the directory.

Thus, the output includes not only the standard counters (directories, files, and total size) but also quota information (if quotas are set on that HDFS path). If quotas are not set (or not supported), the corresponding fields may display as `none`, `inf`, or `-`.

Example of typical output (columns may vary slightly across different Hadoop versions):
```
QUOTA  REM_QUOTA  SPACE_QUOTA  REM_SPACE_QUOTA  DIR_COUNT  FILE_COUNT   CONTENT_SIZE  PATHNAME
none   inf        none         inf              107        13           72587175      /user/hive/warehouse
```
- **Note:** `none` indicates that no quota is set for the number of objects or for space.  
- **Note:** `inf` means that the limit is not restricted.

Thus, the `-count -q` command is useful when you need not only to determine how much data is stored in a directory, but also whether any limits (quotas) are set and how much of them have been used.

---

The `-h` (human-readable) option is sometimes supported:
```bash
hdfs dfs -du -s -h /path/to/directory
```
where `-du` shows the disk usage.

### Creating and Deleting Directories

- **Creating a directory**  
  ```bash
  hdfs dfs -mkdir /path/to/directory
  ```  
  The `-p` option creates intermediate directories if they don’t exist:
  ```bash
  hdfs dfs -mkdir -p /path/to/directory/nested_directory
  ```

- **Deleting an empty directory**  
  ```bash
  hdfs dfs -rmdir /path/to/directory
  ```  
  If you need to delete a non-empty directory (recursively), use the `-rm -r` command.

### Uploading Files to HDFS

- **Copying files from the local file system to HDFS**  
  ```bash
  hdfs dfs -put /path/to/local/file /path/to/HDFS/
  ```  
  or  
  ```bash
  hdfs dfs -copyFromLocal /path/to/local/file /path/to/HDFS/
  ```  
  - The `-f` (force) option can be used to overwrite the file if necessary.

- **Moving a file from the local file system to HDFS (deleting the local file)**  
  ```bash
  hdfs dfs -moveFromLocal /path/to/local/file /path/to/HDFS/
  ```

### Downloading (Extracting) Files from HDFS

- **Downloading files from HDFS to the local file system**  
  ```bash
  hdfs dfs -get /path/to/HDFS/file /path/to/local/directory
  ```  
  or  
  ```bash
  hdfs dfs -copyToLocal /path/to/HDFS/file /path/to/local/directory
  ```

- **Moving a file from HDFS to the local file system (deleting it from HDFS)**  
  ```bash
  hdfs dfs -moveToLocal /path/to/HDFS/file /path/to/local/directory
  ```  
  *(Note: this command may not be available in some Hadoop distributions.)*

### Deleting Files

- **Deleting a file**  
  ```bash
  hdfs dfs -rm /path/to/file
  ```  
  - The `-skipTrash` option deletes the file bypassing the Trash.  
  - The `-r` option deletes a directory recursively.

Example:
```bash
hdfs dfs -rm -r -skipTrash /path/to/directory
```

### Viewing File Contents

- **Reading a file directly from HDFS to stdout**  
  ```bash
  hdfs dfs -cat /path/to/file
  ```  
  Example:
  ```bash
  hdfs dfs -cat /user/hive/warehouse/table/000000_0
  ```

- **Viewing the first N lines**  
  ```bash
  hdfs dfs -head /path/to/file
  ```  
  By default, this shows the first 1K bytes; depending on the version, you can specify `-n <number_of_lines>`.

- **Viewing the last N lines**  
  ```bash
  hdfs dfs -tail /path/to/file
  ```  
  Typically, this displays the last 1K bytes. In some versions, there is an option `-f` (to follow updates, like `tail -f`).

---

## 3. Managing Permissions, Ownership, and Replication

### Access Permissions (chmod)

- **Changing access permissions**  
  ```bash
  hdfs dfs -chmod <mode> /path/to/file/or/directory
  ```  
  Example:
  ```bash
  hdfs dfs -chmod 755 /user/username/directory
  ```

### Owner and Group (chown)

- **Changing the owner and group**  
  ```bash
  hdfs dfs -chown [owner][:[group]] /path/to/file
  ```  
  Examples:
  ```bash
  # Change only the owner
  hdfs dfs -chown newowner /path/to/file
  
  # Change both the owner and group
  hdfs dfs -chown newowner:newgroup /path/to/file
  ```

### Replication and Duplication Factors (setrep)

- **Viewing or changing the replication factor**  
  ```bash
  # Setting the replication factor for a file/directory
  hdfs dfs -setrep -w <integer> /path/to/file/or/directory
  ```  
  The `-w` parameter makes the command wait until the replication operation is complete.

---

## 4. Useful Commands for Diagnostics and Administration

### Checking Integrity (fsck)

- **Checking HDFS for block corruption**  
  ```bash
  hdfs fsck /path/to/file/or/directory -files -blocks -locations
  ```  
  - Parameters:
    - `-files` — show files,
    - `-blocks` — show blocks,
    - `-locations` — show the physical locations of blocks.

### Testing for Object Existence (test)

- **Checking if a file/directory exists**  
  ```bash
  hdfs dfs -test -[e|d|f|s|z] /path/to/object
  ```  
  Flags:
  - `-e` — checks if the file/directory exists;
  - `-d` — checks if it is a directory;
  - `-f` — checks if it is a file;
  - `-s` — checks if the file is non-empty (non-zero);
  - `-z` — checks if the file is empty (zero length).

### Brief Information about a File/Directory

- **The `-stat` command**  
  ```bash
  hdfs dfs -stat "%r %y %F" /path/to/file
  ```  
  Where:
  - `%r` — replication factor,
  - `%y` — last modification time,
  - `%F` — file type (FILE, DIRECTORY, etc.).

### Copying Between Clusters (distcp)

If you have multiple Hadoop clusters and a configured network, `distcp` is often used to copy large volumes of data. This is a separate tool (command):
```bash
hadoop distcp hdfs://namenode_src:8020/path hdfs://namenode_dest:8020/path
```
It allows you to copy data in parallel between clusters while preserving directory structure and metadata.

---

## 5. Working with the Trash

By default, when deleting files with `hdfs dfs -rm`, files may be moved to the “Trash.”  
- **Emptying the Trash**  
  ```bash
  hdfs dfs -expunge
  ```  
  This command deletes files that have remained in the Trash longer than the period defined in the configuration (`fs.trash.interval` and `fs.trash.checkpoint.interval`).

If you need to delete without moving to the Trash, use `-skipTrash`:
```bash
hdfs dfs -rm -skipTrash /path/to/file
```

---

## 6. Additional Tips and Recommendations

1. **Use aliases:** It can be very convenient to set up shortcut commands in your `~/.bashrc`, for example:
   ```bash
   alias hls='hdfs dfs -ls'
   alias hrm='hdfs dfs -rm'
   ```
   and so on.

2. **Check permissions and ownership:** If an operation fails, it might be due to insufficient permissions. In that case, either use the `hdfs` user (if accessible via `sudo -u hdfs`) or adjust the permissions accordingly.

3. **Be mindful of version differences:** Different Hadoop versions may have variations in keys and options (especially for commands like `-head`, `-tail`, `-test`, and `distcp`).

4. **Optimize large file transfers:** If the files are very large, consider using:
   - `distcp` for copying between clusters, or
   - `-D dfs.client.block.write.replace-datanode-on-failure.policy=NEVER` in extreme cases.

5. **Monitoring and Web UI:** In addition to the CLI, HDFS has a web interface (usually on port 50070 or 9870, depending on the version) where you can view the NameNode status, blocks, health, and more.