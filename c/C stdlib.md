# C stdlib
 
- [abs.c](#absc)
- [getenv.c](#getenvc)
- [getopt_long.c](#getopt_longc)
- [getsubopt.c](#getsuboptc)
- [hcreate.c](#hcreatec)
- [hcreate_r.c](#hcreate_rc)
- [hdestroy_r.c](#hdestroy_rc)
- [hsearch.h](#hsearchh)
- [hsearch_r.c](#hsearch_rc)
- [imaxabs.c](#imaxabsc)
- [imaxdiv.c](#imaxdivc)
- [insque.c](#insquec)
- [labs.c](#labsc)
- [llabs.c](#llabsc)
- [lsearch.c](#lsearchc)
- [qsort.c](#qsortc)
- [quick_exit.c](#quick_exitc)
- [random](#random)
- [reallocarray.c](#reallocarrayc)
- [realpath.c](#realpathc)
- [remque.c](#remquec)
- [setenv.c](#setenvc)
- [tfind.c](#tfindc)
- [tsearch.c](#tsearchc)

## abs.c


```c

#include <stdlib.h>
int
abs(int j)
{
	return(j < 0 ? -j : j);
}
DEF_STRONG(abs);
```

## getenv.c


```c

#include <stdlib.h>
#include <string.h>
char *__findenv(const char *name, int len, int *offset);
/*
 * __findenv --
 *	Returns pointer to value associated with name, if any, else NULL.
 *	Starts searching within the environmental array at offset.
 *	Sets offset to be the offset of the name/value combination in the
 *	environmental array, for use by putenv(3), setenv(3) and unsetenv(3).
 *	Explicitly removes '=' in argument name.
 *
 *	This routine *should* be a static; don't use it.
 */
char *
__findenv(const char *name, int len, int *offset)
{
	extern char **environ;
	int i;
	const char *np;
	char **p, *cp;
	if (name == NULL || environ == NULL)
		return (NULL);
	for (p = environ + *offset; (cp = *p) != NULL; ++p) {
		for (np = name, i = len; i && *cp; i--)
			if (*cp++ != *np++)
				break;
		if (i == 0 && *cp++ == '=') {
			*offset = p - environ;
			return (cp);
		}
	}
	return (NULL);
}
/*
 * getenv --
 *	Returns ptr to value associated with name, if any, else NULL.
 */
char *
getenv(const char *name)
{
	int offset = 0;
	const char *np;
	for (np = name; *np && *np != '='; ++np)
		;
	return (__findenv(name, (int)(np - name), &offset));
}
```

## getopt_long.c


```c

#if 0
#if defined(LIBC_SCCS) && !defined(lint)
static char *rcsid = "$OpenBSD: getopt_long.c,v 1.16 2004/02/04 18:17:25 millert Exp $";
#endif /* LIBC_SCCS and not lint */
#endif
#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <err.h>
#include <errno.h>
#include <getopt.h>
#include <stdlib.h>
#include <string.h>
#define GNU_COMPATIBLE		/* Be more compatible, configure's use us! */
#if 0				/* we prefer to keep our getopt(3) */
#define	REPLACE_GETOPT		/* use this getopt as the system getopt(3) */
#endif
#ifdef REPLACE_GETOPT
int	opterr = 1;		/* if error message should be printed */
int	optind = 1;		/* index into parent argv vector */
int	optopt = '?';		/* character checked for validity */
int	optreset;		/* reset getopt */
char    *optarg;		/* argument associated with option */
#endif
#define PRINT_ERROR	((opterr) && (*options != ':'))
#define FLAG_PERMUTE	0x01	/* permute non-options to the end of argv */
#define FLAG_ALLARGS	0x02	/* treat non-options as args to option "-1" */
#define FLAG_LONGONLY	0x04	/* operate as getopt_long_only */
/* return values */
#define	BADCH		(int)'?'
#define	BADARG		((*options == ':') ? (int)':' : (int)'?')
#define	INORDER 	(int)1
#define	EMSG		""
#ifdef GNU_COMPATIBLE
#define NO_PREFIX	(-1)
#define D_PREFIX	0
#define DD_PREFIX	1
#define W_PREFIX	2
#endif
static int getopt_internal(int, char * const *, const char *,
			   const struct option *, int *, int);
static int parse_long_options(char * const *, const char *,
			      const struct option *, int *, int, int);
static int gcd(int, int);
static void permute_args(int, int, int, char * const *);
static char *place = EMSG; /* option letter processing */
/* XXX: set optreset to 1 rather than these two */
static int nonopt_start = -1; /* first non option argument (for permute) */
static int nonopt_end = -1;   /* first option after non options (for permute) */
/* Error messages */
static const char recargchar[] = "option requires an argument -- %c";
static const char illoptchar[] = "illegal option -- %c"; /* From P1003.2 */
#ifdef GNU_COMPATIBLE
static int dash_prefix = NO_PREFIX;
static const char gnuoptchar[] = "invalid option -- %c";
static const char recargstring[] = "option `%s%s' requires an argument";
static const char ambig[] = "option `%s%.*s' is ambiguous";
static const char noarg[] = "option `%s%.*s' doesn't allow an argument";
static const char illoptstring[] = "unrecognized option `%s%s'";
#else
static const char recargstring[] = "option requires an argument -- %s";
static const char ambig[] = "ambiguous option -- %.*s";
static const char noarg[] = "option doesn't take an argument -- %.*s";
static const char illoptstring[] = "unknown option -- %s";
#endif
/*
 * Compute the greatest common divisor of a and b.
 */
static int
gcd(int a, int b)
{
	int c;
	c = a % b;
	while (c != 0) {
		a = b;
		b = c;
		c = a % b;
	}
	return (b);
}
/*
 * Exchange the block from nonopt_start to nonopt_end with the block
 * from nonopt_end to opt_end (keeping the same order of arguments
 * in each block).
 */
static void
permute_args(int panonopt_start, int panonopt_end, int opt_end,
	char * const *nargv)
{
	int cstart, cyclelen, i, j, ncycle, nnonopts, nopts, pos;
	char *swap;
	/*
	 * compute lengths of blocks and number and size of cycles
	 */
	nnonopts = panonopt_end - panonopt_start;
	nopts = opt_end - panonopt_end;
	ncycle = gcd(nnonopts, nopts);
	cyclelen = (opt_end - panonopt_start) / ncycle;
	for (i = 0; i < ncycle; i++) {
		cstart = panonopt_end+i;
		pos = cstart;
		for (j = 0; j < cyclelen; j++) {
			if (pos >= panonopt_end)
				pos -= nnonopts;
			else
				pos += nopts;
			swap = nargv[pos];
			/* LINTED const cast */
			((char **) nargv)[pos] = nargv[cstart];
			/* LINTED const cast */
			((char **)nargv)[cstart] = swap;
		}
	}
}
/*
 * parse_long_options --
 *	Parse long options in argc/argv argument vector.
 * Returns -1 if short_too is set and the option does not match long_options.
 */
static int
parse_long_options(char * const *nargv, const char *options,
	const struct option *long_options, int *idx, int short_too, int flags)
{
	char *current_argv, *has_equal;
#ifdef GNU_COMPATIBLE
	char *current_dash;
#endif
	size_t current_argv_len;
	int i, match, exact_match, second_partial_match;
	current_argv = place;
#ifdef GNU_COMPATIBLE
	switch (dash_prefix) {
		case D_PREFIX:
			current_dash = "-";
			break;
		case DD_PREFIX:
			current_dash = "--";
			break;
		case W_PREFIX:
			current_dash = "-W ";
			break;
		default:
			current_dash = "";
			break;
	}
#endif
	match = -1;
	exact_match = 0;
	second_partial_match = 0;
	optind++;
	if ((has_equal = strchr(current_argv, '=')) != NULL) {
		/* argument found (--option=arg) */
		current_argv_len = has_equal - current_argv;
		has_equal++;
	} else
		current_argv_len = strlen(current_argv);
	for (i = 0; long_options[i].name; i++) {
		/* find matching long option */
		if (strncmp(current_argv, long_options[i].name,
		    current_argv_len))
			continue;
		if (strlen(long_options[i].name) == current_argv_len) {
			/* exact match */
			match = i;
			exact_match = 1;
			break;
		}
		/*
		 * If this is a known short option, don't allow
		 * a partial match of a single character.
		 */
		if (short_too && current_argv_len == 1)
			continue;
		if (match == -1)	/* first partial match */
			match = i;
		else if ((flags & FLAG_LONGONLY) ||
			 long_options[i].has_arg !=
			     long_options[match].has_arg ||
			 long_options[i].flag != long_options[match].flag ||
			 long_options[i].val != long_options[match].val)
			second_partial_match = 1;
	}
	if (!exact_match && second_partial_match) {
		/* ambiguous abbreviation */
		if (PRINT_ERROR)
			warnx(ambig,
#ifdef GNU_COMPATIBLE
			     current_dash,
#endif
			     (int)current_argv_len,
			     current_argv);
		optopt = 0;
		return (BADCH);
	}
	if (match != -1) {		/* option found */
		if (long_options[match].has_arg == no_argument
		    && has_equal) {
			if (PRINT_ERROR)
				warnx(noarg,
#ifdef GNU_COMPATIBLE
				     current_dash,
#endif
				     (int)current_argv_len,
				     current_argv);
			/*
			 * XXX: GNU sets optopt to val regardless of flag
			 */
			if (long_options[match].flag == NULL)
				optopt = long_options[match].val;
			else
				optopt = 0;
#ifdef GNU_COMPATIBLE
			return (BADCH);
#else
			return (BADARG);
#endif
		}
		if (long_options[match].has_arg == required_argument ||
		    long_options[match].has_arg == optional_argument) {
			if (has_equal)
				optarg = has_equal;
			else if (long_options[match].has_arg ==
			    required_argument) {
				/*
				 * optional argument doesn't use next nargv
				 */
				optarg = nargv[optind++];
			}
		}
		if ((long_options[match].has_arg == required_argument)
		    && (optarg == NULL)) {
			/*
			 * Missing argument; leading ':' indicates no error
			 * should be generated.
			 */
			if (PRINT_ERROR)
				warnx(recargstring,
#ifdef GNU_COMPATIBLE
				    current_dash,
#endif
				    current_argv);
			/*
			 * XXX: GNU sets optopt to val regardless of flag
			 */
			if (long_options[match].flag == NULL)
				optopt = long_options[match].val;
			else
				optopt = 0;
			--optind;
			return (BADARG);
		}
	} else {			/* unknown option */
		if (short_too) {
			--optind;
			return (-1);
		}
		if (PRINT_ERROR)
			warnx(illoptstring,
#ifdef GNU_COMPATIBLE
			      current_dash,
#endif
			      current_argv);
		optopt = 0;
		return (BADCH);
	}
	if (idx)
		*idx = match;
	if (long_options[match].flag) {
		*long_options[match].flag = long_options[match].val;
		return (0);
	} else
		return (long_options[match].val);
}
/*
 * getopt_internal --
 *	Parse argc/argv argument vector.  Called by user level routines.
 */
static int
getopt_internal(int nargc, char * const *nargv, const char *options,
	const struct option *long_options, int *idx, int flags)
{
	char *oli;				/* option letter list index */
	int optchar, short_too;
	static int posixly_correct = -1;
	if (options == NULL)
		return (-1);
	/*
	 * XXX Some GNU programs (like cvs) set optind to 0 instead of
	 * XXX using optreset.  Work around this braindamage.
	 */
	if (optind == 0)
		optind = optreset = 1;
	/*
	 * Disable GNU extensions if POSIXLY_CORRECT is set or options
	 * string begins with a '+'.
	 */
	if (posixly_correct == -1 || optreset)
		posixly_correct = (getenv("POSIXLY_CORRECT") != NULL);
	if (*options == '-')
		flags |= FLAG_ALLARGS;
	else if (posixly_correct || *options == '+')
		flags &= ~FLAG_PERMUTE;
	if (*options == '+' || *options == '-')
		options++;
	optarg = NULL;
	if (optreset)
		nonopt_start = nonopt_end = -1;
start:
	if (optreset || !*place) {		/* update scanning pointer */
		optreset = 0;
		if (optind >= nargc) {          /* end of argument vector */
			place = EMSG;
			if (nonopt_end != -1) {
				/* do permutation, if we have to */
				permute_args(nonopt_start, nonopt_end,
				    optind, nargv);
				optind -= nonopt_end - nonopt_start;
			}
			else if (nonopt_start != -1) {
				/*
				 * If we skipped non-options, set optind
				 * to the first of them.
				 */
				optind = nonopt_start;
			}
			nonopt_start = nonopt_end = -1;
			return (-1);
		}
		if (*(place = nargv[optind]) != '-' ||
#ifdef GNU_COMPATIBLE
		    place[1] == '\0') {
#else
		    (place[1] == '\0' && strchr(options, '-') == NULL)) {
#endif
			place = EMSG;		/* found non-option */
			if (flags & FLAG_ALLARGS) {
				/*
				 * GNU extension:
				 * return non-option as argument to option 1
				 */
				optarg = nargv[optind++];
				return (INORDER);
			}
			if (!(flags & FLAG_PERMUTE)) {
				/*
				 * If no permutation wanted, stop parsing
				 * at first non-option.
				 */
				return (-1);
			}
			/* do permutation */
			if (nonopt_start == -1)
				nonopt_start = optind;
			else if (nonopt_end != -1) {
				permute_args(nonopt_start, nonopt_end,
				    optind, nargv);
				nonopt_start = optind -
				    (nonopt_end - nonopt_start);
				nonopt_end = -1;
			}
			optind++;
			/* process next argument */
			goto start;
		}
		if (nonopt_start != -1 && nonopt_end == -1)
			nonopt_end = optind;
		/*
		 * If we have "-" do nothing, if "--" we are done.
		 */
		if (place[1] != '\0' && *++place == '-' && place[1] == '\0') {
			optind++;
			place = EMSG;
			/*
			 * We found an option (--), so if we skipped
			 * non-options, we have to permute.
			 */
			if (nonopt_end != -1) {
				permute_args(nonopt_start, nonopt_end,
				    optind, nargv);
				optind -= nonopt_end - nonopt_start;
			}
			nonopt_start = nonopt_end = -1;
			return (-1);
		}
	}
	/*
	 * Check long options if:
	 *  1) we were passed some
	 *  2) the arg is not just "-"
	 *  3) either the arg starts with -- we are getopt_long_only()
	 */
	if (long_options != NULL && place != nargv[optind] &&
	    (*place == '-' || (flags & FLAG_LONGONLY))) {
		short_too = 0;
#ifdef GNU_COMPATIBLE
		dash_prefix = D_PREFIX;
#endif
		if (*place == '-') {
			place++;		/* --foo long option */
#ifdef GNU_COMPATIBLE
			dash_prefix = DD_PREFIX;
#endif
		} else if (*place != ':' && strchr(options, *place) != NULL)
			short_too = 1;		/* could be short option too */
		optchar = parse_long_options(nargv, options, long_options,
		    idx, short_too, flags);
		if (optchar != -1) {
			place = EMSG;
			return (optchar);
		}
	}
	if ((optchar = (int)*place++) == (int)':' ||
	    (optchar == (int)'-' && *place != '\0') ||
	    (oli = strchr(options, optchar)) == NULL) {
		/*
		 * If the user specified "-" and  '-' isn't listed in
		 * options, return -1 (non-option) as per POSIX.
		 * Otherwise, it is an unknown option character (or ':').
		 */
		if (optchar == (int)'-' && *place == '\0')
			return (-1);
		if (!*place)
			++optind;
#ifdef GNU_COMPATIBLE
		if (PRINT_ERROR)
			warnx(posixly_correct ? illoptchar : gnuoptchar,
			      optchar);
#else
		if (PRINT_ERROR)
			warnx(illoptchar, optchar);
#endif
		optopt = optchar;
		return (BADCH);
	}
	if (long_options != NULL && optchar == 'W' && oli[1] == ';') {
		/* -W long-option */
		if (*place)			/* no space */
			/* NOTHING */;
		else if (++optind >= nargc) {	/* no arg */
			place = EMSG;
			if (PRINT_ERROR)
				warnx(recargchar, optchar);
			optopt = optchar;
			return (BADARG);
		} else				/* white space */
			place = nargv[optind];
#ifdef GNU_COMPATIBLE
		dash_prefix = W_PREFIX;
#endif
		optchar = parse_long_options(nargv, options, long_options,
		    idx, 0, flags);
		place = EMSG;
		return (optchar);
	}
	if (*++oli != ':') {			/* doesn't take argument */
		if (!*place)
			++optind;
	} else {				/* takes (optional) argument */
		optarg = NULL;
		if (*place)			/* no white space */
			optarg = place;
		else if (oli[1] != ':') {	/* arg not optional */
			if (++optind >= nargc) {	/* no arg */
				place = EMSG;
				if (PRINT_ERROR)
					warnx(recargchar, optchar);
				optopt = optchar;
				return (BADARG);
			} else
				optarg = nargv[optind];
		}
		place = EMSG;
		++optind;
	}
	/* dump back option letter */
	return (optchar);
}
#ifdef REPLACE_GETOPT
/*
 * getopt --
 *	Parse argc/argv argument vector.
 *
 * [eventually this will replace the BSD getopt]
 */
int
getopt(int nargc, char * const *nargv, const char *options)
{
	/*
	 * We don't pass FLAG_PERMUTE to getopt_internal() since
	 * the BSD getopt(3) (unlike GNU) has never done this.
	 *
	 * Furthermore, since many privileged programs call getopt()
	 * before dropping privileges it makes sense to keep things
	 * as simple (and bug-free) as possible.
	 */
	return (getopt_internal(nargc, nargv, options, NULL, NULL, 0));
}
#endif /* REPLACE_GETOPT */
/*
 * getopt_long --
 *	Parse argc/argv argument vector.
 */
int
getopt_long(int nargc, char * const *nargv, const char *options,
	const struct option *long_options, int *idx)
{
	return (getopt_internal(nargc, nargv, options, long_options, idx,
	    FLAG_PERMUTE));
}
/*
 * getopt_long_only --
 *	Parse argc/argv argument vector.
 */
int
getopt_long_only(int nargc, char * const *nargv, const char *options,
	const struct option *long_options, int *idx)
{
	return (getopt_internal(nargc, nargv, options, long_options, idx,
	    FLAG_PERMUTE|FLAG_LONGONLY));
}
```

## getsubopt.c


```c

#include <unistd.h>
#include <stdlib.h>
#include <string.h>
/*
 * The SVID interface to getsubopt provides no way of figuring out which
 * part of the suboptions list wasn't matched.  This makes error messages
 * tricky...  The extern variable suboptarg is a pointer to the token
 * which didn't match.
 */
char *suboptarg;
int
getsubopt(char **optionp, char * const *tokens, char **valuep)
{
	int cnt;
	char *p;
	suboptarg = *valuep = NULL;
	if (!optionp || !*optionp)
		return(-1);
	/* skip leading white-space, commas */
	for (p = *optionp; *p && (*p == ',' || *p == ' ' || *p == '\t'); ++p);
	if (!*p) {
		*optionp = p;
		return(-1);
	}
	/* save the start of the token, and skip the rest of the token. */
	for (suboptarg = p;
	    *++p && *p != ',' && *p != '=' && *p != ' ' && *p != '\t';);
	if (*p) {
		/*
		 * If there's an equals sign, set the value pointer, and
		 * skip over the value part of the token.  Terminate the
		 * token.
		 */
		if (*p == '=') {
			*p = '\0';
			for (*valuep = ++p;
			    *p && *p != ',' && *p != ' ' && *p != '\t'; ++p);
			if (*p)
				*p++ = '\0';
		} else
			*p++ = '\0';
		/* Skip any whitespace or commas after this token. */
		for (; *p && (*p == ',' || *p == ' ' || *p == '\t'); ++p);
	}
	/* set optionp for next round. */
	*optionp = p;
	for (cnt = 0; *tokens; ++tokens, ++cnt)
		if (!strcmp(suboptarg, *tokens))
			return(cnt);
	return(-1);
}
```

## hcreate.c


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD: head/lib/libc/stdlib/hcreate.c 292767 2015-12-27 07:50:11Z ed $");
#include <search.h>
#include <stdbool.h>
#include <stddef.h>
/*
 * Thread unsafe interface: use a single process-wide hash table and
 * forward calls to *_r() functions.
 */
static struct hsearch_data global_hashtable;
static bool global_hashtable_initialized = false;
int
hcreate(size_t nel)
{
	return (1);
}
void
hdestroy(void)
{
	/* Destroy global hash table if present. */
	if (global_hashtable_initialized) {
		hdestroy_r(&global_hashtable);
		global_hashtable_initialized = false;
	}
}
ENTRY *
hsearch(ENTRY item, ACTION action)
{
	ENTRY *retval;
	/* Create global hash table if needed. */
	if (!global_hashtable_initialized) {
		if (hcreate_r(0, &global_hashtable) == 0)
			return (NULL);
		global_hashtable_initialized = true;
	}
	if (hsearch_r(item, action, &retval, &global_hashtable) == 0)
		return (NULL);
	return (retval);
}
```

## hcreate_r.c


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD: head/lib/libc/stdlib/hcreate_r.c 292767 2015-12-27 07:50:11Z ed $");
#include <search.h>
#include <stdlib.h>
#include "hsearch.h"
int
hcreate_r(size_t nel, struct hsearch_data *htab)
{
	struct __hsearch *hsearch;
	/*
	 * Allocate a hash table object. Ignore the provided hint and start
	 * off with a table of sixteen entries. In most cases this hint is
	 * just a wild guess. Resizing the table dynamically if the use
	 * increases a threshold does not affect the worst-case running time.
	 */
	hsearch = malloc(sizeof(*hsearch));
	if (hsearch == NULL)
		return 0;
	hsearch->entries = calloc(16, sizeof(ENTRY));
	if (hsearch->entries == NULL) {
		free(hsearch);
		return 0;
	}
	/*
	 * Pick a random initialization for the FNV-1a hashing. This makes it
	 * hard to come up with a fixed set of keys to force hash collisions.
	 */
	arc4random_buf(&hsearch->offset_basis, sizeof(hsearch->offset_basis));
	hsearch->index_mask = 0xf;
	hsearch->entries_used = 0;
	htab->__hsearch = hsearch;
	return 1;
}
```

## hdestroy_r.c


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD: head/lib/libc/stdlib/hdestroy_r.c 292767 2015-12-27 07:50:11Z ed $");
#include <search.h>
#include <stdlib.h>
#include "hsearch.h"
void
hdestroy_r(struct hsearch_data *htab)
{
	struct __hsearch *hsearch;
	/* Free hash table object and its entries. */
	hsearch = htab->__hsearch;
	free(hsearch->entries);
	free(hsearch);
}
```

## hsearch.h


```c

#ifndef HSEARCH_H
#define HSEARCH_H
#include <search.h>
struct __hsearch {
	size_t offset_basis;	/* Initial value for FNV-1a hashing. */
	size_t index_mask;	/* Bitmask for indexing the table. */
	size_t entries_used;	/* Number of entries currently used. */
	ENTRY *entries;		/* Hash table entries. */
};
#endif
```

## hsearch_r.c


```c

#include <sys/cdefs.h>
__FBSDID("$FreeBSD: head/lib/libc/stdlib/hsearch_r.c 292767 2015-12-27 07:50:11Z ed $");
#include <errno.h>
#include <limits.h>
#include <search.h>
#include <stdint.h>
#include <stdlib.h>
#include <string.h>
#include "hsearch.h"
/*
 * Look up an unused entry in the hash table for a given hash. For this
 * implementation we use quadratic probing. Quadratic probing has the
 * advantage of preventing primary clustering.
 */
static ENTRY *
hsearch_lookup_free(struct __hsearch *hsearch, size_t hash)
{
	size_t index, i;
	for (index = hash, i = 0;; index += ++i) {
		ENTRY *entry = &hsearch->entries[index & hsearch->index_mask];
		if (entry->key == NULL)
			return (entry);
	}
}
/*
 * Computes an FNV-1a hash of the key. Depending on the pointer size, this
 * either uses the 32- or 64-bit FNV prime.
 */
static size_t
hsearch_hash(size_t offset_basis, const char *str)
{
	size_t hash;
	hash = offset_basis;
	while (*str != '\0') {
		hash ^= (uint8_t)*str++;
		if (sizeof(size_t) * CHAR_BIT <= 32)
			hash *= UINT32_C(16777619);
		else
			hash *= UINT64_C(1099511628211);
	}
	return (hash);
}
int
hsearch_r(ENTRY item, ACTION action, ENTRY **retval, struct hsearch_data *htab)
{
	struct __hsearch *hsearch;
	ENTRY *entry, *old_entries, *new_entries;
	size_t hash, index, i, old_hash, old_count, new_count;
	hsearch = htab->__hsearch;
	hash = hsearch_hash(hsearch->offset_basis, item.key);
	/*
	 * Search the hash table for an existing entry for this key.
	 * Stop searching if we run into an unused hash table entry.
	 */
	for (index = hash, i = 0;; index += ++i) {
		entry = &hsearch->entries[index & hsearch->index_mask];
		if (entry->key == NULL)
			break;
		if (strcmp(entry->key, item.key) == 0) {
			*retval = entry;
			return (1);
		}
	}
	/* Only perform the insertion if action is set to ENTER. */
	if (action == FIND) {
		errno = ESRCH;
		return (0);
	}
	if (hsearch->entries_used * 2 >= hsearch->index_mask) {
		/* Preserve the old hash table entries. */
		old_count = hsearch->index_mask + 1;
		old_entries = hsearch->entries;
		/*
		 * Allocate and install a new table if insertion would
		 * yield a hash table that is more than 50% used. By
		 * using 50% as a threshold, a lookup will only take up
		 * to two steps on average.
		 */
		new_count = (hsearch->index_mask + 1) * 2;
		new_entries = calloc(new_count, sizeof(ENTRY));
		if (new_entries == NULL)
			return (0);
		hsearch->entries = new_entries;
		hsearch->index_mask = new_count - 1;
		/* Copy over the entries from the old table to the new table. */
		for (i = 0; i < old_count; ++i) {
			entry = &old_entries[i];
			if (entry->key != NULL) {
				old_hash = hsearch_hash(hsearch->offset_basis,
				    entry->key);
				*hsearch_lookup_free(hsearch, old_hash) =
				    *entry;
			}
		}
		/* Destroy the old hash table entries. */
		free(old_entries);
		/*
		 * Perform a new lookup for a free table entry, so that
		 * we insert the entry into the new hash table.
		 */
		hsearch = htab->__hsearch;
		entry = hsearch_lookup_free(hsearch, hash);
	}
	/* Insert the new entry into the hash table. */
	*entry = item;
	++hsearch->entries_used;
	*retval = entry;
	return (1);
}
```

## imaxabs.c


```c

#include <inttypes.h>
intmax_t
imaxabs(intmax_t j)
{
	return (j < 0 ? -j : j);
}
```

## imaxdiv.c


```c

#include <inttypes.h>		/* imaxdiv_t */
imaxdiv_t
imaxdiv(intmax_t num, intmax_t denom)
{
	imaxdiv_t r;
	/* see div.c for comments */
	r.quot = num / denom;
	r.rem = num % denom;
	if (num >= 0 && r.rem < 0) {
		r.quot++;
		r.rem -= denom;
	}
	return (r);
}
```

## insque.c


```c

#include <stdlib.h>
#include <search.h>
struct qelem {
        struct qelem *q_forw;
        struct qelem *q_back;
};
void
insque(void *entry, void *pred)
{
	struct qelem *e = entry;
	struct qelem *p = pred;
	if (p == NULL)
		e->q_forw = e->q_back = NULL;
	else {
		e->q_forw = p->q_forw;
		e->q_back = p;
		if (p->q_forw != NULL)
			p->q_forw->q_back = e;
		p->q_forw = e;
	}
}
```

## labs.c


```c

#include <stdlib.h>
long
labs(long j)
{
	return(j < 0 ? -j : j);
}
```

## llabs.c


```c

#include <stdlib.h>
long long
llabs(long long j)
{
	return (j < 0 ? -j : j);
}
```

## lsearch.c


```c

#include <sys/types.h>
#include <string.h>
#include <search.h>
typedef int (*cmp_fn_t)(const void *, const void *);
static void *linear_base(const void *, const void *, size_t *, size_t,
    cmp_fn_t, int);
void *
lsearch(const void *key, void *base, size_t *nelp, size_t width,
    	cmp_fn_t compar)
{
	return(linear_base(key, base, nelp, width, compar, 1));
}
void *
lfind(const void *key, const void *base, size_t *nelp, size_t width,
	cmp_fn_t compar)
{
	return(linear_base(key, base, nelp, width, compar, 0));
}
static void *
linear_base(const void *key, const void *base, size_t *nelp, size_t width,
	cmp_fn_t compar, int add_flag)
{
	const char *element, *end;
	end = (const char *)base + *nelp * width;
	for (element = base; element < end; element += width)
		if (!compar(key, element))		/* key found */
			return((void *)element);
	if (!add_flag)					/* key not found */
		return(NULL);
	/*
	 * The UNIX System User's Manual, 1986 edition claims that
	 * a NULL pointer is returned by lsearch with errno set
	 * appropriately, if there is not enough room in the table
	 * to add a new item.  This can't be done as none of these
	 * routines have any method of determining the size of the
	 * table.  This comment isn't in the 1986-87 System V
	 * manual.
	 */
	++*nelp;
	memcpy((void *)end, key, width);
	return((void *)end);
}
```

## qsort.c


```c

#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)qsort.c	8.1 (Berkeley) 6/4/93";
#endif /* LIBC_SCCS and not lint */
#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include <stdlib.h>
#ifdef I_AM_QSORT_R
typedef int		 cmp_t(void *, const void *, const void *);
#else
typedef int		 cmp_t(const void *, const void *);
#endif
static inline char	*med3(char *, char *, char *, cmp_t *, void *);
static inline void	 swapfunc(char *, char *, size_t, int, int);
#define	MIN(a, b)	((a) < (b) ? a : b)
/*
 * Qsort routine from Bentley & McIlroy's "Engineering a Sort Function".
 */
#define	swapcode(TYPE, parmi, parmj, n) {		\
	size_t i = (n) / sizeof (TYPE);			\
	TYPE *pi = (TYPE *) (parmi);		\
	TYPE *pj = (TYPE *) (parmj);		\
	do { 						\
		TYPE	t = *pi;		\
		*pi++ = *pj;				\
		*pj++ = t;				\
	} while (--i > 0);				\
}
#define	SWAPINIT(TYPE, a, es) swaptype_ ## TYPE =	\
	((char *)a - (char *)0) % sizeof(TYPE) ||	\
	es % sizeof(TYPE) ? 2 : es == sizeof(TYPE) ? 0 : 1;
static inline void
swapfunc(char *a, char *b, size_t n, int swaptype_long, int swaptype_int)
{
	if (swaptype_long <= 1)
		swapcode(long, a, b, n)
	else if (swaptype_int <= 1)
		swapcode(int, a, b, n)
	else
		swapcode(char, a, b, n)
}
#define	swap(a, b)					\
	if (swaptype_long == 0) {			\
		long t = *(long *)(a);			\
		*(long *)(a) = *(long *)(b);		\
		*(long *)(b) = t;			\
	} else if (swaptype_int == 0) {			\
		int t = *(int *)(a);			\
		*(int *)(a) = *(int *)(b);		\
		*(int *)(b) = t;			\
	} else						\
		swapfunc(a, b, es, swaptype_long, swaptype_int)
#define	vecswap(a, b, n)				\
	if ((n) > 0) swapfunc(a, b, n, swaptype_long, swaptype_int)
#ifdef I_AM_QSORT_R
#define	CMP(t, x, y) (cmp((t), (x), (y)))
#else
#define	CMP(t, x, y) (cmp((x), (y)))
#endif
static inline char *
med3(char *a, char *b, char *c, cmp_t *cmp, void *thunk
#ifndef I_AM_QSORT_R
__unused
#endif
)
{
	return CMP(thunk, a, b) < 0 ?
	       (CMP(thunk, b, c) < 0 ? b : (CMP(thunk, a, c) < 0 ? c : a ))
	      :(CMP(thunk, b, c) > 0 ? b : (CMP(thunk, a, c) < 0 ? a : c ));
}
#ifdef I_AM_QSORT_R
void
qsort_r(void *a, size_t n, size_t es, void *thunk, cmp_t *cmp)
#else
#define	thunk NULL
void
qsort(void *a, size_t n, size_t es, cmp_t *cmp)
#endif
{
	char *pa, *pb, *pc, *pd, *pl, *pm, *pn;
	size_t d1, d2;
	int cmp_result;
	int swaptype_long, swaptype_int, swap_cnt;
loop:	SWAPINIT(long, a, es);
	SWAPINIT(int, a, es);
	swap_cnt = 0;
	if (n < 7) {
		for (pm = (char *)a + es; pm < (char *)a + n * es; pm += es)
			for (pl = pm;
			     pl > (char *)a && CMP(thunk, pl - es, pl) > 0;
			     pl -= es)
				swap(pl, pl - es);
		return;
	}
	pm = (char *)a + (n / 2) * es;
	if (n > 7) {
		pl = a;
		pn = (char *)a + (n - 1) * es;
		if (n > 40) {
			size_t d = (n / 8) * es;
			pl = med3(pl, pl + d, pl + 2 * d, cmp, thunk);
			pm = med3(pm - d, pm, pm + d, cmp, thunk);
			pn = med3(pn - 2 * d, pn - d, pn, cmp, thunk);
		}
		pm = med3(pl, pm, pn, cmp, thunk);
	}
	swap(a, pm);
	pa = pb = (char *)a + es;
	pc = pd = (char *)a + (n - 1) * es;
	for (;;) {
		while (pb <= pc && (cmp_result = CMP(thunk, pb, a)) <= 0) {
			if (cmp_result == 0) {
				swap_cnt = 1;
				swap(pa, pb);
				pa += es;
			}
			pb += es;
		}
		while (pb <= pc && (cmp_result = CMP(thunk, pc, a)) >= 0) {
			if (cmp_result == 0) {
				swap_cnt = 1;
				swap(pc, pd);
				pd -= es;
			}
			pc -= es;
		}
		if (pb > pc)
			break;
		swap(pb, pc);
		swap_cnt = 1;
		pb += es;
		pc -= es;
	}
	if (swap_cnt == 0) {  /* Switch to insertion sort */
		for (pm = (char *)a + es; pm < (char *)a + n * es; pm += es)
			for (pl = pm;
			     pl > (char *)a && CMP(thunk, pl - es, pl) > 0;
			     pl -= es)
				swap(pl, pl - es);
		return;
	}
	pn = (char *)a + n * es;
	d1 = MIN(pa - (char *)a, pb - pa);
	vecswap(a, pb - d1, d1);
	d1 = MIN(pd - pc, pn - pd - es);
	vecswap(pb, pn - d1, d1);
	d1 = pb - pa;
	d2 = pd - pc;
	if (d1 <= d2) {
		/* Recurse on left partition, then iterate on right partition */
		if (d1 > es) {
#ifdef I_AM_QSORT_R
			qsort_r(a, d1 / es, es, thunk, cmp);
#else
			qsort(a, d1 / es, es, cmp);
#endif
		}
		if (d2 > es) {
			/* Iterate rather than recurse to save stack space */
			/* qsort(pn - d2, d2 / es, es, cmp); */
			a = pn - d2;
			n = d2 / es;
			goto loop;
		}
	} else {
		/* Recurse on right partition, then iterate on left partition */
		if (d2 > es) {
#ifdef I_AM_QSORT_R
			qsort_r(pn - d2, d2 / es, es, thunk, cmp);
#else
			qsort(pn - d2, d2 / es, es, cmp);
#endif
		}
		if (d1 > es) {
			/* Iterate rather than recurse to save stack space */
			/* qsort(a, d1 / es, es, cmp); */
			n = d1 / es;
			goto loop;
		}
	}
}
```

## quick_exit.c


```c

#include <stdlib.h>
#include <pthread.h>
/**
 * Linked list of quick exit handlers.  This is simpler than the atexit()
 * version, because it is not required to support C++ destructors or
 * DSO-specific cleanups.
 */
struct quick_exit_handler {
	struct quick_exit_handler *next;
	void (*cleanup)(void);
};
/**
 * Lock protecting the handlers list.
 */
static pthread_mutex_t atexit_mutex = PTHREAD_MUTEX_INITIALIZER;
/**
 * Stack of cleanup handlers.  These will be invoked in reverse order when
 */
static struct quick_exit_handler *handlers;
int
at_quick_exit(void (*func)(void))
{
	struct quick_exit_handler *h;

	h = malloc(sizeof(*h));
	if (NULL == h)
		return (1);
	h->cleanup = func;
	pthread_mutex_lock(&atexit_mutex);
	h->next = handlers;
	handlers = h;
	pthread_mutex_unlock(&atexit_mutex);
	return (0);
}
void
quick_exit(int status)
{
	struct quick_exit_handler *h;
	/*
	 * XXX: The C++ spec requires us to call std::terminate if there is an
	 * exception here.
	 */
	for (h = handlers; NULL != h; h = h->next)
		h->cleanup();
	_Exit(status);
}
```

## random

```c
#if !defined(_KERNEL) && !defined(_STANDALONE)
#include <sys/cdefs.h>
#if defined(LIBC_SCCS) && !defined(lint)
#if 0
static char sccsid[] = "@(#)random.c	8.2 (Berkeley) 5/19/95";
#else
__RCSID("$NetBSD: random.c,v 1.4 2014/06/12 20:59:46 christos Exp $");
#endif
#endif /* LIBC_SCCS and not lint */

#include "namespace.h"

#include <assert.h>
#include <errno.h>
#include <stdlib.h>
#include "reentrant.h"

#ifdef __weak_alias
__weak_alias(initstate,_initstate)
__weak_alias(random,_random)
__weak_alias(setstate,_setstate)
__weak_alias(srandom,_srandom)
#endif


#ifdef _REENTRANT
static mutex_t random_mutex = MUTEX_INITIALIZER;
#endif
#else
#include <lib/libkern/libkern.h>
#define mutex_lock(a)	(void)0
#define mutex_unlock(a) (void)0
#endif

#ifndef SMALL_RANDOM
static void srandom_unlocked(unsigned int);
static long random_unlocked(void);

#define USE_BETTER_RANDOM

/*
 * random.c:
 *
 * An improved random number generation package.  In addition to the standard
 * rand()/srand() like interface, this package also has a special state info
 * interface.  The initstate() routine is called with a seed, an array of
 * bytes, and a count of how many bytes are being passed in; this array is
 * then initialized to contain information for random number generation with
 * that much state information.  Good sizes for the amount of state
 * information are 32, 64, 128, and 256 bytes.  The state can be switched by
 * calling the setstate() routine with the same array as was initiallized
 * with initstate().  By default, the package runs with 128 bytes of state
 * information and generates far better random numbers than a linear
 * congruential generator.  If the amount of state information is less than
 * 32 bytes, a simple linear congruential R.N.G. is used.
 *
 * Internally, the state information is treated as an array of ints; the
 * zeroeth element of the array is the type of R.N.G. being used (small
 * integer); the remainder of the array is the state information for the
 * R.N.G.  Thus, 32 bytes of state information will give 7 ints worth of
 * state information, which will allow a degree seven polynomial.  (Note:
 * the zeroeth word of state information also has some other information
 * stored in it -- see setstate() for details).
 *
 * The random number generation technique is a linear feedback shift register
 * approach, employing trinomials (since there are fewer terms to sum up that
 * way).  In this approach, the least significant bit of all the numbers in
 * the state table will act as a linear feedback shift register, and will
 * have period 2^deg - 1 (where deg is the degree of the polynomial being
 * used, assuming that the polynomial is irreducible and primitive).  The
 * higher order bits will have longer periods, since their values are also
 * influenced by pseudo-random carries out of the lower bits.  The total
 * period of the generator is approximately deg*(2**deg - 1); thus doubling
 * the amount of state information has a vast influence on the period of the
 * generator.  Note: the deg*(2**deg - 1) is an approximation only good for
 * large deg, when the period of the shift register is the dominant factor.
 * With deg equal to seven, the period is actually much longer than the
 * 7*(2**7 - 1) predicted by this formula.
 *
 * Modified 28 December 1994 by Jacob S. Rosenberg.
 * The following changes have been made:
 * All references to the type u_int have been changed to unsigned long.
 * All references to type int have been changed to type long.  Other
 * cleanups have been made as well.  A warning for both initstate and
 * setstate has been inserted to the effect that on Sparc platforms
 * the 'arg_state' variable must be forced to begin on word boundaries.
 * This can be easily done by casting a long integer array to char *.
 * The overall logic has been left STRICTLY alone.  This software was
 * tested on both a VAX and Sun SpacsStation with exactly the same
 * results.  The new version and the original give IDENTICAL results.
 * The new version is somewhat faster than the original.  As the
 * documentation says:  "By default, the package runs with 128 bytes of
 * state information and generates far better random numbers than a linear
 * congruential generator.  If the amount of state information is less than
 * 32 bytes, a simple linear congruential R.N.G. is used."  For a buffer of
 * 128 bytes, this new version runs about 19 percent faster and for a 16
 * byte buffer it is about 5 percent faster.
 *
 * Modified 07 January 2002 by Jason R. Thorpe.
 * The following changes have been made:
 * All the references to "long" have been changed back to "int".  This
 * fixes memory corruption problems on LP64 platforms.
 */

/*
 * For each of the currently supported random number generators, we have a
 * break value on the amount of state information (you need at least this
 * many bytes of state info to support this random number generator), a degree
 * for the polynomial (actually a trinomial) that the R.N.G. is based on, and
 * the separation between the two lower order coefficients of the trinomial.
 */
#define	TYPE_0		0		/* linear congruential */
#define	BREAK_0		8
#define	DEG_0		0
#define	SEP_0		0

#define	TYPE_1		1		/* x**7 + x**3 + 1 */
#define	BREAK_1		32
#define	DEG_1		7
#define	SEP_1		3

#define	TYPE_2		2		/* x**15 + x + 1 */
#define	BREAK_2		64
#define	DEG_2		15
#define	SEP_2		1

#define	TYPE_3		3		/* x**31 + x**3 + 1 */
#define	BREAK_3		128
#define	DEG_3		31
#define	SEP_3		3

#define	TYPE_4		4		/* x**63 + x + 1 */
#define	BREAK_4		256
#define	DEG_4		63
#define	SEP_4		1

/*
 * Array versions of the above information to make code run faster --
 * relies on fact that TYPE_i == i.
 */
#define	MAX_TYPES	5		/* max number of types above */

static const int degrees[MAX_TYPES] =	{ DEG_0, DEG_1, DEG_2, DEG_3, DEG_4 };
static const int seps[MAX_TYPES] =	{ SEP_0, SEP_1, SEP_2, SEP_3, SEP_4 };

/*
 * Initially, everything is set up as if from:
 *
 *	initstate(1, &randtbl, 128);
 *
 * Note that this initialization takes advantage of the fact that srandom()
 * advances the front and rear pointers 10*rand_deg times, and hence the
 * rear pointer which starts at 0 will also end up at zero; thus the zeroeth
 * element of the state information, which contains info about the current
 * position of the rear pointer is just
 *
 *	MAX_TYPES * (rptr - state) + TYPE_3 == TYPE_3.
 */

/* LINTED */
static int randtbl[DEG_3 + 1] = {
	TYPE_3,
#ifdef USE_BETTER_RANDOM
	0x991539b1, 0x16a5bce3, 0x6774a4cd,
	0x3e01511e, 0x4e508aaa, 0x61048c05,
	0xf5500617, 0x846b7115, 0x6a19892c,
	0x896a97af, 0xdb48f936, 0x14898454,
	0x37ffd106, 0xb58bff9c, 0x59e17104,
	0xcf918a49, 0x09378c83, 0x52c7a471,
	0x8d293ea9, 0x1f4fc301, 0xc3db71be,
	0x39b44e1c, 0xf8a44ef9, 0x4c8b80b1,
	0x19edc328, 0x87bf4bdd, 0xc9b240e5,
	0xe9ee4b1b, 0x4382aee7, 0x535b6b41,
	0xf3bec5da,
#else
	0x9a319039, 0x32d9c024, 0x9b663182,
	0x5da1f342, 0xde3b81e0, 0xdf0a6fb5,
	0xf103bc02, 0x48f340fb, 0x7449e56b,
	0xbeb1dbb0, 0xab5c5918, 0x946554fd,
	0x8c2e680f, 0xeb3d799f, 0xb11ee0b7,
	0x2d436b86, 0xda672e2a, 0x1588ca88,
	0xe369735d, 0x904f35f7, 0xd7158fd6,
	0x6fa6f051, 0x616e6b96, 0xac94efdc,
	0x36413f93, 0xc622c298, 0xf5a42ab8,
	0x8a88d77b, 0xf5ad9d0e, 0x8999220b,
	0x27fb47b9,
#endif /* USE_BETTER_RANDOM */
};

/*
 * fptr and rptr are two pointers into the state info, a front and a rear
 * pointer.  These two pointers are always rand_sep places aparts, as they
 * cycle cyclically through the state information.  (Yes, this does mean we
 * could get away with just one pointer, but the code for random() is more
 * efficient this way).  The pointers are left positioned as they would be
 * from the call
 *
 *	initstate(1, randtbl, 128);
 *
 * (The position of the rear pointer, rptr, is really 0 (as explained above
 * in the initialization of randtbl) because the state table pointer is set
 * to point to randtbl[1] (as explained below).
 */
static int *fptr = &randtbl[SEP_3 + 1];
static int *rptr = &randtbl[1];

/*
 * The following things are the pointer to the state information table, the
 * type of the current generator, the degree of the current polynomial being
 * used, and the separation between the two pointers.  Note that for efficiency
 * of random(), we remember the first location of the state information, not
 * the zeroeth.  Hence it is valid to access state[-1], which is used to
 * store the type of the R.N.G.  Also, we remember the last location, since
 * this is more efficient than indexing every time to find the address of
 * the last element to see if the front and rear pointers have wrapped.
 */
static int *state = &randtbl[1];
static int rand_type = TYPE_3;
static int rand_deg = DEG_3;
static int rand_sep = SEP_3;
static int *end_ptr = &randtbl[DEG_3 + 1];

/*
 * srandom:
 *
 * Initialize the random number generator based on the given seed.  If the
 * type is the trivial no-state-information type, just remember the seed.
 * Otherwise, initializes state[] based on the given "seed" via a linear
 * congruential generator.  Then, the pointers are set to known locations
 * that are exactly rand_sep places apart.  Lastly, it cycles the state
 * information a given number of times to get rid of any initial dependencies
 * introduced by the L.C.R.N.G.  Note that the initialization of randtbl[]
 * for default usage relies on values produced by this routine.
 */
static void
srandom_unlocked(unsigned int x)
{
	int i;

	if (rand_type == TYPE_0)
		state[0] = x;
	else {
		state[0] = x;
		for (i = 1; i < rand_deg; i++) {
#ifdef USE_BETTER_RANDOM
			int x1, hi, lo, t;

			/*
			 * Compute x[n + 1] = (7^5 * x[n]) mod (2^31 - 1).
			 * From "Random number generators: good ones are hard
			 * to find", Park and Miller, Communications of the ACM,
			 * vol. 31, no. 10,
			 * October 1988, p. 1195.
			 */
			x1 = state[i - 1];
			hi = x1 / 127773;
			lo = x1 % 127773;
			t = 16807 * lo - 2836 * hi;
			if (t <= 0)
				t += 0x7fffffff;
			state[i] = t;
#else
			state[i] = 1103515245 * state[i - 1] + 12345;
#endif /* USE_BETTER_RANDOM */
		}
		fptr = &state[rand_sep];
		rptr = &state[0];
		for (i = 0; i < 10 * rand_deg; i++)
			(void)random_unlocked();
	}
}

void
srandom(unsigned int x)
{

	mutex_lock(&random_mutex);
	srandom_unlocked(x);
	mutex_unlock(&random_mutex);
}

/*
 * initstate:
 *
 * Initialize the state information in the given array of n bytes for future
 * random number generation.  Based on the number of bytes we are given, and
 * the break values for the different R.N.G.'s, we choose the best (largest)
 * one we can and set things up for it.  srandom() is then called to
 * initialize the state information.
 *
 * Note that on return from srandom(), we set state[-1] to be the type
 * multiplexed with the current value of the rear pointer; this is so
 * successive calls to initstate() won't lose this information and will be
 * able to restart with setstate().
 *
 * Note: the first thing we do is save the current state, if any, just like
 * setstate() so that it doesn't matter when initstate is called.
 *
 * Returns a pointer to the old state.
 *
 * Note: The Sparc platform requires that arg_state begin on an int
 * word boundary; otherwise a bus error will occur. Even so, lint will
 * complain about mis-alignment, but you should disregard these messages.
 */
char *
initstate(
	unsigned int seed,		/* seed for R.N.G. */
	char *arg_state,		/* pointer to state array */
	size_t n)			/* # bytes of state info */
{
	void *ostate = (void *)(&state[-1]);
	int *int_arg_state;

	_DIAGASSERT(arg_state != NULL);

	int_arg_state = (int *)(void *)arg_state;

	mutex_lock(&random_mutex);
	if (rand_type == TYPE_0)
		state[-1] = rand_type;
	else
		state[-1] = MAX_TYPES * (int)(rptr - state) + rand_type;
	if (n < BREAK_0) {
		mutex_unlock(&random_mutex);
		return (NULL);
	} else if (n < BREAK_1) {
		rand_type = TYPE_0;
		rand_deg = DEG_0;
		rand_sep = SEP_0;
	} else if (n < BREAK_2) {
		rand_type = TYPE_1;
		rand_deg = DEG_1;
		rand_sep = SEP_1;
	} else if (n < BREAK_3) {
		rand_type = TYPE_2;
		rand_deg = DEG_2;
		rand_sep = SEP_2;
	} else if (n < BREAK_4) {
		rand_type = TYPE_3;
		rand_deg = DEG_3;
		rand_sep = SEP_3;
	} else {
		rand_type = TYPE_4;
		rand_deg = DEG_4;
		rand_sep = SEP_4;
	}
	state = (int *) (int_arg_state + 1); /* first location */
	end_ptr = &state[rand_deg];	/* must set end_ptr before srandom */
	srandom_unlocked(seed);
	if (rand_type == TYPE_0)
		int_arg_state[0] = rand_type;
	else
		int_arg_state[0] = MAX_TYPES * (int)(rptr - state) + rand_type;
	mutex_unlock(&random_mutex);
	return((char *)ostate);
}

/*
 * setstate:
 *
 * Restore the state from the given state array.
 *
 * Note: it is important that we also remember the locations of the pointers
 * in the current state information, and restore the locations of the pointers
 * from the old state information.  This is done by multiplexing the pointer
 * location into the zeroeth word of the state information.
 *
 * Note that due to the order in which things are done, it is OK to call
 * setstate() with the same state as the current state.
 *
 * Returns a pointer to the old state information.
 *
 * Note: The Sparc platform requires that arg_state begin on a long
 * word boundary; otherwise a bus error will occur. Even so, lint will
 * complain about mis-alignment, but you should disregard these messages.
 */
char *
setstate(char *arg_state)		/* pointer to state array */
{
	int *new_state;
	int type;
	int rear;
	void *ostate = (void *)(&state[-1]);

	_DIAGASSERT(arg_state != NULL);

	new_state = (int *)(void *)arg_state;
	type = (int)(new_state[0] % MAX_TYPES);
	rear = (int)(new_state[0] / MAX_TYPES);

	mutex_lock(&random_mutex);
	if (rand_type == TYPE_0)
		state[-1] = rand_type;
	else
		state[-1] = MAX_TYPES * (int)(rptr - state) + rand_type;
	switch(type) {
	case TYPE_0:
	case TYPE_1:
	case TYPE_2:
	case TYPE_3:
	case TYPE_4:
		rand_type = type;
		rand_deg = degrees[type];
		rand_sep = seps[type];
		break;
	default:
		mutex_unlock(&random_mutex);
		return (NULL);
	}
	state = (int *) (new_state + 1);
	if (rand_type != TYPE_0) {
		rptr = &state[rear];
		fptr = &state[(rear + rand_sep) % rand_deg];
	}
	end_ptr = &state[rand_deg];		/* set end_ptr too */
	mutex_unlock(&random_mutex);
	return((char *)ostate);
}

/*
 * random:
 *
 * If we are using the trivial TYPE_0 R.N.G., just do the old linear
 * congruential bit.  Otherwise, we do our fancy trinomial stuff, which is
 * the same in all the other cases due to all the global variables that have
 * been set up.  The basic operation is to add the number at the rear pointer
 * into the one at the front pointer.  Then both pointers are advanced to
 * the next location cyclically in the table.  The value returned is the sum
 * generated, reduced to 31 bits by throwing away the "least random" low bit.
 *
 * Note: the code takes advantage of the fact that both the front and
 * rear pointers can't wrap on the same call by not testing the rear
 * pointer if the front one has wrapped.
 *
 * Returns a 31-bit random number.
 */
static long
random_unlocked(void)
{
	int i;
	int *f, *r;

	if (rand_type == TYPE_0) {
		i = state[0];
		state[0] = i = (i * 1103515245 + 12345) & 0x7fffffff;
	} else {
		/*
		 * Use local variables rather than static variables for speed.
		 */
		f = fptr; r = rptr;
		*f += *r;
		/* chucking least random bit */
		i = ((unsigned int)*f >> 1) & 0x7fffffff;
		if (++f >= end_ptr) {
			f = state;
			++r;
		}
		else if (++r >= end_ptr) {
			r = state;
		}

		fptr = f; rptr = r;
	}
	return(i);
}

long
random(void)
{
	long r;

	mutex_lock(&random_mutex);
	r = random_unlocked();
	mutex_unlock(&random_mutex);
	return (r);
}
#else
long
random(void)
{
	static u_long randseed = 1;
	long x, hi, lo, t;

	/*
	 * Compute x[n + 1] = (7^5 * x[n]) mod (2^31 - 1).
	 * From "Random number generators: good ones are hard to find",
	 * Park and Miller, Communications of the ACM, vol. 31, no. 10,
	 * October 1988, p. 1195.
	 */
	x = randseed;
	hi = x / 127773;
	lo = x % 127773;
	t = 16807 * lo - 2836 * hi;
	if (t <= 0)
		t += 0x7fffffff;
	randseed = t;
	return (t);
}
#endif /* SMALL_RANDOM */

```

## reallocarray.c


```c

#include <sys/types.h>
#include <errno.h>
#include <stdint.h>
#include <stdlib.h>
/*
 * This is sqrt(SIZE_MAX+1), as s1*s2 <= SIZE_MAX
 * if both s1 < MUL_NO_OVERFLOW and s2 < MUL_NO_OVERFLOW
 */
#define MUL_NO_OVERFLOW	((size_t)1 << (sizeof(size_t) * 4))
void *
reallocarray(void *optr, size_t nmemb, size_t size)
{
	if ((nmemb >= MUL_NO_OVERFLOW || size >= MUL_NO_OVERFLOW) &&
	    nmemb > 0 && SIZE_MAX / nmemb < size) {
		errno = ENOMEM;
		return NULL;
	}
	return realloc(optr, size * nmemb);
}
DEF_WEAK(reallocarray);
```

## realpath.c


```c

#if defined(LIBC_SCCS) && !defined(lint)
static char sccsid[] = "@(#)realpath.c	8.1 (Berkeley) 2/16/94";
#endif /* LIBC_SCCS and not lint */
#include <sys/cdefs.h>
__FBSDID("$FreeBSD$");
#include "namespace.h"
#include <sys/param.h>
#include <sys/stat.h>
#include <errno.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include "un-namespace.h"
/*
 * Find the real name of path, by removing all ".", ".." and symlink
 * components.  Returns (resolved) on success, or (NULL) on failure,
 * in which case the path which caused trouble is left in (resolved).
 */
char *
realpath(const char * __restrict path, char * __restrict resolved)
{
	struct stat sb;
	char *p, *q, *s;
	size_t left_len, resolved_len;
	unsigned symlinks;
	int m, slen;
	char left[PATH_MAX], next_token[PATH_MAX], symlink[PATH_MAX];
	if (path == NULL) {
		errno = EINVAL;
		return (NULL);
	}
	if (path[0] == '\0') {
		errno = ENOENT;
		return (NULL);
	}
	if (resolved == NULL) {
		resolved = malloc(PATH_MAX);
		if (resolved == NULL)
			return (NULL);
		m = 1;
	} else
		m = 0;
	symlinks = 0;
	if (path[0] == '/') {
		resolved[0] = '/';
		resolved[1] = '\0';
		if (path[1] == '\0')
			return (resolved);
		resolved_len = 1;
		left_len = strlcpy(left, path + 1, sizeof(left));
	} else {
		if (getcwd(resolved, PATH_MAX) == NULL) {
			if (m)
				free(resolved);
			else {
				resolved[0] = '.';
				resolved[1] = '\0';
			}
			return (NULL);
		}
		resolved_len = strlen(resolved);
		left_len = strlcpy(left, path, sizeof(left));
	}
	if (left_len >= sizeof(left) || resolved_len >= PATH_MAX) {
		if (m)
			free(resolved);
		errno = ENAMETOOLONG;
		return (NULL);
	}
	/*
	 * Iterate over path components in `left'.
	 */
	while (left_len != 0) {
		/*
		 * Extract the next path component and adjust `left'
		 * and its length.
		 */
		p = strchr(left, '/');
		s = p ? p : left + left_len;
		if (s - left >= sizeof(next_token)) {
			if (m)
				free(resolved);
			errno = ENAMETOOLONG;
			return (NULL);
		}
		memcpy(next_token, left, s - left);
		next_token[s - left] = '\0';
		left_len -= s - left;
		if (p != NULL)
			memmove(left, s + 1, left_len + 1);
		if (resolved[resolved_len - 1] != '/') {
			if (resolved_len + 1 >= PATH_MAX) {
				if (m)
					free(resolved);
				errno = ENAMETOOLONG;
				return (NULL);
			}
			resolved[resolved_len++] = '/';
			resolved[resolved_len] = '\0';
		}
		if (next_token[0] == '\0') {
			/* Handle consequential slashes. */
			continue;
		}
		else if (strcmp(next_token, ".") == 0)
			continue;
		else if (strcmp(next_token, "..") == 0) {
			/*
			 * Strip the last path component except when we have
			 * single "/"
			 */
			if (resolved_len > 1) {
				resolved[resolved_len - 1] = '\0';
				q = strrchr(resolved, '/') + 1;
				*q = '\0';
				resolved_len = q - resolved;
			}
			continue;
		}
		/*
		 * Append the next path component and lstat() it.
		 */
		resolved_len = strlcat(resolved, next_token, PATH_MAX);
		if (resolved_len >= PATH_MAX) {
			if (m)
				free(resolved);
			errno = ENAMETOOLONG;
			return (NULL);
		}
		if (lstat(resolved, &sb) != 0) {
			if (m)
				free(resolved);
			return (NULL);
		}
		if (S_ISLNK(sb.st_mode)) {
			if (symlinks++ > MAXSYMLINKS) {
				if (m)
					free(resolved);
				errno = ELOOP;
				return (NULL);
			}
			slen = readlink(resolved, symlink, sizeof(symlink) - 1);
			if (slen < 0) {
				if (m)
					free(resolved);
				return (NULL);
			}
			symlink[slen] = '\0';
			if (symlink[0] == '/') {
				resolved[1] = 0;
				resolved_len = 1;
			} else if (resolved_len > 1) {
				/* Strip the last path component. */
				resolved[resolved_len - 1] = '\0';
				q = strrchr(resolved, '/') + 1;
				*q = '\0';
				resolved_len = q - resolved;
			}
			/*
			 * If there are any path components left, then
			 * append them to symlink. The result is placed
			 * in `left'.
			 */
			if (p != NULL) {
				if (symlink[slen - 1] != '/') {
					if (slen + 1 >= sizeof(symlink)) {
						if (m)
							free(resolved);
						errno = ENAMETOOLONG;
						return (NULL);
					}
					symlink[slen] = '/';
					symlink[slen + 1] = 0;
				}
				left_len = strlcat(symlink, left,
				    sizeof(symlink));
				if (left_len >= sizeof(left)) {
					if (m)
						free(resolved);
					errno = ENAMETOOLONG;
					return (NULL);
				}
			}
			left_len = strlcpy(left, symlink, sizeof(left));
		} else if (!S_ISDIR(sb.st_mode) && p != NULL) {
			if (m)
				free(resolved);
			errno = ENOTDIR;
			return (NULL);
		}
	}
	/*
	 * Remove trailing slash except when the resolved pathname
	 * is a single "/".
	 */
	if (resolved_len > 1 && resolved[resolved_len - 1] == '/')
		resolved[resolved_len - 1] = '\0';
	return (resolved);
}
```


## remque.c


```c

#include <stdlib.h>
#include <search.h>
struct qelem {
        struct qelem *q_forw;
        struct qelem *q_back;
};
void
remque(void *element)
{
	struct qelem *e = element;
	if (e->q_forw != NULL)
		e->q_forw->q_back = e->q_back;
	if (e->q_back != NULL)
		e->q_back->q_forw = e->q_forw;
}
```

## setenv.c


```c

#include <errno.h>
#include <stdlib.h>
#include <string.h>
extern char **environ;
static char **lastenv;				/* last value of environ */
/*
 * putenv --
 *	Add a name=value string directly to the environmental, replacing
 *	any current value.
 */
int
putenv(char *str)
{
	char **P, *cp;
	size_t cnt;
	int offset = 0;
	for (cp = str; *cp && *cp != '='; ++cp)
		;
	if (*cp != '=') {
		errno = EINVAL;
		return (-1);			/* missing `=' in string */
	}
	if (__findenv(str, (int)(cp - str), &offset) != NULL) {
		environ[offset++] = str;
		/* could be set multiple times */
		while (__findenv(str, (int)(cp - str), &offset)) {
			for (P = &environ[offset];; ++P)
				if (!(*P = *(P + 1)))
					break;
		}
		return (0);
	}
	/* create new slot for string */
	for (P = environ; *P != NULL; P++)
		;
	cnt = P - environ;
	P = reallocarray(lastenv, cnt + 2, sizeof(char *));
	if (!P)
		return (-1);
	if (lastenv != environ)
		memcpy(P, environ, cnt * sizeof(char *));
	lastenv = environ = P;
	environ[cnt] = str;
	environ[cnt + 1] = NULL;
	return (0);
}
DEF_WEAK(putenv);
/*
 * setenv --
 *	Set the value of the environmental variable "name" to be
 *	"value".  If rewrite is set, replace any current value.
 */
int
setenv(const char *name, const char *value, int rewrite)
{
	char *C, **P;
	const char *np;
	int l_value, offset = 0;
	if (!name || !*name) {
		errno = EINVAL;
		return (-1);
	}
	for (np = name; *np && *np != '='; ++np)
		;
	if (*np) {
		errno = EINVAL;
		return (-1);			/* has `=' in name */
	}
	l_value = strlen(value);
	if ((C = __findenv(name, (int)(np - name), &offset)) != NULL) {
		int tmpoff = offset + 1;
		if (!rewrite)
			return (0);
#if 0 /* XXX - existing entry may not be writable */
		if (strlen(C) >= l_value) {	/* old larger; copy over */
			while ((*C++ = *value++))
				;
			return (0);
		}
#endif
		/* could be set multiple times */
		while (__findenv(name, (int)(np - name), &tmpoff)) {
			for (P = &environ[tmpoff];; ++P)
				if (!(*P = *(P + 1)))
					break;
		}
	} else {					/* create new slot */
		size_t cnt;
		for (P = environ; *P != NULL; P++)
			;
		cnt = P - environ;
		P = reallocarray(lastenv, cnt + 2, sizeof(char *));
		if (!P)
			return (-1);
		if (lastenv != environ)
			memcpy(P, environ, cnt * sizeof(char *));
		lastenv = environ = P;
		offset = cnt;
		environ[cnt + 1] = NULL;
	}
	if (!(environ[offset] =			/* name + `=' + value */
	    malloc((size_t)((int)(np - name) + l_value + 2))))
		return (-1);
	for (C = environ[offset]; (*C = *name++) && *C != '='; ++C)
		;
	for (*C++ = '='; (*C++ = *value++); )
		;
	return (0);
}
DEF_WEAK(setenv);
/*
 * unsetenv(name) --
 *	Delete environmental variable "name".
 */
int
unsetenv(const char *name)
{
	char **P;
	const char *np;
	int offset = 0;
	if (!name || !*name) {
		errno = EINVAL;
		return (-1);
	}
	for (np = name; *np && *np != '='; ++np)
		;
	if (*np) {
		errno = EINVAL;
		return (-1);			/* has `=' in name */
	}
	/* could be set multiple times */
	while (__findenv(name, (int)(np - name), &offset)) {
		for (P = &environ[offset];; ++P)
			if (!(*P = *(P + 1)))
				break;
	}
	return (0);
}
DEF_WEAK(unsetenv);
```

## tfind.c


```c

#include <search.h>
typedef struct node_t
{
    char	  *key;
    struct node_t *llink, *rlink;
} node;
/* find a node, or return 0 */
void *
tfind(const void *vkey, void * const *vrootp,
    int (*compar)(const void *, const void *))
{
    char *key = (char *)vkey;
    node **rootp = (node **)vrootp;
    if (rootp == (struct node_t **)0)
	return ((struct node_t *)0);
    while (*rootp != (struct node_t *)0) {	/* T1: */
	int r;
	if ((r = (*compar)(key, (*rootp)->key)) == 0)	/* T2: */
	    return (*rootp);		/* key found */
	rootp = (r < 0) ?
	    &(*rootp)->llink :		/* T3: follow left branch */
	    &(*rootp)->rlink;		/* T4: follow right branch */
    }
    return (node *)0;
}
```

## tsearch.c


```c

#include <search.h>
#include <stdlib.h>
typedef struct node_t {
    char	  *key;
    struct node_t *left, *right;
} node;
/* find or insert datum into search tree */
void *
tsearch(const void *vkey, void **vrootp,
    int (*compar)(const void *, const void *))
{
    node *q;
    char *key = (char *)vkey;
    node **rootp = (node **)vrootp;
    if (rootp == (struct node_t **)0)
	return ((void *)0);
    while (*rootp != (struct node_t *)0) {	/* Knuth's T1: */
	int r;
	if ((r = (*compar)(key, (*rootp)->key)) == 0)	/* T2: */
	    return ((void *)*rootp);		/* we found it! */
	rootp = (r < 0) ?
	    &(*rootp)->left :		/* T3: follow left branch */
	    &(*rootp)->right;		/* T4: follow right branch */
    }
    q = malloc(sizeof(node));	/* T5: key not found */
    if (q != (struct node_t *)0) {	/* make new node */
	*rootp = q;			/* link new node to old */
	q->key = key;			/* initialize new node */
	q->left = q->right = (struct node_t *)0;
    }
    return ((void *)q);
}
/* delete node with given key */
void *
tdelete(const void *vkey, void **vrootp,
    int (*compar)(const void *, const void *))
{
    node **rootp = (node **)vrootp;
    char *key = (char *)vkey;
    node *p = (node *)1;
    node *q;
    node *r;
    int cmp;
    if (rootp == (struct node_t **)0 || *rootp == (struct node_t *)0)
	return ((struct node_t *)0);
    while ((cmp = (*compar)(key, (*rootp)->key)) != 0) {
	p = *rootp;
	rootp = (cmp < 0) ?
	    &(*rootp)->left :		/* follow left branch */
	    &(*rootp)->right;		/* follow right branch */
	if (*rootp == (struct node_t *)0)
	    return ((void *)0);		/* key not found */
    }
    r = (*rootp)->right;			/* D1: */
    if ((q = (*rootp)->left) == (struct node_t *)0)	/* Left (struct node_t *)0? */
	q = r;
    else if (r != (struct node_t *)0) {		/* Right link is null? */
	if (r->left == (struct node_t *)0) {	/* D2: Find successor */
	    r->left = q;
	    q = r;
	} else {			/* D3: Find (struct node_t *)0 link */
	    for (q = r->left; q->left != (struct node_t *)0; q = r->left)
		r = q;
	    r->left = q->right;
	    q->left = (*rootp)->left;
	    q->right = (*rootp)->right;
	}
    }
    free((struct node_t *) *rootp);	/* D4: Free node */
    *rootp = q;				/* link parent to new node */
    return(p);
}
/* Walk the nodes of a tree */
static void
trecurse(node *root, void (*action)(const void *, VISIT, int), int level)
{
    if (root->left == (struct node_t *)0 && root->right == (struct node_t *)0)
	(*action)(root, leaf, level);
    else {
	(*action)(root, preorder, level);
	if (root->left != (struct node_t *)0)
	    trecurse(root->left, action, level + 1);
	(*action)(root, postorder, level);
	if (root->right != (struct node_t *)0)
	    trecurse(root->right, action, level + 1);
	(*action)(root, endorder, level);
    }
}
/* Walk the nodes of a tree */
void
twalk(const void *vroot, void (*action)(const void *, VISIT, int))
{
    node *root = (node *)vroot;
    if (root != (node *)0 && action != (void (*)(const void *, VISIT, int))0)
	trecurse(root, action, 0);
}
```
