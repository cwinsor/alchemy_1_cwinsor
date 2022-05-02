
The steps in this and other INSTRUCTIONS files allow set up Alchemy.

These steps are based on a fork of the original U.Washington repository
this fork being the cwinsor git repository
https://github.com/cwinsor/alchemy_1_cwinsor.git

The reason for the fork is because the original U.Washington code
did not compile - we suspect it is because versions of C/C++, Ubuntu, etc
have evolved and the code has not maintained.  Only minor fixes
were necessary but still a hassle.

The original U.Washington versions (from their documentation) and the
versions used by cwinsor are:

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

Following these steps allow compiling the C/C++ code, running inference and learning weights and structure. It should be noted the source available from U-W (as of 4/17/2022) does not compile, so a pointer to a github is provided that does compile.

References:
Main site:  https://alchemy.cs.washington.edu/
Source code:  https://alchemy.cs.washington.edu/code/
Datasets:  https://alchemy.cs.washington.edu/data/
Tutorial Dataset:  https://alchemy.cs.washington.edu/data/tutorial/
Publication-based datasets:  https://alchemy.cs.washington.edu/mlns/
