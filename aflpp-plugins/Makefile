.PHONY: all clean debug


all:libml-mutator.so

WITH_AFL_PERSISTENT_RECORD := $(shell afl-fuzz -h | grep -q '^Compiled with AFL_PERSISTENT_RECORD support.$$' && echo yes || echo no)

ifeq ($(WITH_AFL_PERSISTENT_RECORD),yes)
  override CFLAGS += -DAFL_PERSISTENT_RECORD
endif

libml-mutator.so:ml-mutator.o
	$(CC) $(CFLAGS) -shared  -o libml-mutator.so ml-mutator.o 

%.o: %.c $(DEPS)
	$(CC) $(CFLAGS) -c -o $@ $< -fPIC -Wall -D_FORTIFY_SOURCE=2 -I$(AFL_PATH)/include


clean:
	rm -rf *.so && \
	rm -rf *.o

debug: CFLAGS += -DDEBUG -g #-fsanitize=address
debug: libml-mutator.so

asan: CFLAGS += -fsanitize=address
asan: debug
