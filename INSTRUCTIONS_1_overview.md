
The steps in this and other INSTRUCTIONS files allow set up Alchemy. These steps are based on a fork of the original U.Washington repository.

If you are interested in the problem (signature and resolutions) see the bottom of this INSTRUCTIONS.

Versions used by original U.Washington (from their documentation) and those of cwinsor are:

```
U-Washington:
Fedora Core 7
Bison 2.3
Flex 2.5.4
g++ 4.1.2
Perl 5.8.8

cwinsor:
Ubuntu 20.04.3
Bision 2:3.5.1
flex 2.6.4
g++ 9.4.0
Perl 5.30.0
```

References:
* Main site:  https://alchemy.cs.washington.edu/
* Source code:  https://alchemy.cs.washington.edu/code/
* Datasets:  https://alchemy.cs.washington.edu/data/
* Tutorial Dataset:  https://alchemy.cs.washington.edu/data/tutorial/
* Publication-based datasets:  https://alchemy.cs.washington.edu/mlns/

#----------------------------------------

Problem signatures fixed in this git:

1. Signature: at runtime - "smokes" predicate should be declared...
  * This is due to non-executable permission on parser .pl files
    * Solution:  chmod 775 parser/replacefolcpp.pl  and chmod 755 parser/replacefoly.pl
    * Summary: The Bison first-order-logic tool uses these .pl files to generate .cpp (parser) files which are then compiled.  The non-execute permissions mean incorrect source is generated, this results in incorrect parsing of the .mln and .db

2. Signature: during compile - cannot call reference to a non-object instance
  * This is a case of using a class method without instancing the class.
  * Fix:
  ```
  int swapwith = Random::randomOneOf(numItems_);
  becomes
  int swapwith = rand() $ numItems_;
```

3. Signature: during compile - cannot compare <file handle> to NULL
  * in this case there is no need for checking the file handle.  The library being used now will throw an exception if the file cannot be opened.  So the solution here is to delete the entire 4 lines of code that check the file handle. 
