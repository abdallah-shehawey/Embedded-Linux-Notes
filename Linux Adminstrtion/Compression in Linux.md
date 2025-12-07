# Archiving and Compression in Linux

In Linux, **compression** is conceptually similar to creating a `.zip` file in Windows. However, Linux separates the ideas of **archiving** and **compression**, giving you more flexibility.

- **Archiving**: Bundling multiple files/directories into a single file.
    
- **Compression**: Reducing the size of that file using algorithms such as gzip or bzip2.
    

The most common tool for archiving in Linux is **`tar`**, which stands for **Tape ARchive**.

---

## 1. Creating an Archive _Without_ Compression

This creates a `.tar` file that only bundles files/directories together, without reducing their size.

```bash
tar -cvf file.tar target_file_or_dir
```

### Options explained:

- `-c` → **Create** a new archive
    
- `-v` → **Verbose** output (shows files being archived)
    
- `-f` → **File** name of the archive
    

### Example:

```bash
tar -cvf project.tar project/
```

### Extracting an uncompressed archive:

```bash
tar -xvf file.tar -C output_dir
```

- `-x` → **Extract** files
    
- `-C` → Directory where files will be extracted
    

---

## 2. Creating an Archive _With_ Compression

Linux allows you to choose the compression algorithm while creating the archive.

### 2.1 Using gzip compression (`.tar.gz` or `.tgz`)

```bash
tar -czvf file.tar.gz target_file_or_dir
```

- `-z` → Compress using **gzip**
    

### Example:

```bash
tar -czvf backup.tar.gz /home/abdallah/Documents
```

---

### 2.2 Using bzip2 compression (`.tar.bz2`)

Provides **better compression** than gzip, but is usually slower.

```bash
tar -cjvf file.tar.bz2 target_file_or_dir
```

- `-j` → Compress using **bzip2**
    

---

## 3. Extracting Compressed Archives

### Extract gzip-compressed archive:

```bash
tar -xzvf file.tar.gz -C output_dir
```

### Extract bzip2-compressed archive:

```bash
tar -xjvf file.tar.bz2 -C output_dir
```

> ✅ `tar` automatically detects the compression type when the correct option is used.

---

## 4. Using `gzip` Command Directly

Unlike `tar`, **`gzip` works on single files** and does **not** archive directories by itself.

### Compress a file:

```bash
gzip file
```

⚠️ This **compresses the file and deletes the original file**, leaving:

```
file.gz
```

---

### Keep the original file (`-k` option):

```bash
gzip -k file
```

This results in:

```
file
file.gz
```

---

### Decompress a gzip file:

```bash
gzip -d file.gz
```

Or equivalently:

```bash
gunzip file.gz
```

---

## 5. Summary Table

|Task|Command|
|---|---|
|Create archive (no compression)|`tar -cvf file.tar dir/`|
|Extract archive|`tar -xvf file.tar`|
|Create gzip archive|`tar -czvf file.tar.gz dir/`|
|Create bzip2 archive|`tar -cjvf file.tar.bz2 dir/`|
|Extract gzip archive|`tar -xzvf file.tar.gz`|
|Extract bzip2 archive|`tar -xjvf file.tar.bz2`|
|Compress single file|`gzip file`|
|Compress & keep original|`gzip -k file`|
|Decompress gzip file|`gzip -d file.gz`|

---

✅ **Tip:** For directories, always use `tar` with compression (`gzip` or `bzip2`). For single files, `gzip` alone is sufficient.