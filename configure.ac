#+------------------------------------------------------------------------+
#| Description:  Magic Set Editor - Program to make Magic (tm) cards      |
#| Copyright:   (C) 2001 - 2007 Twan van Laarhoven and Sean "coppro" Hunt |
#| License:      GNU General Public License 2 or later (see file COPYING) |
#+------------------------------------------------------------------------+

dnl# -*- Autoconf -*-
dnl# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([magicseteditor],[0.3.9],[twanvl@users.sourceforge.net])
AC_CONFIG_SRCDIR([src/main.cpp])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build_aux])
# TODO: Actually use the config header
AC_CONFIG_HEADERS([config.h src/config.h])
AC_CANONICAL_HOST
AC_CANONICAL_TARGET
AC_USE_SYSTEM_EXTENSIONS
AC_SYS_LARGEFILE

# Automake
AM_INIT_AUTOMAKE([1.9 foreign subdir-objects])
AM_DEP_TRACK
AM_MAINTAINER_MODE
AM_MAKE_INCLUDE
AM_SANITY_CHECK
AC_SEARCH_LIBS([strerror],[cposix]) dnl# used to be AC\_ISC\_POSIX
AC_EXEEXT
AC_OBJEXT

# Checks for programs.
if test -n "${CXXFLAGS}"; then
  user_set_cxxflags=yes
fi
AC_PROG_CXX
AC_LANG([C++])
if test "x${AWK}" = "x"; then
  test -z "${AWK}" || unset AWK
  AC_PROG_AWK
else
  test -n "${AWK}" && export AWK
  AC_SUBST([AWK])
fi
if test "x${CC}" = "x"; then
  test -z "${CC}" || unset CC
  AC_PROG_CC
  if test "x${CC_C_O}" = "x"; then
    test -z "${CC_C_O}" || unset CC_C_O
    AM_PROG_CC_C_O
  else
    test -n "${CC_C_O}" && export CC_C_O
  fi
else
  test -n "${CC}" && export CC
  AC_SUBST([CC])
fi
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_OBJCXX
AC_PROG_OBJCXXCPP
LT_INIT([win32-dll])
LT_LANG([C++])
LT_LANG([Windows Resource])
if test "x${RANLIB}" = "x"; then
  test -z "${RANLIB}" || unset RANLIB
  AC_PROG_RANLIB
else
  test -n "${RANLIB}" && export RANLIB
  AC_SUBST([RANLIB])
fi
AC_ARG_ENABLE([debug],[
  AS_HELP_STRING([--enable-debug],
             [Enable debug build (requires debug versions of wxWidgets/libstdc++)])
])
if test "x${enable_debug}" = "xyes"; then
  DEFAULT_CXXFLAGS="-ggdb -O0"
  WXCONFIG_FLAGS="--debug"
else
  DEFAULT_CXXFLAGS="-O2"
  WXCONFIG_FLAGS=""
fi

if test "X${user_set_cxxflags}" != "Xyes"; then
  CXXFLAGS=${DEFAULT_CXXFLAGS}
fi

# Checks for libraries.
LT_LIB_M
AC_CHECK_LIB([c],[printf])
AC_CHECK_LIB([hunspell],[Hunspell_create])
AM_GNU_GETTEXT([external])
AM_GNU_GETTEXT_VERSION([0.18.3])

