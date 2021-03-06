AC_INIT([libusrsctp], [0.1])
AC_CONFIG_SRCDIR([usrsctplib/netinet/sctp.h])
AM_INIT_AUTOMAKE

AC_PROG_CC
AC_PROG_LIBTOOL
AC_CANONICAL_HOST
AC_CONFIG_MACRO_DIR([m4])

CFLAGS="$CFLAGS -Wall -g -O0 -DINET -DUSE_SCTP_SHA1 -DSCTP_PROCESS_LEVEL_LOCKS -DSCTP_SIMPLE_ALLOCATOR -D__Userspace__"

case $host_os in
darwin*)
  CFLAGS="$CFLAGS -U__APPLE__ -D__Userspace_os_Darwin"
  ;;
linux*)
  CFLAGS="$CFLAGS -pthread -D__Userspace_os_Linux"
  ;;
freebsd*)
  CFLAGS="$CFLAGS -pthread -U__FreeBSD__ -D__Userspace_os_FreeBSD"
  ;;
solaris*)
  CFLAGS="$CFLAGS -D_XPG4_2"
  ;;
esac

AC_ARG_ENABLE(debug,
  AC_HELP_STRING( [--enable-debug],
                  [provide debug information @<:@default=yes@:>@]),
    enable_debug=$enableval,enable_debug=yes)
if test x$enable_debug = xyes; then
        AC_DEFINE(SCTP_DEBUG, 1, [Provide debug information])
fi

AC_ARG_ENABLE(debug,
  AC_HELP_STRING( [--enable-callback-api],
                  [use callback API @<:@default=yes@:>@]),
    enable_callback_api=$enableval,enable_callback_api=yes)
if test x$enable_callback_api = xyes; then
        AC_DEFINE(CALLBACK_API, 1, [Use callback API])
fi

AC_CHECK_TYPE(size_t)
AC_CHECK_TYPE(ssize_t)

AC_CHECK_FUNCS(socket, , AC_CHECK_LIB(socket, socket))
AC_CHECK_FUNCS(inet_addr, , AC_CHECK_LIB(nsl, inet_addr))

AC_CHECK_HEADERS(sys/types.h)
AC_CHECK_MEMBER(struct sockaddr_in.sin_len,
                AC_DEFINE(HAVE_SIN_LEN, 1, [Define this if your IPv4 has sin_len in sockaddr_in struct.]),,
                [#ifdef HAVE_SYS_TYPES_H
                 #include <sys/types.h>
                 #endif
                 #include <netinet/in.h>])

AC_MSG_CHECKING(for socklen_t)
AC_TRY_COMPILE([#ifdef HAVE_SYS_TYPES_H
                #include <sys/types.h>
                #endif
                #include <sys/socket.h>],
               [socklen_t x; x = 1; return 0;],
               [AC_MSG_RESULT(yes)],
               [AC_MSG_RESULT(int)
                AC_DEFINE(socklen_t, int, [Define a type for socklen_t.])]) 

AC_OUTPUT(Makefile usrsctplib/Makefile programs/Makefile)
