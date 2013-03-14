#+----------------------------------------------------------------------------+
#| Description:  Magic Set Editor - Program to make Magic (tm) cards          |
#| Copyright:    (C) 2001 - 2007 Twan van Laarhoven and Sean "coppro" Hunt    |
#| License:      GNU General Public License 2 or later (see file COPYING)     |
#+----------------------------------------------------------------------------+

# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([magicseteditor], [0.3.9], [twanvl@users.sourceforge.net])
AC_CONFIG_SRCDIR([src/main.cpp])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build_aux])
# TODO: Actually use the config header
AC_CONFIG_HEADERS([config.h src/config.h])
AC_CANONICAL_HOST
AC_CANONICAL_TARGET
AC_CANONICAL_SYSTEM
AC_USE_SYSTEM_EXTENSIONS

AM_INIT_AUTOMAKE([foreign])
AM_DEP_TRACK
AM_MAINTAINER_MODE
AM_MAKE_INCLUDE
AC_ISC_POSIX
AC_EXEEXT

# Checks for programs.
if test -n "${CXXFLAGS}"; then
  user_set_cxxflags=yes
fi
AC_PROG_CXX
AC_PROG_AWK
AC_PROG_CC
AM_PROG_CC_C_O
AC_PROG_CPP
AC_PROG_GCC_TRADITIONAL
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_RANLIB
AM_PROG_LIBTOOL
AC_LIBTOOL_WIN32_DLL
AC_ARG_ENABLE([debug], [--enable-debug  Enable debug build (requires debug
 versions of wxWidgets and libstdc++.])
if test "x${enable_debug}" = "xyes"; then
  DEFAULT_CXXFLAGS="-ggdb -O0"
  WXCONFIG_FLAGS="--debug"
else
  DEFAULT_CXXFLAGS="-O2"
  WXCONFIG_FLAGS=""
fi

if test X$user_set_cxxflags != Xyes; then
  CXXFLAGS=${DEFAULT_CXXFLAGS}
fi

# Checks for libraries.
AC_CHECK_LIB([hunspell],[Hunspell_create])

# AX_* macros are from the autoconf-archive package
AX_BOOST_BASE([1.37.0])
AX_BOOST_REGEX
AX_CC_MAXOPT
AX_CREATE_STDINT_H
AX_CXX_COMPILE_STDCXX_0X
AX_CXX_HEADER_STDCXX_0X
AX_DLL_STRING
AX_CXX_GNUCXX_HASHMAP
AX_CXX_HAVE_EXT_HASH_MAP
AX_CXX_HAVE_EXT_HASH_SET
AX_CXX_HEADER_PRE_STDCXX
AX_CXX_STLPORT_HASHMAP

# gt_* macros are from gettext
gt_TYPE_WCHAR_T
gt_TYPE_WINT_T

# Check for wxWidgets
AM_OPTIONS_WXCONFIG
AM_PATH_WXCONFIG([2.8.0],[HAVE_WX=1],[HAVE_WX=0],[],${WXCONFIG_FLAGS})
if test "${HAVE_WX}" != 1; then
    AC_MSG_ERROR([
       wxWindows must be installed on your system
       but could not be configured.

       Please check that wx-config is in path, the directory
       where wxWindows libraries are installed (returned by
       'wx-config --libs' command) is in LD_LIBRARY_PATH or
       equivalent variable and wxWindows version is 2.6.0 or
       above. If --enable-debug was passed, please ensure
       debugging libraries are installed.
    ])
fi

WX_CONFIG_OPTIONS

WXRC_CHECK([HAVE_WXRC=1], [HAVE_WXRC=0])
if test "x$HAVE_WXRC" != x1; then
    AC_MSG_ERROR([
           The wxrc program was not installed or not found.

           Please check the wxWidgets installation.
    ])
fi

CPPFLAGS="$CPPFLAGS $WX_CPPFLAGS"
CXXFLAGS="$CXXFLAGS $WX_CXXFLAGS_ONLY"
CFLAGS="$CFLAGS $WX_CFLAGS_ONLY"

LDFLAGS="$LDFLAGS $WX_LIBS"

WX_STANDARD_OPTIONS([--enable-[debug|unicode|shared|wxshared|wxdebug]])
WX_STANDARD_OPTIONS([--with-[gtk|msw|motif|x11|mac|dfb]])
WX_STANDARD_OPTIONS([--with-wxversion])

AC_ARG_ENABLE([pch], [--enable-pch  Enable precompiled headers (Requires
 GCC of sufficiently high version). Speeds up compile times
 significantly... if it works.])

# Check for precompiled headers
# TODO: Deal with braindead GCC and actually check
AM_CONDITIONAL([GLIBCPP_BUILD_PCH], [test "x$enable_pch" = "xyes" -a "x$CXX" = "xg++"])

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([stdint.h stdio.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_SIZE_T
AC_STRUCT_TM
AC_TYPE_LONG_LONG_INT

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_FUNC_SELECT_ARGTYPES
AC_CHECK_FUNCS([floor memset pow select sqrt])

AC_CACHE_CHECK([whether the compiler provides atomic builtins], [ap_cv_atomic_builtins],
[AC_TRY_RUN([
int main()
{
    unsigned long val = 1010, tmp, *mem = &val;

    if (__sync_fetch_and_add(&val, 1010) != 1010 || val != 2020)
        return 1;

    tmp = val;

    if (__sync_fetch_and_sub(mem, 1010) != tmp || val != 1010)
        return 1;

    if (__sync_sub_and_fetch(&val, 1010) != 0 || val != 0)
        return 1;

    tmp = 3030;

    if (__sync_val_compare_and_swap(mem, 0, tmp) != 0 || val != tmp)
        return 1;

    if (__sync_lock_test_and_set(&val, 4040) != 3030)
        return 1;

    mem = &tmp;

    if (__sync_val_compare_and_swap(&mem, &tmp, &val) != &tmp)
        return 1;

    __sync_synchronize();

    if (mem != &val)
        return 1;

    return 0;
}], [ap_cv_atomic_builtins=yes], [ap_cv_atomic_builtins=no], [ap_cv_atomic_builtins=no])])

if test "x$ap_cv_atomic_builtins" = "xyes"; then
    AC_DEFINE([HAVE_GCC_ATOMIC_BUILTINS], [1], [Define if compiler provides atomic builtins])
fi

# we want to always have DEBUG==WX_DEBUG and UNICODE==WX_UNICODE
WX_DEBUG=$DEBUG
WX_UNICODE=$UNICODE

WX_CONVERT_STANDARD_OPTIONS_TO_WXCONFIG_FLAGS
WX_CONFIG_CHECK([2.8.0], [wxWin=1],[],[html,core,net,base],[$WXCONFIG_FLAGS])
WX_DETECT_STANDARD_OPTION_VALUES

AC_CONFIG_FILES([Makefile])
AC_OUTPUT

WX_STANDARD_OPTIONS_SUMMARY_MSG
WX_STANDARD_OPTIONS_SUMMARY_MSG_BEGIN
WX_STANDARD_OPTIONS_SUMMARY_MSG_END
