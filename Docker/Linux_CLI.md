# CLI

## sed -i
SED command in UNIX stands for stream editor and it can perform lots of functions on file like searching, find and replace, insertion or deletion.

### Syntax:
`sed OPTIONS... [SCRIPT] [INPUTFILE...] `

### Sample Commands

### Replacing or substituting string :

`$sed 's/unix/linux/' geekfile.txt`

Here the “s” specifies the substitution operation. The “/” are delimiters. The “unix” is the search pattern and the “linux” is the replacement string.

By default, the sed command replaces the first occurrence of the pattern in each line and it won’t replace the second, third…occurrence in the line.
Replacing all the occurrence of the pattern in a line : `$sed 's/unix/linux/g' geekfile.txt`
Use -i to edit files in-place instead of printing to standard output.
Reference: [https://phoenixnap.com/kb/linux-sed](https://phoenixnap.com/kb/linux-sed)

## apt-get

apt-get - APT package handling utility - command-line interface
Reference: [https://linux.die.net/man/8/apt-get](https://linux.die.net/man/8/apt-get)

## wget
Wget is a free software package for retrieving files using HTTP, HTTPS, FTP and FTPS, the most widely used Internet protocols. It is a non-interactive commandline tool, so it may easily be called from scripts, cron jobs, terminals without X-Windows support, etc.
Reference: [https://www.gnu.org/software/wget/manual/](https://www.gnu.org/software/wget/manual/)