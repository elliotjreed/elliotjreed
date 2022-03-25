# Search directory for a word / text via the command line

To list all occurances of a particular string / text in your Bash (or ZSH or Shell) command line, run:
```bash
grep -ni "Text to search for" .
```

This will output the following if it is found:
```bash
./2021-06-12 search a directory for text string via command line.md:5:grep -rni "Text to search for" .
```

You can also search recursively in subdirectories by adding the `r` flag, for example:

```bash
grep -ri "Text to search for" .
```

Which would output the following if the string is found in any file in any directory or subdirectory:

```bash
./blog/2021-06-12 search a directory for text string via command line.md:grep -rni "Text to search for" .
```

If you use this sort of thing a lot, you could always add this to you `~/.profile`, `~/.bashrc`, or `~/.zshrc` in handy aliases like:

```bash
# Search current directory for files containing specified string (Usage: searchdir "Search Term")
searchdir() {
  if [[ $# -eq 0 ]] ; then
    echo -e "\e[0;31mPlease provide a string / search term\e[0m"
  else
    grep -rni "$@" .
  fi
}
```
