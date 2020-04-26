# Bash Commands

### Get full directory name
```bash
dirname $(readlink -f "$0")
```

Output
```bash
$ /path/to/dir/script.sh
/path/to/dir
```

### Get script name
```bash
basename $(readlink -f "$0")
```

Output
```bash
$ /path/to/dir/script.sh
script.sh
```