# AX_* macros are from the autoconf-archive package
AX_BOOST_BASE([1.37.0])
AX_BOOST_REGEX
AX_BOOST_THREAD
AX_CC_MAXOPT
AX_CREATE_STDINT_H
AC_DEFUN([AX_REQUIRE_CXX_0X],[
  AC_REQUIRE([AC_PROG_CXXCPP])dnl
  AC_REQUIRE([AC_PROG_CXX_C_O])dnl
  AC_REQUIRE([AX_CXX_COMPILE_STDCXX_11])dnl
  m4_ifdef([AX_CXX_HEADER_STDCXX_11],[
    AC_REQUIRE([AX_CXX_HEADER_STDCXX_11])dnl
  ],[
    m4_ifdef([AX_CXX_HEADER_STDCXX_0X],[
      dnl# it needs this one, too:
      m4_ifdef([AX_CXX_COMPILE_STDCXX_0X],[
        AC_REQUIRE([AX_CXX_HEADER_STDCXX_0X])dnl
      ])dnl
    ])dnl
  ])dnl
])dnl
AX_REQUIRE_CXX_0X
AX_DLL_STRING
AX_CXX_GCC_ABI_DEMANGLE
AX_CXX_GNUCXX_HASHMAP
AX_CXX_HAVE_EXT_HASH_MAP
AX_CXX_HAVE_EXT_HASH_SET
AX_CXX_HEADER_PRE_STDCXX
AX_CXX_RTTI
AX_CXX_STLPORT_HASHMAP
AX_PTHREAD
AC_LANG_PUSH([C++])
AX_CHECK_COMPILE_FLAG([-frtti],[export CXXFLAGS="${CXXFLAGS} -frtti"])
AC_LANG_POP

# gt_* macros are from gettext
# gl_* macros are from gnulib
AC_DEFUN([AC_POSSIBLE_DUPLICATE_GETTEXT_CHECKS],[
  AC_REQUIRE([gt_TYPE_WCHAR_T])dnl
  AC_REQUIRE([gt_TYPE_WINT_T])dnl
  AC_REQUIRE([gl_THREADLIB])dnl
  AC_REQUIRE([AC_PROG_YACC])dnl
  AC_CHECK_HEADERS([argz.h fcntl.h float.h langinfo.h libintl.h limits.h \
                    locale.h malloc.h malloc/malloc.h stddef.h \
                    stdio_ext.h wchar.h])dnl
  AC_REQUIRE([AC_FUNC_ALLOCA])dnl
  AC_REQUIRE([AC_FUNC_MBRTOWC])dnl
  AC_REQUIRE([AC_FUNC_MMAP])dnl
  AC_REQUIRE([AC_TYPE_UINT8_T])dnl
  AC_REQUIRE([AC_TYPE_UINT16_T])dnl
  AC_REQUIRE([AC_TYPE_UINT32_T])dnl
  AC_CHECK_TYPES([ptrdiff_t])dnl
  AC_CHECK_FUNCS([__argz_count __argz_next __argz_stringify getcwd getwd \
                  localeconv mblen mbrlen memchr mempcpy munmap \
                  nl_langinfo setlocale stpcpy strcasecmp strchr strcspn \
                  strdup strstr strtoul])dnl
])dnl
AC_POSSIBLE_DUPLICATE_GETTEXT_CHECKS

# Bakefile checks
BAKEFILE_HOST="${host}"
AC_BAKEFILE_GNUMAKE
AC_BAKEFILE_PLATFORM
AC_BAKEFILE_PLATFORM_SPECIFICS
AC_BAKEFILE_SUFFIXES
AC_BAKEFILE_SHARED_VERSIONS
AC_BAKEFILE_RES_COMPILERS
AC_BAKEFILE_METROWERKS_EXTO

# Check for wxWidgets
AM_OPTIONS_WXCONFIG
AM_PATH_WXCONFIG([2.8.0],[HAVE_WX=1],[HAVE_WX=0],[],[${WXCONFIG_FLAGS}])
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

WXRC_CHECK([HAVE_WXRC=1],[HAVE_WXRC=0])
if test "x${HAVE_WXRC}" != "x1"; then
    AC_MSG_ERROR([
           The wxrc program was not installed or not found.

           Please check the wxWidgets installation.
    ])
fi

CPPFLAGS="${CPPFLAGS} ${WX_CPPFLAGS}"
CXXFLAGS="${CXXFLAGS} ${WX_CXXFLAGS_ONLY}"
CFLAGS="${CFLAGS} ${WX_CFLAGS_ONLY}"

