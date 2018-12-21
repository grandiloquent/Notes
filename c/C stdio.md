# C stdio
 
- [fgetln.c](#fgetlnc)
- [fgetwc.c](#fgetwcc)
- [fgetws.c](#fgetwsc)
- [flags.c](#flagsc)
- [fpurge.c](#fpurgec)
- [fputwc.c](#fputwcc)
- [fputws.c](#fputwsc)
- [fvwrite.c](#fvwritec)
- [fvwrite.h](#fvwriteh)
- [fwalk.c](#fwalkc)
- [fwide.c](#fwidec)
- [getdelim.c](#getdelimc)
- [gets.c](#getsc)
- [makebuf.c](#makebufc)
- [mktemp.c](#mktempc)
- [open_memstream.c](#open_memstreamc)
- [open_wmemstream.c](#open_wmemstreamc)
- [rget.c](#rgetc)
- [setvbuf.c](#setvbufc)
- [tempnam.c](#tempnamc)
- [tmpnam.c](#tmpnamc)
- [ungetc.c](#ungetcc)
- [ungetwc.c](#ungetwcc)
- [vasprintf.c](#vasprintfc)
- [vdprintf.c](#vdprintfc)
- [vsscanf.c](#vsscanfc)
- [vswprintf.c](#vswprintfc)
- [vswscanf.c](#vswscanfc)
- [wbuf.c](#wbufc)
- [wsetup.c](#wsetupc)

## fgetln.c


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "local.h"
/*
 * Expand the line buffer.  Return -1 on error.
 */
static int
__slbexpand(FILE *fp, size_t newsize)
{
	void *p;
	if (fp->_lb._size >= newsize)
		return (0);
	if ((p = realloc(fp->_lb._base, newsize)) == NULL)
		return (-1);
	fp->_lb._base = p;
	fp->_lb._size = newsize;
	return (0);
}
/*
 * Get an input line.  The returned pointer often (but not always)
 * points into a stdio buffer.  Fgetline does not alter the text of
 * the returned line (which is thus not a C string because it will
 * not necessarily end with '\0'), but does allow callers to modify
 * it if they wish.  Thus, we set __SMOD in case the caller does.
 */
char *
fgetln(FILE *fp, size_t *lenp)
{
	unsigned char *p;
	char *ret;
	size_t len;
	size_t off;
	FLOCKFILE(fp);
	_SET_ORIENTATION(fp, -1);
	/* make sure there is input */
	if (fp->_r <= 0 && __srefill(fp))
		goto error;
	/* look for a newline in the input */
	if ((p = memchr((void *)fp->_p, '\n', fp->_r)) != NULL) {
		/*
		 * Found one.  Flag buffer as modified to keep fseek from
		 * `optimising' a backward seek, in case the user stomps on
		 * the text.
		 */
		p++;		/* advance over it */
		ret = (char *)fp->_p;
		*lenp = len = p - fp->_p;
		fp->_flags |= __SMOD;
		fp->_r -= len;
		fp->_p = p;
		FUNLOCKFILE(fp);
		return (ret);
	}
	/*
	 * We have to copy the current buffered data to the line buffer.
	 * As a bonus, though, we can leave off the __SMOD.
	 *
	 * OPTIMISTIC is length that we (optimistically) expect will
	 * accommodate the `rest' of the string, on each trip through the
	 * loop below.
	 */
#define OPTIMISTIC 80
	for (len = fp->_r, off = 0;; len += fp->_r) {
		size_t diff;
		/*
		 * Make sure there is room for more bytes.  Copy data from
		 * file buffer to line buffer, refill file and look for
		 * newline.  The loop stops only when we find a newline.
		 */
		if (__slbexpand(fp, len + OPTIMISTIC))
			goto error;
		(void)memcpy((void *)(fp->_lb._base + off), (void *)fp->_p,
		    len - off);
		off = len;
		if (__srefill(fp))
			break;	/* EOF or error: return partial line */
		if ((p = memchr((void *)fp->_p, '\n', fp->_r)) == NULL)
			continue;
		/* got it: finish up the line (like code above) */
		p++;
		diff = p - fp->_p;
		len += diff;
		if (__slbexpand(fp, len))
			goto error;
		(void)memcpy((void *)(fp->_lb._base + off), (void *)fp->_p,
		    diff);
		fp->_r -= diff;
		fp->_p = p;
		break;
	}
	*lenp = len;
	ret = (char *)fp->_lb._base;
	FUNLOCKFILE(fp);
	return (ret);
error:
	FUNLOCKFILE(fp);
	*lenp = 0;
	return (NULL);
}
```

## fgetwc.c


```c

#include <errno.h>
#include <stdio.h>
#include <wchar.h>
#include "local.h"
wint_t
__fgetwc_unlock(FILE *fp)
{
	struct wchar_io_data *wcio;
	mbstate_t *st;
	wchar_t wc;
	size_t size;
	_SET_ORIENTATION(fp, 1);
	wcio = WCIO_GET(fp);
	if (wcio == 0) {
		errno = ENOMEM;
		return WEOF;
	}
	/* if there're ungetwc'ed wchars, use them */
	if (wcio->wcio_ungetwc_inbuf) {
		wc = wcio->wcio_ungetwc_buf[--wcio->wcio_ungetwc_inbuf];
		return wc;
	}
	st = &wcio->wcio_mbstate_in;
	do {
		char c;
		int ch = __sgetc(fp);
		if (ch == EOF) {
			return WEOF;
		}
		c = ch;
		size = mbrtowc(&wc, &c, 1, st);
		if (size == (size_t)-1) {
			fp->_flags |= __SERR;
			return WEOF;
		}
	} while (size == (size_t)-2);
	return wc;
}
wint_t
fgetwc(FILE *fp)
{
	wint_t r;
	FLOCKFILE(fp);
	r = __fgetwc_unlock(fp);
	FUNLOCKFILE(fp);
	return (r);
}
DEF_STRONG(fgetwc);
```

## fgetws.c


```c

#include <errno.h>
#include <stdio.h>
#include <wchar.h>
#include "local.h"
wchar_t *
fgetws(wchar_t * __restrict ws, int n, FILE * __restrict fp)
{
	wchar_t *wsp;
	wint_t wc;
	FLOCKFILE(fp);
	_SET_ORIENTATION(fp, 1);
	if (n <= 0) {
		errno = EINVAL;
		goto error;
	}
	wsp = ws;
	while (n-- > 1) {
		if ((wc = __fgetwc_unlock(fp)) == WEOF &&
		    ferror(fp) && errno == EILSEQ)
			goto error;
		if (wc == WEOF) {
			if (wsp == ws) {
				/* EOF/error, no characters read yet. */
				goto error;
			}
			break;
		}
		*wsp++ = (wchar_t)wc;
		if (wc == L'\n') {
			break;
		}
	}
	*wsp++ = L'\0';
	FUNLOCKFILE(fp);
	return (ws);
error:
	FUNLOCKFILE(fp);
	return (NULL);
}
DEF_STRONG(fgetws);
```

## flags.c


```c

#include <sys/types.h>
#include <sys/file.h>
#include <stdio.h>
#include <errno.h>
#include "local.h"
/*
 * Return the (stdio) flags for a given mode.  Store the flags
 * to be passed to an open() syscall through *optr.
 * Return 0 on error.
 */
int
__sflags(const char *mode, int *optr)
{
	int ret, m, o;
	switch (*mode++) {
	case 'r':	/* open for reading */
		ret = __SRD;
		m = O_RDONLY;
		o = 0;
		break;
	case 'w':	/* open for writing */
		ret = __SWR;
		m = O_WRONLY;
		o = O_CREAT | O_TRUNC;
		break;
	case 'a':	/* open for appending */
		ret = __SWR;
		m = O_WRONLY;
		o = O_CREAT | O_APPEND;
		break;
	default:	/* illegal mode */
		errno = EINVAL;
		return (0);
	}
	while (*mode != '\0') 
		switch (*mode++) {
		case 'b':
			break;
		case '+':
			ret = __SRW;
			m = O_RDWR;
			break;
		case 'e':
			o |= O_CLOEXEC;
			break;
		case 'x':
			if (o & O_CREAT)
				o |= O_EXCL;
			break;
		default:
			/*
			 * Lots of software passes other extension mode
			 * letters, like Window's 't'
			 */
#if 0
			errno = EINVAL;
			return (0);
#else
			break;
#endif
		}
	*optr = m | o;
	return (ret);
}
```

## fpurge.c


```c

#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include "local.h"
/*
 * fpurge: like fflush, but without writing anything: leave the
 * given FILE's buffer empty.
 */
int
fpurge(FILE *fp)
{
	FLOCKFILE(fp);
	if (!fp->_flags) {
		FUNLOCKFILE(fp);
		errno = EBADF;
		return(EOF);
	}
	if (HASUB(fp))
		FREEUB(fp);
	WCIO_FREE(fp);
	fp->_p = fp->_bf._base;
	fp->_r = 0;
	fp->_w = fp->_flags & (__SLBF|__SNBF) ? 0 : fp->_bf._size;
	FUNLOCKFILE(fp);
	return (0);
}
DEF_WEAK(fpurge);
```

## fputwc.c


```c

#include <errno.h>
#include <limits.h>
#include <stdio.h>
#include <wchar.h>
#include "local.h"
#include "fvwrite.h"
wint_t
__fputwc_unlock(wchar_t wc, FILE *fp)
{
	struct wchar_io_data *wcio;
	mbstate_t *st;
	size_t size;
	char buf[MB_LEN_MAX];
	struct __suio uio;
	struct __siov iov;
	iov.iov_base = buf;
	uio.uio_iov = &iov;
	uio.uio_iovcnt = 1;
	_SET_ORIENTATION(fp, 1);
	wcio = WCIO_GET(fp);
	if (wcio == 0) {
		errno = ENOMEM;
		return WEOF;
	}
	wcio->wcio_ungetwc_inbuf = 0;
	st = &wcio->wcio_mbstate_out;
	size = wcrtomb(buf, wc, st);
	if (size == (size_t)-1) {
		errno = EILSEQ;
		return WEOF;
	}
	uio.uio_resid = iov.iov_len = size;
	if (__sfvwrite(fp, &uio)) {
		return WEOF;
	}
	return (wint_t)wc;
}
wint_t
fputwc(wchar_t wc, FILE *fp)
{
	wint_t r;
	FLOCKFILE(fp);
	r = __fputwc_unlock(wc, fp);
	FUNLOCKFILE(fp);
	return (r);
}
DEF_STRONG(fputwc);
```

## fputws.c


```c

#include <errno.h>
#include <stdio.h>
#include <wchar.h>
#include "local.h"
#include "fvwrite.h"
int
fputws(ws, fp)
	const wchar_t * __restrict ws;
	FILE * __restrict fp;
{
	FLOCKFILE(fp);
	_SET_ORIENTATION(fp, 1);
	while (*ws != '\0') {
		if (__fputwc_unlock(*ws++, fp) == WEOF) {
			FUNLOCKFILE(fp);
			return (-1);
		}
	}
	FUNLOCKFILE(fp);
	return (0);
}
DEF_STRONG(fputws);
```

## fvwrite.c


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include "local.h"
#include "fvwrite.h"
/*
 * Write some memory regions.  Return zero on success, EOF on error.
 *
 * This routine is large and unsightly, but most of the ugliness due
 * to the three different kinds of output buffering is handled here.
 */
int
__sfvwrite(FILE *fp, struct __suio *uio)
{
	size_t len;
	char *p;
	struct __siov *iov;
	int w, s;
	char *nl;
	int nlknown, nldist;
	if ((len = uio->uio_resid) == 0)
		return (0);
	/* make sure we can write */
	if (cantwrite(fp)) {
		errno = EBADF;
		return (EOF);
	}
#define	MIN(a, b) ((a) < (b) ? (a) : (b))
#define	COPY(n)	  (void)memcpy((void *)fp->_p, (void *)p, (size_t)(n))
	iov = uio->uio_iov;
	p = iov->iov_base;
	len = iov->iov_len;
	iov++;
#define GETIOV(extra_work) \
	while (len == 0) { \
		extra_work; \
		p = iov->iov_base; \
		len = iov->iov_len; \
		iov++; \
	}
	if (fp->_flags & __SNBF) {
		/*
		 * Unbuffered: write up to BUFSIZ bytes at a time.
		 */
		do {
			GETIOV(;);
			w = (*fp->_write)(fp->_cookie, p, MIN(len, BUFSIZ));
			if (w <= 0)
				goto err;
			p += w;
			len -= w;
		} while ((uio->uio_resid -= w) != 0);
	} else if ((fp->_flags & __SLBF) == 0) {
		/*
		 * Fully buffered: fill partially full buffer, if any,
		 * and then flush.  If there is no partial buffer, write
		 * one _bf._size byte chunk directly (without copying).
		 *
		 * String output is a special case: write as many bytes
		 * as fit, but pretend we wrote everything.  This makes
		 * snprintf() return the number of bytes needed, rather
		 * than the number used, and avoids its write function
		 * (so that the write function can be invalid).
		 */
		do {
			GETIOV(;);
			if ((fp->_flags & (__SALC | __SSTR)) ==
			    (__SALC | __SSTR) && fp->_w < len) {
				size_t blen = fp->_p - fp->_bf._base;
				unsigned char *_base;
				int _size;
				/* Allocate space exponentially. */
				_size = fp->_bf._size;
				do {
					_size = (_size << 1) + 1;
				} while (_size < blen + len);
				_base = realloc(fp->_bf._base, _size + 1);
				if (_base == NULL)
					goto err;
				fp->_w += _size - fp->_bf._size;
				fp->_bf._base = _base;
				fp->_bf._size = _size;
				fp->_p = _base + blen;
			}
			w = fp->_w;
			if (fp->_flags & __SSTR) {
				if (len < w)
					w = len;
				COPY(w);	/* copy MIN(fp->_w,len), */
				fp->_w -= w;
				fp->_p += w;
				w = len;	/* but pretend copied all */
			} else if (fp->_p > fp->_bf._base && len > w) {
				/* fill and flush */
				COPY(w);
				/* fp->_w -= w; */ /* unneeded */
				fp->_p += w;
				if (__sflush(fp))
					goto err;
			} else if (len >= (w = fp->_bf._size)) {
				/* write directly */
				w = (*fp->_write)(fp->_cookie, p, w);
				if (w <= 0)
					goto err;
			} else {
				/* fill and done */
				w = len;
				COPY(w);
				fp->_w -= w;
				fp->_p += w;
			}
			p += w;
			len -= w;
		} while ((uio->uio_resid -= w) != 0);
	} else {
		/*
		 * Line buffered: like fully buffered, but we
		 * must check for newlines.  Compute the distance
		 * to the first newline (including the newline),
		 * or `infinity' if there is none, then pretend
		 * that the amount to write is MIN(len,nldist).
		 */
		nlknown = 0;
		nldist = 0;	/* XXX just to keep gcc happy */
		do {
			GETIOV(nlknown = 0);
			if (!nlknown) {
				nl = memchr((void *)p, '\n', len);
				nldist = nl ? nl + 1 - p : len + 1;
				nlknown = 1;
			}
			s = MIN(len, nldist);
			w = fp->_w + fp->_bf._size;
			if (fp->_p > fp->_bf._base && s > w) {
				COPY(w);
				/* fp->_w -= w; */
				fp->_p += w;
				if (__sflush(fp))
					goto err;
			} else if (s >= (w = fp->_bf._size)) {
				w = (*fp->_write)(fp->_cookie, p, w);
				if (w <= 0)
				 	goto err;
			} else {
				w = s;
				COPY(w);
				fp->_w -= w;
				fp->_p += w;
			}
			if ((nldist -= w) == 0) {
				/* copied the newline: flush and forget */
				if (__sflush(fp))
					goto err;
				nlknown = 0;
			}
			p += w;
			len -= w;
		} while ((uio->uio_resid -= w) != 0);
	}
	return (0);
err:
	fp->_flags |= __SERR;
	return (EOF);
}
```

## fvwrite.h


```c


```

## fwalk.c


```c

#include <errno.h>
#include <stdio.h>
#include "local.h"
#include "glue.h"
int
_fwalk(int (*function)(FILE *))
{
	FILE *fp;
	int n, ret;
	struct glue *g;
	ret = 0;
	for (g = &__sglue; g != NULL; g = g->next)
		for (fp = g->iobs, n = g->niobs; --n >= 0; fp++) {
			if ((fp->_flags != 0) && ((fp->_flags & __SIGN) == 0))
				ret |= (*function)(fp);
		}
	return (ret);
}
```

## fwide.c


```c

#include <stdio.h>
#include <wchar.h>
#include "local.h"
int
fwide(FILE *fp, int mode)
{
	struct wchar_io_data *wcio;
	/*
	 * this implementation use only -1, 0, 1
	 * for mode value.
	 * (we don't need to do this, but
	 *  this can make things simpler.)
	 */
	if (mode > 0)
		mode = 1;
	else if (mode < 0)
		mode = -1;
	FLOCKFILE(fp);
	wcio = WCIO_GET(fp);
	if (!wcio)
		return 0; /* XXX */
	if (wcio->wcio_mode == 0 && mode != 0)
		wcio->wcio_mode = mode;
	else
		mode = wcio->wcio_mode;
	FUNLOCKFILE(fp);
	return mode;
}
DEF_STRONG(fwide);
```

## getdelim.c


```c

#include <errno.h>
#include <limits.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "local.h"
/* Minimum buffer size we create.
 * This should allow config files to fit into our power of 2 buffer growth
 * without the need for a realloc. */
#define MINBUF	128
ssize_t
getdelim(char **__restrict buf, size_t *__restrict buflen,
    int sep, FILE *__restrict fp)
{
	unsigned char *p;
	size_t len, newlen, off;
	char *newb;
	FLOCKFILE(fp);
	if (buf == NULL || buflen == NULL) {
		errno = EINVAL;
		goto error;
	}
	/* If buf is NULL, we have to assume a size of zero */
	if (*buf == NULL)
		*buflen = 0;
	_SET_ORIENTATION(fp, -1);
	off = 0;
	do {
		/* If the input buffer is empty, refill it */
		if (fp->_r <= 0 && __srefill(fp)) {
			if (__sferror(fp))
				goto error;
			/* No error, so EOF. */
			break;
		}
		/* Scan through looking for the separator */
		p = memchr(fp->_p, sep, (size_t)fp->_r);
		if (p == NULL)
			len = fp->_r;
		else
			len = (p - fp->_p) + 1;
		/* Ensure we can handle it */
		if (off > SSIZE_MAX || len + 1 > SSIZE_MAX - off) {
			errno = EOVERFLOW;
			goto error;
		}
		newlen = off + len + 1; /* reserve space for NUL terminator */
		if (newlen > *buflen) {
			if (newlen < MINBUF)
				newlen = MINBUF;
#define powerof2(x) ((((x)-1)&(x))==0)
			if (!powerof2(newlen)) {
				/* Grow the buffer to the next power of 2 */
				newlen--;
				newlen |= newlen >> 1;
				newlen |= newlen >> 2;
				newlen |= newlen >> 4;
				newlen |= newlen >> 8;
				newlen |= newlen >> 16;
#if SIZE_MAX > 0xffffffffU
				newlen |= newlen >> 32;
#endif
				newlen++;
			}
			newb = realloc(*buf, newlen);
			if (newb == NULL)
				goto error;
			*buf = newb;
			*buflen = newlen;
		}
		(void)memcpy((*buf + off), fp->_p, len);
		/* Safe, len is never greater than what fp->_r can fit. */
		fp->_r -= (int)len;
		fp->_p += (int)len;
		off += len;
	} while (p == NULL);
	FUNLOCKFILE(fp);
	/* POSIX demands we return -1 on EOF. */
	if (off == 0)
		return -1;
	if (*buf != NULL)
		*(*buf + off) = '\0';
	return off;
error:
	fp->_flags |= __SERR;
	FUNLOCKFILE(fp);
	return -1;
}
DEF_WEAK(getdelim);
```

## gets.c


```c

#include <stdio.h>
#include "local.h"
__warn_references(gets,
    "warning: gets() is very unsafe; consider using fgets()");
char *
gets(char *buf)
{
	int c;
	char *s;
	FLOCKFILE(stdin);
	for (s = buf; (c = getchar_unlocked()) != '\n';)
		if (c == EOF)
			if (s == buf) {
				FUNLOCKFILE(stdin);
				return (NULL);
			} else
				break;
		else
			*s++ = c;
	*s = '\0';
	FUNLOCKFILE(stdin);
	return (buf);
}
```

## makebuf.c


```c

#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include "local.h"
/*
 * Allocate a file buffer, or switch to unbuffered I/O.
 * Per the ANSI C standard, ALL tty devices default to line buffered.
 *
 * As a side effect, we set __SOPT or __SNPT (en/dis-able fseek
 * optimisation) right after the fstat() that finds the buffer size.
 */
void
__smakebuf(FILE *fp)
{
	void *p;
	int flags;
	size_t size;
	int couldbetty;
	if (fp->_flags & __SNBF) {
		fp->_bf._base = fp->_p = fp->_nbuf;
		fp->_bf._size = 1;
		return;
	}
	flags = __swhatbuf(fp, &size, &couldbetty);
	if ((p = malloc(size)) == NULL) {
		fp->_flags |= __SNBF;
		fp->_bf._base = fp->_p = fp->_nbuf;
		fp->_bf._size = 1;
		return;
	}
	flags |= __SMBF;
	fp->_bf._base = fp->_p = p;
	fp->_bf._size = size;
	if (couldbetty && isatty(fp->_file))
		flags |= __SLBF;
	fp->_flags |= flags;
}
/*
 * Internal routine to determine `proper' buffering for a file.
 */
int
__swhatbuf(FILE *fp, size_t *bufsize, int *couldbetty)
{
	struct stat st;
	if (fp->_file < 0 || fstat(fp->_file, &st) < 0) {
		*couldbetty = 0;
		*bufsize = BUFSIZ;
		return (__SNPT);
	}
	/* could be a tty iff it is a character device */
	*couldbetty = S_ISCHR(st.st_mode);
	if (st.st_blksize == 0) {
		*bufsize = BUFSIZ;
		return (__SNPT);
	}
	/*
	 * Optimise fseek() only if it is a regular file.  (The test for
	 * __sseek is mainly paranoia.)  It is safe to set _blksize
	 * unconditionally; it will only be used if __SOPT is also set.
	 */
	*bufsize = st.st_blksize;
	fp->_blksize = st.st_blksize;
	return ((st.st_mode & S_IFMT) == S_IFREG && fp->_seek == __sseek ?
	    __SOPT : __SNPT);
}
```

## mktemp.c


```c

#include <sys/types.h>
#include <sys/stat.h>
#include <errno.h>
#include <fcntl.h>
#include <limits.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <unistd.h>
#define MKTEMP_NAME	0
#define MKTEMP_FILE	1
#define MKTEMP_DIR	2
#define TEMPCHARS	"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"
#define NUM_CHARS	(sizeof(TEMPCHARS) - 1)
#define MIN_X		6
#define MKOTEMP_FLAGS	(O_APPEND | O_CLOEXEC | O_DSYNC | O_RSYNC | O_SYNC)
#ifndef nitems
#define nitems(_a)	(sizeof((_a)) / sizeof((_a)[0]))
#endif
static int
mktemp_internal(char *path, int slen, int mode, int flags)
{
	char *start, *cp, *ep;
	const char tempchars[] = TEMPCHARS;
	unsigned int tries;
	struct stat sb;
	size_t len;
	int fd;
	len = strlen(path);
	if (len < MIN_X || slen < 0 || (size_t)slen > len - MIN_X) {
		errno = EINVAL;
		return(-1);
	}
	ep = path + len - slen;
	for (start = ep; start > path && start[-1] == 'X'; start--)
		;
	if (ep - start < MIN_X) {
		errno = EINVAL;
		return(-1);
	}
	if (flags & ~MKOTEMP_FLAGS) {
		errno = EINVAL;
		return(-1);
	}
	flags |= O_CREAT | O_EXCL | O_RDWR;
	tries = INT_MAX;
	do {
		cp = start;
		do {
			unsigned short rbuf[16];
			unsigned int i;
			/*
			 * Avoid lots of arc4random() calls by using
			 * a buffer sized for up to 16 Xs at a time.
			 */
			arc4random_buf(rbuf, sizeof(rbuf));
			for (i = 0; i < nitems(rbuf) && cp != ep; i++)
				*cp++ = tempchars[rbuf[i] % NUM_CHARS];
		} while (cp != ep);
		switch (mode) {
		case MKTEMP_NAME:
			if (lstat(path, &sb) != 0)
				return(errno == ENOENT ? 0 : -1);
			break;
		case MKTEMP_FILE:
			fd = open(path, flags, S_IRUSR|S_IWUSR);
			if (fd != -1 || errno != EEXIST)
				return(fd);
			break;
		case MKTEMP_DIR:
			if (mkdir(path, S_IRUSR|S_IWUSR|S_IXUSR) == 0)
				return(0);
			if (errno != EEXIST)
				return(-1);
			break;
		}
	} while (--tries);
	errno = EEXIST;
	return(-1);
}
char *
_mktemp(char *path)
{
	if (mktemp_internal(path, 0, MKTEMP_NAME, 0) == -1)
		return(NULL);
	return(path);
}
__warn_references(mktemp,
    "warning: mktemp() possibly used unsafely; consider using mkstemp()");
char *
mktemp(char *path)
{
	return(_mktemp(path));
}
int
mkostemps(char *path, int slen, int flags)
{
	return(mktemp_internal(path, slen, MKTEMP_FILE, flags));
}
int
mkstemp(char *path)
{
	return(mktemp_internal(path, 0, MKTEMP_FILE, 0));
}
DEF_WEAK(mkstemp);
int
mkostemp(char *path, int flags)
{
	return(mktemp_internal(path, 0, MKTEMP_FILE, flags));
}
DEF_WEAK(mkostemp);
int
mkstemps(char *path, int slen)
{
	return(mktemp_internal(path, slen, MKTEMP_FILE, 0));
}
char *
mkdtemp(char *path)
{
	int error;
	error = mktemp_internal(path, 0, MKTEMP_DIR, 0);
	return(error ? NULL : path);
}
```

## open_memstream.c


```c

#include <errno.h>
#include <fcntl.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "local.h"
#define	MINIMUM(a, b)	(((a) < (b)) ? (a) : (b))
struct state {
	char		 *string;	/* actual stream */
	char		**pbuf;		/* point to the stream */
	size_t		 *psize;	/* point to min(pos, len) */
	size_t		  pos;		/* current position */
	size_t		  size;		/* number of allocated char */
	size_t		  len;		/* length of the data */
};
static int
memstream_write(void *v, const char *b, int l)
{
	struct state	*st = v;
	char		*p;
	size_t		 i, end;
	end = (st->pos + l);
	if (end >= st->size) {
		/* 1.6 is (very) close to the golden ratio. */
		size_t	sz = st->size * 8 / 5;
		if (sz < end + 1)
			sz = end + 1;
		p = realloc(st->string, sz);
		if (!p)
			return (-1);
		bzero(p + st->size, sz - st->size);
		*st->pbuf = st->string = p;
		st->size = sz;
	}
	for (i = 0; i < l; i++)
		st->string[st->pos + i] = b[i];
	st->pos += l;
	if (st->pos > st->len) {
		st->len = st->pos;
		st->string[st->len] = '\0';
	}
	*st->psize = st->pos;
	return (i);
}
static fpos_t
memstream_seek(void *v, fpos_t off, int whence)
{
	struct state	*st = v;
	ssize_t		 base = 0;
	switch (whence) {
	case SEEK_SET:
		break;
	case SEEK_CUR:
		base = st->pos;
		break;
	case SEEK_END:
		base = st->len;
		break;
	}
	if (off > SIZE_MAX - base || off < -base) {
		errno = EOVERFLOW;
		return (-1);
	}
	st->pos = base + off;
	*st->psize = MINIMUM(st->pos, st->len);
	return (st->pos);
}
static int
memstream_close(void *v)
{
	struct state	*st = v;
	free(st);
	return (0);
}
FILE *
open_memstream(char **pbuf, size_t *psize)
{
	struct state	*st;
	FILE		*fp;
	if (pbuf == NULL || psize == NULL) {
		errno = EINVAL;
		return (NULL);
	}
	if ((st = malloc(sizeof(*st))) == NULL)
		return (NULL);
	if ((fp = __sfp()) == NULL) {
		free(st);
		return (NULL);
	}
	st->size = BUFSIZ;
	if ((st->string = calloc(1, st->size)) == NULL) {
		free(st);
		fp->_flags = 0;
		return (NULL);
	}
	*st->string = '\0';
	st->pos = 0;
	st->len = 0;
	st->pbuf = pbuf;
	st->psize = psize;
	*pbuf = st->string;
	*psize = st->len;
	fp->_flags = __SWR;
	fp->_file = -1;
	fp->_cookie = st;
	fp->_read = NULL;
	fp->_write = memstream_write;
	fp->_seek = memstream_seek;
	fp->_close = memstream_close;
	_SET_ORIENTATION(fp, -1);
	return (fp);
}
DEF_WEAK(open_memstream);
```

## open_wmemstream.c


```c

#include <errno.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>
#include "local.h"
#define	MINIMUM(a, b)	(((a) < (b)) ? (a) : (b))
struct state {
	wchar_t		 *string;	/* actual stream */
	wchar_t		**pbuf;		/* point to the stream */
	size_t		 *psize;	/* point to min(pos, len) */
	size_t		  pos;		/* current position */
	size_t		  size;		/* number of allocated wchar_t */
	size_t		  len;		/* length of the data */
	mbstate_t	  mbs;		/* conversion state of the stream */
};
static int
wmemstream_write(void *v, const char *b, int l)
{
	struct state	*st = v;
	wchar_t		*p;
	size_t		 nmc, len, end;
	end = (st->pos + l);
	if (end >= st->size) {
		/* 1.6 is (very) close to the golden ratio. */
		size_t	sz = st->size * 8 / 5;
		if (sz < end + 1)
			sz = end + 1;
		p = reallocarray(st->string, sz, sizeof(wchar_t));
		if (!p)
			return (-1);
		bzero(p + st->size, (sz - st->size) * sizeof(wchar_t));
		*st->pbuf = st->string = p;
		st->size = sz;
	}
	nmc = (st->size - st->pos) * sizeof(wchar_t);
	len = mbsnrtowcs(st->string + st->pos, &b, nmc, l, &st->mbs);
	if (len == (size_t)-1)
		return (-1);
	st->pos += len;
	if (st->pos > st->len) {
		st->len = st->pos;
		st->string[st->len] = L'\0';
	}
	*st->psize = st->pos;
	return (len);
}
static fpos_t
wmemstream_seek(void *v, fpos_t off, int whence)
{
	struct state	*st = v;
	ssize_t		 base = 0;
	switch (whence) {
	case SEEK_SET:
		break;
	case SEEK_CUR:
		base = st->pos;
		break;
	case SEEK_END:
		base = st->len;
		break;
	}
	if (off > (SIZE_MAX / sizeof(wchar_t)) - base || off < -base) {
		errno = EOVERFLOW;
		return (-1);
	}
	/*
	 * XXX Clearing mbs here invalidates shift state for state-
	 * dependent encodings, but they are not (yet) supported.
	 */
	bzero(&st->mbs, sizeof(st->mbs));
	st->pos = base + off;
	*st->psize = MINIMUM(st->pos, st->len);
	return (st->pos);
}
static int
wmemstream_close(void *v)
{
	struct state	*st = v;
	free(st);
	return (0);
}
FILE *
open_wmemstream(wchar_t **pbuf, size_t *psize)
{
	struct state	*st;
	FILE		*fp;
	if (pbuf == NULL || psize == NULL) {
		errno = EINVAL;
		return (NULL);
	}
	if ((st = malloc(sizeof(*st))) == NULL)
		return (NULL);
	if ((fp = __sfp()) == NULL) {
		free(st);
		return (NULL);
	}
	st->size = BUFSIZ * sizeof(wchar_t);
	if ((st->string = calloc(1, st->size)) == NULL) {
		free(st);
		fp->_flags = 0;
		return (NULL);
	}
	*st->string = L'\0';
	st->pos = 0;
	st->len = 0;
	st->pbuf = pbuf;
	st->psize = psize;
	bzero(&st->mbs, sizeof(st->mbs));
	*pbuf = st->string;
	*psize = st->len;
	fp->_flags = __SWR;
	fp->_file = -1;
	fp->_cookie = st;
	fp->_read = NULL;
	fp->_write = wmemstream_write;
	fp->_seek = wmemstream_seek;
	fp->_close = wmemstream_close;
	_SET_ORIENTATION(fp, 1);
	return (fp);
}
```

## rget.c


```c

#include <stdio.h>
#include "local.h"
/*
 * Handle getc() when the buffer ran out:
 * Refill, then return the first character
 * in the newly-filled buffer.
 */
int
__srget(FILE *fp)
{
	_SET_ORIENTATION(fp, -1);
	if (__srefill(fp) == 0) {
		fp->_r--;
		return (*fp->_p++);
	}
	return (EOF);
}
DEF_STRONG(__srget);
```

## setvbuf.c


```c

#include <stdio.h>
#include <stdlib.h>
#include "local.h"
/*
 * Set one of the three kinds of buffering, optionally including
 * a buffer.
 */
int
setvbuf(FILE *fp, char *buf, int mode, size_t size)
{
	int ret, flags;
	size_t iosize;
	int ttyflag;
	/*
	 * Verify arguments.  The `int' limit on `size' is due to this
	 * particular implementation.  Note, buf and size are ignored
	 * when setting _IONBF.
	 */
	if (mode != _IONBF)
		if ((mode != _IOFBF && mode != _IOLBF) || (int)size < 0)
			return (EOF);
	/*
	 * Write current buffer, if any.  Discard unread input (including
	 * ungetc data), cancel line buffering, and free old buffer if
	 * malloc()ed.  We also clear any eof condition, as if this were
	 * a seek.
	 */
	FLOCKFILE(fp);
	ret = 0;
	(void)__sflush(fp);
	if (HASUB(fp))
		FREEUB(fp);
	WCIO_FREE(fp);
	fp->_r = fp->_lbfsize = 0;
	flags = fp->_flags;
	if (flags & __SMBF)
		free(fp->_bf._base);
	flags &= ~(__SLBF | __SNBF | __SMBF | __SOPT | __SNPT | __SEOF);
	/* If setting unbuffered mode, skip all the hard work. */
	if (mode == _IONBF)
		goto nbf;
	/*
	 * Find optimal I/O size for seek optimization.  This also returns
	 * a `tty flag' to suggest that we check isatty(fd), but we do not
	 * care since our caller told us how to buffer.
	 */
	flags |= __swhatbuf(fp, &iosize, &ttyflag);
	if (size == 0) {
		buf = NULL;	/* force local allocation */
		size = iosize;
	}
	/* Allocate buffer if needed. */
	if (buf == NULL) {
		if ((buf = malloc(size)) == NULL) {
			/*
			 * Unable to honor user's request.  We will return
			 * failure, but try again with file system size.
			 */
			ret = EOF;
			if (size != iosize) {
				size = iosize;
				buf = malloc(size);
			}
		}
		if (buf == NULL) {
			/* No luck; switch to unbuffered I/O. */
nbf:
			fp->_flags = flags | __SNBF;
			fp->_w = 0;
			fp->_bf._base = fp->_p = fp->_nbuf;
			fp->_bf._size = 1;
			FUNLOCKFILE(fp);
			return (ret);
		}
		flags |= __SMBF;
	}
	/*
	 * We're committed to buffering from here, so make sure we've
	 * registered to flush buffers on exit.
	 */
	if (!__sdidinit)
		__sinit();
	/*
	 * Kill any seek optimization if the buffer is not the
	 * right size.
	 *
	 * SHOULD WE ALLOW MULTIPLES HERE (i.e., ok iff (size % iosize) == 0)?
	 */
	if (size != iosize)
		flags |= __SNPT;
	/*
	 * Fix up the FILE fields.
	 */
	if (mode == _IOLBF)
		flags |= __SLBF;
	fp->_flags = flags;
	fp->_bf._base = fp->_p = (unsigned char *)buf;
	fp->_bf._size = size;
	/* fp->_lbfsize is still 0 */
	if (flags & __SWR) {
		/*
		 * Begin or continue writing: see __swsetup().  Note
		 * that __SNBF is impossible (it was handled earlier).
		 */
		if (flags & __SLBF) {
			fp->_w = 0;
			fp->_lbfsize = -fp->_bf._size;
		} else
			fp->_w = size;
	} else {
		/* begin/continue reading, or stay in intermediate state */
		fp->_w = 0;
	}
	FUNLOCKFILE(fp);
	return (ret);
}
DEF_STRONG(setvbuf);
```

## tempnam.c


```c

#include <errno.h>
#include <limits.h>
#include <paths.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
__warn_references(tempnam,
    "warning: tempnam() possibly used unsafely; consider using mkstemp()");
char *
tempnam(const char *dir, const char *pfx)
{
	int sverrno, len;
	char *f, *name;
	if (!(name = malloc(PATH_MAX)))
		return(NULL);
	if (!pfx)
		pfx = "tmp.";
	if (issetugid() == 0 && (f = getenv("TMPDIR")) && *f != '\0') {
		len = snprintf(name, PATH_MAX, "%s%s%sXXXXXXXXXX", f,
		    f[strlen(f) - 1] == '/' ? "" : "/", pfx);
		if (len < 0 || len >= PATH_MAX) {
			errno = ENAMETOOLONG;
			goto fail;
		}
		if ((f = _mktemp(name)))
			return(f);
	}
	if (dir != NULL) {
		f = *dir ? (char *)dir : ".";
		len = snprintf(name, PATH_MAX, "%s%s%sXXXXXXXXXX", f,
		    f[strlen(f) - 1] == '/' ? "" : "/", pfx);
		if (len < 0 || len >= PATH_MAX) {
			errno = ENAMETOOLONG;
			goto fail;
		}
		if ((f = _mktemp(name)))
			return(f);
	}
	f = P_tmpdir;
	len = snprintf(name, PATH_MAX, "%s%sXXXXXXXXX", f, pfx);
	if (len < 0 || len >= PATH_MAX) {
		errno = ENAMETOOLONG;
		goto fail;
	}
	if ((f = _mktemp(name)))
		return(f);
	f = _PATH_TMP;
	len = snprintf(name, PATH_MAX, "%s%sXXXXXXXXX", f, pfx);
	if (len < 0 || len >= PATH_MAX) {
		errno = ENAMETOOLONG;
		goto fail;
	}
	if ((f = _mktemp(name)))
		return(f);
fail:
	sverrno = errno;
	free(name);
	errno = sverrno;
	return(NULL);
}
```

## tmpnam.c


```c

#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>
__warn_references(tmpnam,
    "warning: tmpnam() possibly used unsafely; consider using mkstemp()");
char *
tmpnam(char *s)
{
	static u_long tmpcount;
	static char buf[L_tmpnam];
	if (s == NULL)
		s = buf;
	(void)snprintf(s, L_tmpnam, "%stmp.%lu.XXXXXXXXX", P_tmpdir, tmpcount);
	++tmpcount;
	return (_mktemp(s));
}
```

## ungetc.c


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "local.h"
static int __submore(FILE *);
/*
 * Expand the ungetc buffer `in place'.  That is, adjust fp->_p when
 * the buffer moves, so that it points the same distance from the end,
 * and move the bytes in the buffer around as necessary so that they
 * are all at the end (stack-style).
 */
static int
__submore(FILE *fp)
{
	int i;
	unsigned char *p;
	if (_UB(fp)._base == fp->_ubuf) {
		/*
		 * Get a new buffer (rather than expanding the old one).
		 */
		if ((p = malloc(BUFSIZ)) == NULL)
			return (EOF);
		_UB(fp)._base = p;
		_UB(fp)._size = BUFSIZ;
		p += BUFSIZ - sizeof(fp->_ubuf);
		for (i = sizeof(fp->_ubuf); --i >= 0;)
			p[i] = fp->_ubuf[i];
		fp->_p = p;
		return (0);
	}
	i = _UB(fp)._size;
	p = reallocarray(_UB(fp)._base, i, 2);
	if (p == NULL)
		return (EOF);
	/* no overlap (hence can use memcpy) because we doubled the size */
	(void)memcpy(p + i, p, i);
	fp->_p = p + i;
	_UB(fp)._base = p;
	_UB(fp)._size = i * 2;
	return (0);
}
int
ungetc(int c, FILE *fp)
{
	if (c == EOF)
		return (EOF);
	if (!__sdidinit)
		__sinit();
	FLOCKFILE(fp);
	_SET_ORIENTATION(fp, -1);
	if ((fp->_flags & __SRD) == 0) {
		/*
		 * Not already reading: no good unless reading-and-writing.
		 * Otherwise, flush any current write stuff.
		 */
		if ((fp->_flags & __SRW) == 0) {
error:			FUNLOCKFILE(fp);
			return (EOF);
		}
		if (fp->_flags & __SWR) {
			if (__sflush(fp))
				goto error;
			fp->_flags &= ~__SWR;
			fp->_w = 0;
			fp->_lbfsize = 0;
		}
		fp->_flags |= __SRD;
	}
	c = (unsigned char)c;
	/*
	 * If we are in the middle of ungetc'ing, just continue.
	 * This may require expanding the current ungetc buffer.
	 */
	if (HASUB(fp)) {
		if (fp->_r >= _UB(fp)._size && __submore(fp))
			goto error;
		*--fp->_p = c;
inc_ret:	fp->_r++;
		FUNLOCKFILE(fp);
		return (c);
	}
	fp->_flags &= ~__SEOF;
	/*
	 * If we can handle this by simply backing up, do so,
	 * but never replace the original character.
	 * (This makes sscanf() work when scanning `const' data.)
	 */
	if (fp->_bf._base != NULL && fp->_p > fp->_bf._base &&
	    fp->_p[-1] == c) {
		fp->_p--;
		goto inc_ret;
	}
	/*
	 * Create an ungetc buffer.
	 * Initially, we will use the `reserve' buffer.
	 */
	fp->_ur = fp->_r;
	fp->_up = fp->_p;
	_UB(fp)._base = fp->_ubuf;
	_UB(fp)._size = sizeof(fp->_ubuf);
	fp->_ubuf[sizeof(fp->_ubuf) - 1] = c;
	fp->_p = &fp->_ubuf[sizeof(fp->_ubuf) - 1];
	fp->_r = 1;
	FUNLOCKFILE(fp);
	return (c);
}
DEF_STRONG(ungetc);
```

## ungetwc.c


```c

#include <errno.h>
#include <stdio.h>
#include <wchar.h>
#include "local.h"
wint_t
__ungetwc(wint_t wc, FILE *fp)
{
	struct wchar_io_data *wcio;
	if (wc == WEOF)
		return WEOF;
	_SET_ORIENTATION(fp, 1);
	/*
	 * XXX since we have no way to transform a wchar string to
	 * a char string in reverse order, we can't use ungetc.
	 */
	/* XXX should we flush ungetc buffer? */
	wcio = WCIO_GET(fp);
	if (wcio == 0) {
		errno = ENOMEM; /* XXX */
		return WEOF;
	}
	if (wcio->wcio_ungetwc_inbuf >= WCIO_UNGETWC_BUFSIZE) {
		return WEOF;
	}
	wcio->wcio_ungetwc_buf[wcio->wcio_ungetwc_inbuf++] = wc;
	__sclearerr(fp);
	return wc;
}
wint_t
ungetwc(wint_t wc, FILE *fp)
{
	wint_t r;
	FLOCKFILE(fp);
	r = __ungetwc(wc, fp);
	FUNLOCKFILE(fp);
	return (r);
}
DEF_STRONG(ungetwc);
```

## vasprintf.c


```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include "local.h"
int
vasprintf(char **str, const char *fmt, __va_list ap)
{
	int ret;
	FILE f;
	struct __sfileext fext;
	unsigned char *_base;
	_FILEEXT_SETUP(&f, &fext);
	f._file = -1;
	f._flags = __SWR | __SSTR | __SALC;
	f._bf._base = f._p = malloc(128);
	if (f._bf._base == NULL)
		goto err;
	f._bf._size = f._w = 127;		/* Leave room for the NUL */
	ret = __vfprintf(&f, fmt, ap);
	if (ret == -1)
		goto err;
	*f._p = '\0';
	_base = realloc(f._bf._base, ret + 1);
	if (_base == NULL)
		goto err;
	*str = (char *)_base;
	return (ret);
err:
	free(f._bf._base);
	f._bf._base = NULL;
	*str = NULL;
	errno = ENOMEM;
	return (-1);
}
DEF_WEAK(vasprintf);
```

## vdprintf.c


```c

#include <errno.h>
#include <stdarg.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include "local.h"
static int
__dwrite(void *cookie, const char *buf, int n)
{
	int *fdp = cookie;
	return (write(*fdp, buf, n));
}
int
vdprintf(int fd, const char * __restrict fmt, va_list ap)
{
	FILE f;
	struct __sfileext fext;
	unsigned char buf[BUFSIZ];
	int ret;
	_FILEEXT_SETUP(&f, &fext);
	f._p = buf;
	f._w = sizeof(buf);
	f._flags = __SWR;
	f._file = -1;
	f._bf._base = buf;
	f._bf._size = sizeof(buf);
	f._cookie = &fd;
	f._write = __dwrite;
	if ((ret = __vfprintf(&f, fmt, ap)) < 0)
		return ret;
	return fflush(&f) ? EOF : ret;
}
DEF_WEAK(vdprintf);
```

## vsscanf.c


```c

#include <stdio.h>
#include <string.h>
#include "local.h"
static int
eofread(void *cookie, char *buf, int len)
{
	return (0);
}
int
vsscanf(const char *str, const char *fmt, __va_list ap)
{
	FILE f;
	struct __sfileext fext;
	_FILEEXT_SETUP(&f, &fext);
	f._flags = __SRD;
	f._bf._base = f._p = (unsigned char *)str;
	f._bf._size = f._r = strlen(str);
	f._read = eofread;
	f._lb._base = NULL;
	return (__svfscanf(&f, fmt, ap));
}
DEF_STRONG(vsscanf);
```

## vswprintf.c


```c

#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>
#include <stdarg.h>
#include "local.h"
int
vswprintf(wchar_t * __restrict s, size_t n, const wchar_t * __restrict fmt,
    __va_list ap)
{
	mbstate_t mbs;
	FILE f;
	char *mbp;
	int ret, sverrno;
	size_t nwc;
	struct __sfileext fext;
	if (n == 0) {
		errno = EINVAL;
		return (-1);
	}
	_FILEEXT_SETUP(&f, &fext);
	f._file = -1;
	f._flags = __SWR | __SSTR | __SALC;
	f._bf._base = f._p = malloc(128);
	if (f._bf._base == NULL) {
		errno = ENOMEM;
		return (-1);
	}
	f._bf._size = f._w = 127;		/* Leave room for the NUL */
	ret = __vfwprintf(&f, fmt, ap);
	if (ret < 0) {
		sverrno = errno;
		free(f._bf._base);
		errno = sverrno;
		return (-1);
	}
	if (ret == 0) {
		s[0] = L'\0';
		free(f._bf._base);
		return (0);
	}
	*f._p = '\0';
	mbp = (char *)f._bf._base;
	/*
	 * XXX Undo the conversion from wide characters to multibyte that
	 * fputwc() did in __vfwprintf().
	 */
	bzero(&mbs, sizeof(mbs));
	nwc = mbsrtowcs(s, (const char **)&mbp, n, &mbs);
	free(f._bf._base);
	if (nwc == (size_t)-1) {
		errno = EILSEQ;
		return (-1);
	}
	if (nwc == n) {
		s[n - 1] = L'\0';
		errno = EOVERFLOW;
		return (-1);
	}
	return (ret);
}
DEF_STRONG(vswprintf);
```

## vswscanf.c


```c

#include <limits.h>
#include <stdarg.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>
#include "local.h"
static int	eofread(void *, char *, int);
static int
eofread(void *cookie, char *buf, int len)
{
	return (0);
}
int
vswscanf(const wchar_t * __restrict str, const wchar_t * __restrict fmt,
    __va_list ap)
{
	mbstate_t mbs;
	FILE f;
	struct __sfileext fext;
	char *mbstr;
	size_t len, mlen;
	int r;
	const wchar_t *strp;
	/*
	 * XXX Convert the wide character string to multibyte, which
	 * __vfwscanf() will convert back to wide characters.
	 */
	len = wcslen(str) * MB_CUR_MAX;
	if ((mbstr = malloc(len + 1)) == NULL)
		return (EOF);
	bzero(&mbs, sizeof(mbs));
	strp = str;
	if ((mlen = wcsrtombs(mbstr, &strp, len, &mbs)) == (size_t)-1) {
		free(mbstr);
		return (EOF);
	}
	if (mlen == len)
		mbstr[len] = '\0';
	_FILEEXT_SETUP(&f, &fext);
	f._flags = __SRD;
	f._bf._base = f._p = (unsigned char *)mbstr;
	f._bf._size = f._r = mlen;
	f._read = eofread;
	f._lb._base = NULL;
	r = __vfwscanf(&f, fmt, ap);
	free(mbstr);
	return (r);
}
DEF_STRONG(vswscanf);
```

## wbuf.c


```c

#include <stdio.h>
#include <errno.h>
#include "local.h"
/*
 * Write the given character into the (probably full) buffer for
 * the given file.  Flush the buffer out if it is or becomes full,
 * or if c=='\n' and the file is line buffered.
 */
int
__swbuf(int c, FILE *fp)
{
	int n;
	_SET_ORIENTATION(fp, -1);
	/*
	 * In case we cannot write, or longjmp takes us out early,
	 * make sure _w is 0 (if fully- or un-buffered) or -_bf._size
	 * (if line buffered) so that we will get called again.
	 * If we did not do this, a sufficient number of putc()
	 * calls might wrap _w from negative to positive.
	 */
	fp->_w = fp->_lbfsize;
	if (cantwrite(fp)) {
		errno = EBADF;
		return (EOF);
	}
	c = (unsigned char)c;
	/*
	 * If it is completely full, flush it out.  Then, in any case,
	 * stuff c into the buffer.  If this causes the buffer to fill
	 * completely, or if c is '\n' and the file is line buffered,
	 * flush it (perhaps a second time).  The second flush will always
	 * happen on unbuffered streams, where _bf._size==1; __sflush()
	 * guarantees that putc() will always call wbuf() by setting _w
	 * to 0, so we need not do anything else.
	 */
	n = fp->_p - fp->_bf._base;
	if (n >= fp->_bf._size) {
		if (__sflush(fp))
			return (EOF);
		n = 0;
	}
	fp->_w--;
	*fp->_p++ = c;
	if (++n == fp->_bf._size || (fp->_flags & __SLBF && c == '\n'))
		if (__sflush(fp))
			return (EOF);
	return (c);
}
DEF_STRONG(__swbuf);
```

## wsetup.c


```c

#include <stdio.h>
#include <stdlib.h>
#include "local.h"
/*
 * Various output routines call wsetup to be sure it is safe to write,
 * because either _flags does not include __SWR, or _buf is NULL.
 * _wsetup returns 0 if OK to write, nonzero otherwise.
 */
int
__swsetup(FILE *fp)
{
	/* make sure stdio is set up */
	if (!__sdidinit)
		__sinit();
	/*
	 * If we are not writing, we had better be reading and writing.
	 */
	if ((fp->_flags & __SWR) == 0) {
		if ((fp->_flags & __SRW) == 0)
			return (EOF);
		if (fp->_flags & __SRD) {
			/* clobber any ungetc data */
			if (HASUB(fp))
				FREEUB(fp);
			fp->_flags &= ~(__SRD|__SEOF);
			fp->_r = 0;
			fp->_p = fp->_bf._base;
		}
		fp->_flags |= __SWR;
	}
	/*
	 * Make a buffer if necessary, then set _w.
	 */
	if (fp->_bf._base == NULL) {
		if ((fp->_flags & (__SSTR | __SALC)) == __SSTR)
			return (EOF);
		__smakebuf(fp);
	}
	if (fp->_flags & __SLBF) {
		/*
		 * It is line buffered, so make _lbfsize be -_bufsize
		 * for the putc() macro.  We will change _lbfsize back
		 * to 0 whenever we turn off __SWR.
		 */
		fp->_w = 0;
		fp->_lbfsize = -fp->_bf._size;
	} else
		fp->_w = fp->_flags & __SNBF ? 0 : fp->_bf._size;
	return (0);
}
```


