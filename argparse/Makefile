# Defaults
BASIC_CFLAGS = -Wall -O3 -g -ggdb -fPIC
BASIC_LDFLAGS = -lm

# We use ALL_* variants
ALL_CFLAGS = $(CFLAGS) $(BASIC_CFLAGS)
ALL_LDFLAGS = $(LDFLAGS) $(BASIC_LDFLAGS)

LIBNAME=libargparse

DYLIBSUFFIX=so
STLIBSUFFIX=a
DYLIBNAME=$(LIBNAME).$(DYLIBSUFFIX)
DYLIB_MAKE_CMD=$(CC) -shared -o $(DYLIBNAME) $(ALL_LDFLAGS)
STLIBNAME=$(LIBNAME).$(STLIBSUFFIX)
STLIB_MAKE_CMD=ar rcs $(STLIBNAME)

# Platform-specific overrides
uname_S := $(shell sh -c 'uname -s 2>/dev/null || echo not')
ifeq ($(uname_S),Darwin)
  DYLIBSUFFIX=dylib
  DYLIB_MAKE_CMD=$(CC) -shared -o $(DYLIBNAME) $(ALL_LDFLAGS)
endif

all: $(DYLIBNAME) $(STLIBNAME)

OBJS += argparse.o
OBJS += test_argparse.o

$(OBJS): %.o: %.c
	$(CC) -o $*.o -c $(ALL_CFLAGS) $<

$(DYLIBNAME): argparse.o
	$(DYLIB_MAKE_CMD) $^

$(STLIBNAME): argparse.o
	$(STLIB_MAKE_CMD) $^

test: test_argparse
	@echo "###### Unit Test #####"
	@./test.sh

test_argparse: $(OBJS)
	$(CC) $(ALL_CFLAGS) -o $@ $^ $(ALL_LDFLAGS)

clean:
	rm -rf test_argparse
	rm -rf *.[ao]
	rm -rf *.dylib