LDFLAGS="${LDFLAGS} ${WX_LIBS}"

WX_STANDARD_OPTIONS([--enable-[debug|unicode|shared|wxshared|wxdebug]])
WX_STANDARD_OPTIONS([--with-[gtk|msw|motif|x11|mac|dfb]])
WX_STANDARD_OPTIONS([--with-wxversion])dnl

AC_ARG_ENABLE([pch],[AS_HELP_STRING([--enable-pch],[
 Enable precompiled headers (Requires GCC of sufficiently
 high version). Speeds up compile times significantly...
 if it works.])
])dnl

# Check for precompiled headers
# TODO: Deal with braindead GCC and actually check
AM_CONDITIONAL([GLIBCPP_BUILD_PCH],
               [test "x${enable_pch}" = "xyes" -a "x${CXX}" = "xg++"])dnl

# Checks for header files.
AC_HEADER_STDBOOL dnl# also "_CHECK"s it
AC_DEFUN([AC_REQUIRE_HEADER_STDC],[
  AC_REQUIRE([AC_PROG_MAKE_SET])dnl
  AC_REQUIRE([AC_HEADER_STDC])dnl
])dnl
AC_REQUIRE_HEADER_STDC
AC_CHECK_HEADERS([boost/preprocessor/cat.hpp errno.h error.h stdio.h tchar.h \
                  windows.h wx/generic/imaglist.h wx/image.h wx/imaglist.h])dnl

# Checks for typedefs, structures, and compiler characteristics.
AC_C_BIGENDIAN
AC_C_INLINE
AC_C_PROTOTYPES
AC_TYPE_SIZE_T
AC_STRUCT_TM
AC_TYPE_LONG_LONG_INT

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_FUNC_SELECT_ARGTYPES
AC_CHECK_FUNCS([floor memset pow select sqrt strcmp])dnl

AC_CACHE_CHECK([whether the compiler provides atomic builtins],[ap_cv_atomic_builtins],
[AC_RUN_IFELSE([AC_LANG_SOURCE([[
int main(void)
{
    unsigned long val = 1010, tmp, *mem = &val;

    if ((__sync_fetch_and_add(&val, 1010) != 1010) || (val != 2020))
        return 1;

    tmp = val;

    if ((__sync_fetch_and_sub(mem, 1010) != tmp) || (val != 1010))
        return 1;

    if ((__sync_sub_and_fetch(&val, 1010) != 0) || (val != 0))
        return 1;

    tmp = 3030;

    if ((__sync_val_compare_and_swap(mem, 0, tmp) != 0) || (val != tmp))
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
}]])],[ap_cv_atomic_builtins=yes],[ap_cv_atomic_builtins=no],[ap_cv_atomic_builtins=cross])])dnl

if test "x${ap_cv_atomic_builtins}" = "xyes"; then
    AC_DEFINE([HAVE_GCC_ATOMIC_BUILTINS],[1],
              [Define if compiler provides atomic builtins])
fi

# we want to always have DEBUG==WX_DEBUG and UNICODE==WX_UNICODE
WX_DEBUG=${DEBUG}
WX_UNICODE=${UNICODE}

WX_CONVERT_STANDARD_OPTIONS_TO_WXCONFIG_FLAGS
WX_CONFIG_CHECK([2.8.0],[wxWin=1],[],[html,core,net,base],[${WXCONFIG_FLAGS}])
WX_DETECT_STANDARD_OPTION_VALUES

# Output.
AC_CONFIG_FILES([Makefile po/Makefile.in po/Makevars])
AC_OUTPUT

WX_STANDARD_OPTIONS_SUMMARY_MSG
WX_STANDARD_OPTIONS_SUMMARY_MSG_BEGIN
WX_STANDARD_OPTIONS_SUMMARY_MSG_END

# EOF
