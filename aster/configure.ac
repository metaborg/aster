AC_PREREQ([2.58])
AC_INIT([ASTER],[0.1],[stratego-bugs@cs.uu.nl])
AM_INIT_AUTOMAKE

test "x$prefix" = xNONE && prefix=$ac_default_prefix

m4_pattern_forbid([^XT_])

XT_SETUP
XT_SVN_REVISION
# comment to make this a stable release
XT_PRE_RELEASE

XT_USE_XT_PACKAGES

AC_DISABLE_STATIC
AC_PROG_CC
AC_PROG_LIBTOOL

AC_CONFIG_FILES([
  Makefile
  aster.spec
  syntax/Makefile
  trans/Makefile
  tests/Makefile
  runtime/Makefile
])
AC_OUTPUT

