## dupfi - A Duplicate Finder

_dupfi_ is a pure shell script to find identical files across your drives with
no other dependencies than a *NIX like system.

The collection of files is done by _find_ utility, see that man page for details.
It works in two main steps:

  1. Collect files with same size
  2. Filter files by same checksum

The result list will be saved for further processing or alternatively
immediately printed.

#### Usage

    dupfi [<options>] <path>... [<find-options>]

#### Some options are:

    -d    Be dull, without calc checksum only for first 1M
    -p    Print result list but not save, implies -q
    -q    Be quiet as possible
    -s    Show last or current results with less

### Examples

Well ok, I do not have so much stuff on my laptop, especially no big files where
calculation of the checksum need notable time, but without hidden directories
the check of my home tree need slightly more than 1min.

    [ ~]$ time dupfi . ! -path '*/.*'
    Execute: find  . ! -path \*/.\* -type f -size +0c
    Collect files.....................................[DONE] Found 11679 files
    Sort and filter files by same size................[DONE] Found 6290 candidates
    Calc checksum for file 6290 of 6290...............[DONE]
    Sort and filter files by same checksum............[DONE] Found 2135 duplicates
    Result saved as: /home/lot/.dupfi/duplicates

    real    1m5,496s

When looking only for pdf files the check was done in 1sec.

    [ ~]$ time dupfi . '! -path */.* -iname *.pdf'
    Execute: find  . ! -path \*/.\* -iname \*.pdf -type f -size +0c
    Collect files.....................................[DONE] Found 1065 files
    Sort and filter files by same size................[DONE] Found 27 candidates
    Calc checksum for file 27 of 27...................[DONE]
    Sort and filter files by same checksum............[DONE] Found 20 duplicates
    Result saved as: /home/lot/.dupfi/duplicates

    real    0m1,214s

Running the same check on jpg files need 6sec, whereby it's interesting that all
candidates are duplicates and the checksum calculation take the most time. At
this point I add the option -d which gives sadly only at notable big files a
real benefit.

    [ ~]$ time dupfi . '! -path */.* -iname *.jpg'
    Execute: find  . ! -path \*/.\* -iname \*.jpg -type f -size +0c
    Collect files.....................................[DONE] Found 637 files
    Sort and filter files by same size................[DONE] Found 180 candidates
    Calc checksum for file 180 of 180.................[DONE]
    Sort and filter files by same checksum............[DONE] Found 180 duplicates
    Result saved as: /home/lot/.dupfi/duplicates

    real    0m5,897s

### Install

Copy _dupfi_ somewhere to your _$PATH_ and ensure it is executable, that's all.

### TODOs and Thoughts

  - Think about how to further process the duplicate file
  - Add some more useful examples how to filter something
  - Testing on various platforms

### Credits

The idea to _dupfi_ is inspired by a answers at a [stackexchange thread](https://unix.stackexchange.com/a/71201), so thanks goes there!

All contributors of stackexchange.com, bash-hackers.org, dict.cc and many more.

### License

GNU General Public License (GPL), Version 2.0
