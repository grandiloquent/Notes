# C/C++ string

- [memchr](#memchr)
- [memrchr](#memrchr)
- [stpcpy](#stpcpy)
- [stpncpy](#stpncpy)
- [strcasecmp](#strcasecmp)
- [strcat](#strcat)
- [strcmp](#strcmp)
- [strcpy](#strcpy)
- [strcspn](#strcspn)
- [strdup](#strdup)
- [strlcat](#strlcat)
- [strlcpy](#strlcpy)
- [strncat](#strncat)
- [strncmp](#strncmp)
- [strncpy](#strncpy)
- [strndup](#strndup)
- [strpbrk](#strpbrk)
- [strsep](#strsep)
- [strspn](#strspn)
- [strstr](#strstr)
- [strtok](#strtok)
- [wcpy](#wcpy)
- [wcppy](#wcppy)
- [wcscasmp](#wcscasmp)
- [wcat](#wcat)
- [wchr](#wchr)
- [wcmp](#wcmp)
- [wcpy](#wcpy)
- [wcspn](#wcspn)
- [wcsdup](#wcsdup)
- [wcsat](#wcsat)
- [wcslcpy](#wcslcpy)
- [wcslen](#wcslen)
- [wcsncasmp](#wcsncasmp)
- [wcsat](#wcsat)
- [wcsmp](#wcsmp)
- [wcspy](#wcspy)
- [wcsnlen](#wcsnlen)
- [wcspbrk](#wcspbrk)
- [wcshr](#wcshr)
- [wcsspn](#wcsspn)
- [wcsstr](#wcsstr)
- [wcstok](#wcstok)
- [wcswidth](#wcswidth)
- [wmehr](#wmehr)
- [wmemp](#wmemp)
- [wmepy](#wmepy)
- [wmemmove](#wmemmove)
- [wmemset](#wmemset)
 
## memchr

```c

#include <string.h>
void *
memchr(const void *s, int c, size_t n)
{
	if (n != 0) {
		const unsigned char *p = s;
		do {
			if (*p++ == (unsigned char)c)
				return ((void *)(p - 1));
		} while (--n != 0);
	}
	return (NULL);
}
DEF_STRONG(memchr);
```

## memrchr

```c

#include <string.h>
/*
 * Reverse memchr()
 * Find the last occurrence of 'c' in the buffer 's' of size 'n'.
 */
void *
memrchr(const void *s, int c, size_t n)
{
	const unsigned char *cp;
	if (n != 0) {
		cp = (unsigned char *)s + n;
		do {
			if (*(--cp) == (unsigned char)c)
				return((void *)cp);
		} while (--n != 0);
	}
	return(NULL);
}
DEF_WEAK(memrchr);
```

## stpcpy

```c

#include <string.h>
#if defined(APIWARN)
__warn_references(stpcpy,
    "warning: stpcpy() is dangerous; do not use it");
#endif
char *
stpcpy(char *to, const char *from)
{
	for (; (*to = *from) != '\0'; ++from, ++to);
	return(to);
}
```

## stpncpy

```c

#include <string.h>
char *
stpncpy(char *dst, const char *src, size_t n)
{
	if (n != 0) {
		char *d = dst;
		const char *s = src;
		dst = &dst[n];
		do {
			if ((*d++ = *s++) == 0) {
				dst = d - 1;
				/* NUL pad the remaining n-1 bytes */
				while (--n != 0)
					*d++ = 0;
				break;
			}
		} while (--n != 0);
	}
	return (dst);
}
DEF_WEAK(stpncpy);
```

## strcasecmp

```c

#include <string.h>
typedef unsigned char u_char;
/*
 * This array is designed for mapping upper and lower case letter
 * together for a case independent comparison.  The mappings are
 * based upon ascii character sequences.
 */
static const u_char charmap[] = {
	'\000', '\001', '\002', '\003', '\004', '\005', '\006', '\007',
	'\010', '\011', '\012', '\013', '\014', '\015', '\016', '\017',
	'\020', '\021', '\022', '\023', '\024', '\025', '\026', '\027',
	'\030', '\031', '\032', '\033', '\034', '\035', '\036', '\037',
	'\040', '\041', '\042', '\043', '\044', '\045', '\046', '\047',
	'\050', '\051', '\052', '\053', '\054', '\055', '\056', '\057',
	'\060', '\061', '\062', '\063', '\064', '\065', '\066', '\067',
	'\070', '\071', '\072', '\073', '\074', '\075', '\076', '\077',
	'\100', '\141', '\142', '\143', '\144', '\145', '\146', '\147',
	'\150', '\151', '\152', '\153', '\154', '\155', '\156', '\157',
	'\160', '\161', '\162', '\163', '\164', '\165', '\166', '\167',
	'\170', '\171', '\172', '\133', '\134', '\135', '\136', '\137',
	'\140', '\141', '\142', '\143', '\144', '\145', '\146', '\147',
	'\150', '\151', '\152', '\153', '\154', '\155', '\156', '\157',
	'\160', '\161', '\162', '\163', '\164', '\165', '\166', '\167',
	'\170', '\171', '\172', '\173', '\174', '\175', '\176', '\177',
	'\200', '\201', '\202', '\203', '\204', '\205', '\206', '\207',
	'\210', '\211', '\212', '\213', '\214', '\215', '\216', '\217',
	'\220', '\221', '\222', '\223', '\224', '\225', '\226', '\227',
	'\230', '\231', '\232', '\233', '\234', '\235', '\236', '\237',
	'\240', '\241', '\242', '\243', '\244', '\245', '\246', '\247',
	'\250', '\251', '\252', '\253', '\254', '\255', '\256', '\257',
	'\260', '\261', '\262', '\263', '\264', '\265', '\266', '\267',
	'\270', '\271', '\272', '\273', '\274', '\275', '\276', '\277',
	'\300', '\301', '\302', '\303', '\304', '\305', '\306', '\307',
	'\310', '\311', '\312', '\313', '\314', '\315', '\316', '\317',
	'\320', '\321', '\322', '\323', '\324', '\325', '\326', '\327',
	'\330', '\331', '\332', '\333', '\334', '\335', '\336', '\337',
	'\340', '\341', '\342', '\343', '\344', '\345', '\346', '\347',
	'\350', '\351', '\352', '\353', '\354', '\355', '\356', '\357',
	'\360', '\361', '\362', '\363', '\364', '\365', '\366', '\367',
	'\370', '\371', '\372', '\373', '\374', '\375', '\376', '\377',
};
int
strcasecmp(const char *s1, const char *s2)
{
	const u_char *cm = charmap;
	const u_char *us1 = (const u_char *)s1;
	const u_char *us2 = (const u_char *)s2;
	while (cm[*us1] == cm[*us2++])
		if (*us1++ == '\0')
			return (0);
	return (cm[*us1] - cm[*--us2]);
}
DEF_WEAK(strcasecmp);
int
strncasecmp(const char *s1, const char *s2, size_t n)
{
	if (n != 0) {
		const u_char *cm = charmap;
		const u_char *us1 = (const u_char *)s1;
		const u_char *us2 = (const u_char *)s2;
		do {
			if (cm[*us1] != cm[*us2++])
				return (cm[*us1] - cm[*--us2]);
			if (*us1++ == '\0')
				break;
		} while (--n != 0);
	}
	return (0);
}
DEF_WEAK(strncasecmp);
```

## strcat

```c

#include <string.h>
#if defined(APIWARN)
__warn_references(strcat,
    "warning: strcat() is almost always misused, please use strlcat()");
#endif
char *
strcat(char *s, const char *append)
{
	char *save = s;
	for (; *s; ++s);
	while ((*s++ = *append++) != '\0');
	return(save);
}
```

## strcmp

```c

#include <string.h>
/*
 * Compare strings.
 */
int
strcmp(const char *s1, const char *s2)
{
	while (*s1 == *s2++)
		if (*s1++ == 0)
			return (0);
	return (*(unsigned char *)s1 - *(unsigned char *)--s2);
}
DEF_STRONG(strcmp);
```

## strcpy

```c

#include <string.h>
#if defined(APIWARN)
__warn_references(strcpy,
    "warning: strcpy() is almost always misused, please use strlcpy()");
#endif
char *
strcpy(char *to, const char *from)
{
	char *save = to;
	for (; (*to = *from) != '\0'; ++from, ++to);
	return(save);
}
```

## strcspn

```c

#include <string.h>
/*
 * Span the complement of string s2.
 */
size_t
strcspn(const char *s1, const char *s2)
{
	const char *p, *spanp;
	char c, sc;
	/*
	 * Stop as soon as we find any character from s2.  Note that there
	 * must be a NUL in s2; it suffices to stop when we find that, too.
	 */
	for (p = s1;;) {
		c = *p++;
		spanp = s2;
		do {
			if ((sc = *spanp++) == c)
				return (p - 1 - s1);
		} while (sc != 0);
	}
	/* NOTREACHED */
}
DEF_STRONG(strcspn);
```

## strdup

```c

#include <sys/types.h>
#include <stddef.h>
#include <stdlib.h>
#include <string.h>
char *
strdup(const char *str)
{
	size_t siz;
	char *copy;
	siz = strlen(str) + 1;
	if ((copy = malloc(siz)) == NULL)
		return(NULL);
	(void)memcpy(copy, str, siz);
	return(copy);
}
DEF_WEAK(strdup);
```

## strlcat

```c

#include <sys/types.h>
#include <string.h>
/*
 * Appends src to string dst of size dsize (unlike strncat, dsize is the
 * full size of dst, not space left).  At most dsize-1 characters
 * will be copied.  Always NUL terminates (unless dsize <= strlen(dst)).
 * Returns strlen(src) + MIN(dsize, strlen(initial dst)).
 * If retval >= dsize, truncation occurred.
 */
size_t
strlcat(char *dst, const char *src, size_t dsize)
{
	const char *odst = dst;
	const char *osrc = src;
	size_t n = dsize;
	size_t dlen;
	/* Find the end of dst and adjust bytes left but don't go past end. */
	while (n-- != 0 && *dst != '\0')
		dst++;
	dlen = dst - odst;
	n = dsize - dlen;
	if (n-- == 0)
		return(dlen + strlen(src));
	while (*src != '\0') {
		if (n != 0) {
			*dst++ = *src;
			n--;
		}
		src++;
	}
	*dst = '\0';
	return(dlen + (src - osrc));	/* count does not include NUL */
}
DEF_WEAK(strlcat);
```

## strlcpy

```c

#include <sys/types.h>
#include <string.h>
/*
 * Copy string src to buffer dst of size dsize.  At most dsize-1
 * chars will be copied.  Always NUL terminates (unless dsize == 0).
 * Returns strlen(src); if retval >= dsize, truncation occurred.
 */
size_t
strlcpy(char *dst, const char *src, size_t dsize)
{
	const char *osrc = src;
	size_t nleft = dsize;
	/* Copy as many bytes as will fit. */
	if (nleft != 0) {
		while (--nleft != 0) {
			if ((*dst++ = *src++) == '\0')
				break;
		}
	}
	/* Not enough room in dst, add NUL and traverse rest of src. */
	if (nleft == 0) {
		if (dsize != 0)
			*dst = '\0';		/* NUL-terminate dst */
		while (*src++)
			;
	}
	return(src - osrc - 1);	/* count does not include NUL */
}
DEF_WEAK(strlcpy);
```

## strncat

```c

#include <string.h>
/*
 * Concatenate src on the end of dst.  At most strlen(dst)+n+1 bytes
 * are written at dst (at most n+1 bytes being appended).  Return dst.
 */
char *
strncat(char *dst, const char *src, size_t n)
{
	if (n != 0) {
		char *d = dst;
		const char *s = src;
		while (*d != 0)
			d++;
		do {
			if ((*d = *s++) == 0)
				break;
			d++;
		} while (--n != 0);
		*d = 0;
	}
	return (dst);
}
DEF_STRONG(strncat);
```

## strncmp

```c

#include <string.h>
int
strncmp(const char *s1, const char *s2, size_t n)
{
	if (n == 0)
		return (0);
	do {
		if (*s1 != *s2++)
			return (*(unsigned char *)s1 - *(unsigned char *)--s2);
		if (*s1++ == 0)
			break;
	} while (--n != 0);
	return (0);
}
DEF_STRONG(strncmp);
```

## strncpy

```c

#include <string.h>
/*
 * Copy src to dst, truncating or null-padding to always copy n bytes.
 * Return dst.
 */
char *
strncpy(char *dst, const char *src, size_t n)
{
	if (n != 0) {
		char *d = dst;
		const char *s = src;
		do {
			if ((*d++ = *s++) == 0) {
				/* NUL pad the remaining n-1 bytes */
				while (--n != 0)
					*d++ = 0;
				break;
			}
		} while (--n != 0);
	}
	return (dst);
}
DEF_STRONG(strncpy);
```

## strndup

```c

#include <sys/types.h>
#include <stddef.h>
#include <stdlib.h>
#include <string.h>
char *
strndup(const char *str, size_t maxlen)
{
	char *copy;
	size_t len;
	len = strnlen(str, maxlen);
	copy = malloc(len + 1);
	if (copy != NULL) {
		(void)memcpy(copy, str, len);
		copy[len] = '\0';
	}
	return copy;
}
DEF_WEAK(strndup);
```

## strpbrk

```c

#include <string.h>
/*
 * Find the first occurrence in s1 of a character in s2 (excluding NUL).
 */
char *
strpbrk(const char *s1, const char *s2)
{
	const char *scanp;
	int c, sc;
	while ((c = *s1++) != 0) {
		for (scanp = s2; (sc = *scanp++) != 0;)
			if (sc == c)
				return ((char *)(s1 - 1));
	}
	return (NULL);
}
DEF_STRONG(strpbrk);
```

## strsep

```c

#include <string.h>
/*
 * Get next token from string *stringp, where tokens are possibly-empty
 * strings separated by characters from delim.
 *
 * Writes NULs into the string at *stringp to end tokens.
 * delim need not remain constant from call to call.
 * On return, *stringp points past the last NUL written (if there might
 * be further tokens), or is NULL (if there are definitely no more tokens).
 *
 * If *stringp is NULL, strsep returns NULL.
 */
char *
strsep(char **stringp, const char *delim)
{
	char *s;
	const char *spanp;
	int c, sc;
	char *tok;
	if ((s = *stringp) == NULL)
		return (NULL);
	for (tok = s;;) {
		c = *s++;
		spanp = delim;
		do {
			if ((sc = *spanp++) == c) {
				if (c == 0)
					s = NULL;
				else
					s[-1] = 0;
				*stringp = s;
				return (tok);
			}
		} while (sc != 0);
	}
	/* NOTREACHED */
}
DEF_WEAK(strsep);
```

## strspn

```c

#include <string.h>
/*
 * Span the string s2 (skip characters that are in s2).
 */
size_t
strspn(const char *s1, const char *s2)
{
	const char *p = s1, *spanp;
	char c, sc;
	/*
	 * Skip any characters in s2, excluding the terminating \0.
	 */
cont:
	c = *p++;
	for (spanp = s2; (sc = *spanp++) != 0;)
		if (sc == c)
			goto cont;
	return (p - 1 - s1);
}
DEF_STRONG(strspn);
```

## strstr

```c

#include <string.h>
/*
 * Find the first occurrence of find in s.
 */
char *
strstr(const char *s, const char *find)
{
	char c, sc;
	size_t len;
	if ((c = *find++) != 0) {
		len = strlen(find);
		do {
			do {
				if ((sc = *s++) == 0)
					return (NULL);
			} while (sc != c);
		} while (strncmp(s, find, len) != 0);
		s--;
	}
	return ((char *)s);
}
```

## strtok

```c

#include <string.h>
char *
strtok(char *s, const char *delim)
{
	static char *last;
	return strtok_r(s, delim, &last);
}
DEF_STRONG(strtok);
char *
strtok_r(char *s, const char *delim, char **last)
{
	const char *spanp;
	int c, sc;
	char *tok;
	if (s == NULL && (s = *last) == NULL)
		return (NULL);
	/*
	 * Skip (span) leading delimiters (s += strspn(s, delim), sort of).
	 */
cont:
	c = *s++;
	for (spanp = delim; (sc = *spanp++) != 0;) {
		if (c == sc)
			goto cont;
	}
	if (c == 0) {		/* no non-delimiter characters */
		*last = NULL;
		return (NULL);
	}
	tok = s - 1;
	/*
	 * Scan token (scan for delimiters: s += strcspn(s, delim), sort of).
	 * Note that delim must have one NUL; we stop if we see that, too.
	 */
	for (;;) {
		c = *s++;
		spanp = delim;
		do {
			if ((sc = *spanp++) == c) {
				if (c == 0)
					s = NULL;
				else
					s[-1] = '\0';
				*last = s;
				return (tok);
			}
		} while (sc != 0);
	}
	/* NOTREACHED */
}
DEF_WEAK(strtok_r);
```

## wcpy


```c

#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)strcpy.c	8.1 (Berkeley) 6/4/93";
#endif /* LIBC_SCCS and not lint */
#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcpcpy(wchar_t * __restrict to, const wchar_t * __restrict from)
{
	for (; (*to = *from); ++from, ++to);
	return(to);
}
```

## wcppy


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcpncpy(wchar_t * __restrict dst, const wchar_t * __restrict src, size_t n)
{
	for (; n--; dst++, src++) {
		if (!(*dst = *src)) {
			wchar_t *ret = dst;
			while (n--)
				*++dst = L'\0';
			return (ret);
		}
	}
	return (dst);
}
```

## wcscasmp


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
#include <wctype.h>
int
wcscasecmp(const wchar_t *s1, const wchar_t *s2)
{
	wchar_t c1, c2;
	for (; *s1; s1++, s2++) {
		c1 = towlower(*s1);
		c2 = towlower(*s2);
		if (c1 != c2)
			return ((int)c1 - c2);
	}
	return (-*s2);
}
```

## wcat


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcscat.c,v 1.1 2000/12/23 23:14:36 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcscat(wchar_t * __restrict s1, const wchar_t * __restrict s2)
{
	wchar_t *cp;
	cp = s1;
	while (*cp != L'\0')
		cp++;
	while ((*cp++ = *s2++) != L'\0')
		;
	return (s1);
}
```

## wchr


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcschr(const wchar_t *s, wchar_t c)
{
	while (*s != c && *s != L'\0')
		s++;
	if (*s == c)
		return ((wchar_t *)s);
	return (NULL);
}
```

## wcmp


```c

#include <sys/cdefs.h>
#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)strcmp.c	8.1 (Berkeley) 6/4/93";
#if 0
__RCSID("$NetBSD: wcscmp.c,v 1.3 2001/01/05 12:13:12 itojun Exp $");
#endif
#endif /* LIBC_SCCS and not lint */
__FBSDID("$FreeBSD$");
#include <wchar.h>
/*
 * Compare strings.
 */
int
wcscmp(const wchar_t *s1, const wchar_t *s2)
{
	while (*s1 == *s2++)
		if (*s1++ == '\0')
			return (0);
	/* XXX assumes wchar_t = int */
	return (*(const unsigned int *)s1 - *(const unsigned int *)--s2);
}
```

## wcpy


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcscpy.c,v 1.1 2000/12/23 23:14:36 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcscpy(wchar_t * __restrict s1, const wchar_t * __restrict s2)
{
	wchar_t *cp;
	cp = s1;
	while ((*cp++ = *s2++) != L'\0')
		;
	return (s1);
}
```

## wcspn


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcscspn.c,v 1.1 2000/12/23 23:14:36 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
size_t
wcscspn(const wchar_t *s, const wchar_t *set)
{
	const wchar_t *p;
	const wchar_t *q;
	p = s;
	while (*p) {
		q = set;
		while (*q) {
			if (*p == *q)
				goto done;
			q++;
		}
		p++;
	}
done:
	return (p - s);
}
```

## wcsdup


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <stdlib.h>
#include <wchar.h>
wchar_t *
wcsdup(const wchar_t *s)
{
	wchar_t *copy;
	size_t len;
	len = wcslen(s) + 1;
	if ((copy = malloc(len * sizeof(wchar_t))) == NULL)
		return (NULL);
	return (wmemcpy(copy, s, len));
}
```

## wcsat


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcslcat.c,v 1.1 2000/12/23 23:14:36 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <sys/types.h>
#include <wchar.h>
/*
 * Appends src to string dst of size siz (unlike wcsncat, siz is the
 * full size of dst, not space left).  At most siz-1 characters
 * will be copied.  Always NUL terminates (unless siz == 0).
 * Returns wcslen(initial dst) + wcslen(src); if retval >= siz,
 * truncation occurred.
 */
size_t
wcslcat(wchar_t *dst, const wchar_t *src, size_t siz)
{
	wchar_t *d = dst;
	const wchar_t *s = src;
	size_t n = siz;
	size_t dlen;
	/* Find the end of dst and adjust bytes left but don't go past end */
	while (*d != '\0' && n-- != 0)
		d++;
	dlen = d - dst;
	n = siz - dlen;
	if (n == 0)
		return(dlen + wcslen(s));
	while (*s != '\0') {
		if (n != 1) {
			*d++ = *s;
			n--;
		}
		s++;
	}
	*d = '\0';
	return(dlen + (s - src));	/* count does not include NUL */
}
```

## wcslcpy

```c

#include <sys/types.h>
#include <wchar.h>
/*
 * Copy string src to buffer dst of size dsize.  At most dsize-1
 * chars will be copied.  Always NUL terminates (unless dsize == 0).
 * Returns wcslen(src); if retval >= dsize, truncation occurred.
 */
size_t
wcslcpy(wchar_t *dst, const wchar_t *src, size_t dsize)
{
	const wchar_t *osrc = src;
	size_t nleft = dsize;
	/* Copy as many bytes as will fit. */
	if (nleft != 0) {
		while (--nleft != 0) {
			if ((*dst++ = *src++) == L'\0')
				break;
		}
	}
	/* Not enough room in dst, add NUL and traverse rest of src. */
	if (nleft == 0) {
		if (dsize != 0)
			*dst = L'\0';		/* NUL-terminate dst */
		while (*src++)
			;
	}
	return(src - osrc - 1);	/* count does not include NUL */
}
DEF_WEAK(wcslcpy);
```

## wcslen


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcslen.c,v 1.1 2000/12/23 23:14:36 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
size_t
wcslen(const wchar_t *s)
{
	const wchar_t *p;
	p = s;
	while (*p)
		p++;
	return p - s;
}
```

## wcsncasmp


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
#include <wctype.h>
int
wcsncasecmp(const wchar_t *s1, const wchar_t *s2, size_t n)
{
	wchar_t c1, c2;
	if (n == 0)
		return (0);
	for (; *s1; s1++, s2++) {
		c1 = towlower(*s1);
		c2 = towlower(*s2);
		if (c1 != c2)
			return ((int)c1 - c2);
		if (--n == 0)
			return (0);
	}
	return (-*s2);
}
```

## wcsat


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcsncat.c,v 1.1 2000/12/23 23:14:36 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcsncat(wchar_t * __restrict s1, const wchar_t * __restrict s2, size_t n)
{
	wchar_t *p;
	wchar_t *q;
	const wchar_t *r;
	p = s1;
	while (*p)
		p++;
	q = p;
	r = s2;
	while (*r && n) {
		*q++ = *r++;
		n--;
	}
	*q = '\0';
	return s1;
}
```

## wcsmp


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)strncmp.c	8.1 (Berkeley) 6/4/93";
__RCSID("$NetBSD: wcsncmp.c,v 1.3 2001/01/05 12:13:13 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
int
wcsncmp(const wchar_t *s1, const wchar_t *s2, size_t n)
{
	if (n == 0)
		return (0);
	do {
		if (*s1 != *s2++) {
			/* XXX assumes wchar_t = int */
			return (*(const unsigned int *)s1 -
			    *(const unsigned int *)--s2);
		}
		if (*s1++ == 0)
			break;
	} while (--n != 0);
	return (0);
}
```

## wcspy


```c

#if 0
#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)strncpy.c	8.1 (Berkeley) 6/4/93";
#endif /* LIBC_SCCS and not lint */
#endif
#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
/*
 * Copy src to dst, truncating or null-padding to always copy n bytes.
 * Return dst.
 */
wchar_t *
wcsncpy(wchar_t * __restrict dst, const wchar_t * __restrict src, size_t n)
{
	if (n != 0) {
		wchar_t *d = dst;
		const wchar_t *s = src;
		do {
			if ((*d++ = *s++) == L'\0') {
				/* NUL pad the remaining n-1 bytes */
				while (--n != 0)
					*d++ = L'\0';
				break;
			}
		} while (--n != 0);
	}
	return (dst);
}
```

## wcsnlen


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
size_t
wcsnlen(const wchar_t *s, size_t maxlen)
{
	size_t len;
	for (len = 0; len < maxlen; len++, s++) {
		if (!*s)
			break;
	}
	return (len);
}
```

## wcspbrk


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcspbrk.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcspbrk(const wchar_t *s, const wchar_t *set)
{
	const wchar_t *p;
	const wchar_t *q;
	p = s;
	while (*p) {
		q = set;
		while (*q) {
			if (*p == *q) {
				/* LINTED interface specification */
				return (wchar_t *)p;
			}
			q++;
		}
		p++;
	}
	return NULL;
}
```

## wcshr


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcsrchr(const wchar_t *s, wchar_t c)
{
	const wchar_t *last;
	last = NULL;
	for (;;) {
		if (*s == c)
			last = s;
		if (*s == L'\0')
			break;
		s++;
	}
	return ((wchar_t *)last);
}
```

## wcsspn


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wcsspn.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
size_t
wcsspn(const wchar_t *s, const wchar_t *set)
{
	const wchar_t *p;
	const wchar_t *q;
	p = s;
	while (*p) {
		q = set;
		while (*q) {
			if (*p == *q)
				break;
			q++;
		}
		if (!*q)
			goto done;
		p++;
	}
done:
	return (p - s);
}
```

## wcsstr


```c

#if 0
#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)strstr.c	8.1 (Berkeley) 6/4/93";
#endif /* LIBC_SCCS and not lint */
#endif
#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
/*
 * Find the first occurrence of find in s.
 */
wchar_t *
wcsstr(const wchar_t * __restrict s, const wchar_t * __restrict find)
{
	wchar_t c, sc;
	size_t len;
	if ((c = *find++) != L'\0') {
		len = wcslen(find);
		do {
			do {
				if ((sc = *s++) == L'\0')
					return (NULL);
			} while (sc != c);
		} while (wcsncmp(s, find, len) != 0);
		s--;
	}
	return ((wchar_t *)s);
}
```

## wcstok


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t *
wcstok(wchar_t * __restrict s, const wchar_t * __restrict delim,
    wchar_t ** __restrict last)
{
	const wchar_t *spanp;
	wchar_t *tok;
	wchar_t c, sc;
	if (s == NULL && (s = *last) == NULL)
		return (NULL);
	/*
	 * Skip (span) leading delimiters (s += wcsspn(s, delim), sort of).
	 */
cont:
	c = *s++;
	for (spanp = delim; (sc = *spanp++) != L'\0';) {
		if (c == sc)
			goto cont;
	}
	if (c == L'\0') {	/* no non-delimiter characters */
		*last = NULL;
		return (NULL);
	}
	tok = s - 1;
	/*
	 * Scan token (scan for delimiters: s += wcscspn(s, delim), sort of).
	 * Note that delim must have one NUL; we stop if we see that, too.
	 */
	for (;;) {
		c = *s++;
		spanp = delim;
		do {
			if ((sc = *spanp++) == c) {
				if (c == L'\0')
					s = NULL;
				else
					s[-1] = L'\0';
				*last = s;
				return (tok);
			}
		} while (sc != L'\0');
	}
	/* NOTREACHED */
}
```

## wcswidth

```c

#include <wchar.h>
int
wcswidth(const wchar_t *s, size_t n)
{
	int w, q;
	w = 0;
	while (n && *s) {
		q = wcwidth(*s);
		if (q == -1)
			return (-1);
		w += q;
		s++;
		n--;
	}
	return w;
}
DEF_WEAK(wcswidth);
```
## wmehr


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wmemchr.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t	*
wmemchr(const wchar_t *s, wchar_t c, size_t n)
{
	size_t i;
	for (i = 0; i < n; i++) {
		if (*s == c) {
			/* LINTED const castaway */
			return (wchar_t *)s;
		}
		s++;
	}
	return NULL;
}
```

## wmemp


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wmemcmp.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
int
wmemcmp(const wchar_t *s1, const wchar_t *s2, size_t n)
{
	size_t i;
	for (i = 0; i < n; i++) {
		if (*s1 != *s2) {
			/* wchar might be unsigned */
			return *s1 > *s2 ? 1 : -1;
		}
		s1++;
		s2++;
	}
	return 0;
}
```

## wmepy


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wmemcpy.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <string.h>
#include <wchar.h>
wchar_t *
wmemcpy(wchar_t * __restrict d, const wchar_t * __restrict s, size_t n)
{
	return (wchar_t *)memcpy(d, s, n * sizeof(wchar_t));
}
```

## wmemmove


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wmemmove.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <string.h>
#include <wchar.h>
wchar_t *
wmemmove(wchar_t *d, const wchar_t *s, size_t n)
{
	return (wchar_t *)memmove(d, s, n * sizeof(wchar_t));
}
```

## wmemset


```c

#include <sys/cdefs.h>
#if 0
#if defined(LIBC_SCCS) && !defined(lint)
__RCSID("$NetBSD: wmemset.c,v 1.1 2000/12/23 23:14:37 itojun Exp $");
#endif /* LIBC_SCCS and not lint */
#endif
__FBSDID("$FreeBSD$");
#include <wchar.h>
wchar_t	*
wmemset(wchar_t *s, wchar_t c, size_t n)
{
	size_t i;
	wchar_t *p;
	p = (wchar_t *)s;
	for (i = 0; i < n; i++) {
		*p = c;
		p++;
	}
	return s;
}
```

