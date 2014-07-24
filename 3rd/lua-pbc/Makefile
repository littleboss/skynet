CC = gcc
CFLAGS = -O2 -Wall
AR = ar rc

PREFIX =            /usr/local
PBC_LDFLAGS = --shared
LUA_INCLUDE_DIR =   $(PREFIX)/include

BUILD = build

.PHONY : all lib clean

LIBSRCS = context.c varint.c array.c pattern.c register.c proto.c map.c alloc.c rmessage.c wmessage.c bootstrap.c stringpool.c decode.c
LIBNAME = libpbc.a
LUA_BINDING = protobuf.so

BUILD_O = $(BUILD)/o

all : lib luabinding 

lib : $(LIBNAME)
luabinding : $(LUA_BINDING)

clean :
	rm -rf $(BUILD)
	rm -rf $(LUA_BINDING)

$(BUILD) : $(BUILD_O)

$(BUILD_O) :
	mkdir -p $@

LIB_O :=

define BUILD_temp
  TAR :=  $(BUILD_O)/$(notdir $(basename $(1)))
  LIB_O := $(LIB_O) $$(TAR).o
  $$(TAR).o : | $(BUILD_O)
  -include $$(TAR).d
  $$(TAR).o : src/$(1)
	$(CC) $(CFLAGS) -c -Isrc -I. -o $$@ -MMD $$<
endef

$(foreach s,$(LIBSRCS),$(eval $(call BUILD_temp,$(s))))

$(LIBNAME) : $(LIB_O)
	cd $(BUILD) && $(AR) $(LIBNAME) $(addprefix ../,$^)

$(LUA_BINDING) :
	$(CC) $(CFLAGS) $(PBC_LDFLAGS) -o $(LUA_BINDING) -I$(LUA_INCLUDE_DIR) -Lbuild -lpbc pbc-lua.c

.PHONY : all lib clean

