OBJS_DIR = .objs

# define all of student executables
EXE_RELEASE = par_reduce
EXE_DEBUG = $(EXE_RELEASE)-debug

# list object file dependencies for each
OBJS = main.o par_reduce.o reduce.o reducers.o

# set up compiler
CC = clang
WARNINGS = -Wall -Wextra -Werror -Wno-error=unused-parameter -Wmissing-declarations -Wmissing-variable-declarations

CFLAGS_COMMON = $(WARNINGS) -std=c99 -c -MMD -MP -D_GNU_SOURCE -fPIC
CFLAGS_RELEASE = $(CFLAGS_COMMON) -O2
CFLAGS_DEBUG = $(CFLAGS_COMMON) -O0 -DDEBUG -g

# set up linker
LD = clang
LDFLAGS = -pthread -fPIC -lm

.PHONY: all
all: release

# build types
.PHONY: release
.PHONY: debug

release: $(EXE_RELEASE)
debug:   $(EXE_DEBUG)

# include dependencies
-include $(OBJS_DIR)/*.d

$(OBJS_DIR):
	@mkdir -p $(OBJS_DIR)

# patterns to create objects
# keep the debug and release postfix for object files so that we can always
# separate them correctly
$(OBJS_DIR)/%-release.o: %.c | $(OBJS_DIR)
	$(CC) $(CFLAGS_RELEASE) $< -o $@

$(OBJS_DIR)/%-debug.o: %.c | $(OBJS_DIR)
	$(CC) $(CFLAGS_DEBUG) $< -o $@

# exes
# you will need a pair of exe and exe-debug targets for each exe
$(EXE_RELEASE): $(OBJS:%.o=$(OBJS_DIR)/%-release.o)
	$(LD) $^ $(LDFLAGS) -o $@

$(EXE_DEBUG): $(OBJS:%.o=$(OBJS_DIR)/%-debug.o)
	$(LD) $^ $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -rf .objs $(EXE_RELEASE) $(EXE_DEBUG)
