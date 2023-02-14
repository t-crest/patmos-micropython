# Include the core environment definitions; this will set $(TOP).
include ../../py/mkenv.mk

# Include py core make definitions.
include $(TOP)/py/py.mk
#include $(TOP)/extmod/extmod.mk

# Set CFLAGS and libraries.
CFLAGS += -I. -I$(BUILD) -I$(TOP)
CFLAGS += -Os -DNDEBUG
#CFLAGS += -Wall -Werror
CFLAGS += -fdata-sections -ffunction-sections
LIBS += -lm

CC = patmos-clang

# Define the required source files.
SRC_C = \
    main.c \
    mphalport.c \
    shared/runtime/stdout_helpers.c \
    shared/runtime/pyexec.c \
    shared/readline/readline.c \
    $(BUILD)/_frozen_mpy.c \
#    shared/runtime/gchelper_generic.c \
#    mphalport.c \
#    shared/libc/printf.c \

#include troll.mk

SRC_QSTR += shared/readline/readline.c shared/runtime/pyexec.c
#SRC_QSTR += shared/runtime/pyexec.c

# Define the required object files.
OBJ = $(PY_CORE_O) $(addprefix $(BUILD)/, $(SRC_C:.c=.o))

# Define the top-level target, the main firmware.
all: $(BUILD)/firmware.elf

# Frozen bytecode
$(BUILD)/_frozen_mpy.c: $(TOP)/tests/frozen/frozentest.mpy $(BUILD)/genhdr/qstrdefs.generated.h
	$(ECHO) "MISC freezing bytecode"
	$(Q)$(TOP)/tools/mpy-tool.py -f -q $(BUILD)/genhdr/qstrdefs.preprocessed.h -mlongint-impl=none $< > $@

# Run emulation build on a POSIX system with suitable terminal settings
run:
	@saved_=`stty -g`; \
	stty raw opost -echo; \
          $(BUILD)/firmware.elf; \
          echo "Exit status: $$?"; \
          stty $$saved_

patmosRun:
	@saved_=`stty -g`; \
        stty raw opost -echo; \
          pasim $(BUILD)/firmware.elf; \
          echo "Exit status: $$?"; \
          stty $$saved_

# Define how to build the firmware.
$(BUILD)/firmware.elf: $(OBJ)
	$(ECHO) "LINK $@"
	$(Q)$(CC) $(LDFLAGS) -o $@ $^ $(LIBS)
	$(Q)$(SIZE) $@

# Include remaining core make rules.
include $(TOP)/py/mkrules.mk