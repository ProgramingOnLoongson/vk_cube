
SRC = cube.c model.c \
	vk_init.c vk_swapchain.c vk_common.c vk_render_pass.c vk_cmd.c \
	create_xcb_window.c \
	vk_texture.c cube_vert.c cube_frag.c vk_shader.c \
	vk_debug.c


SRC2 = main.c vulkanlib.c qvk.c vk_instance.c vk_validation.c vk_common.c

SRC_BIN_TO_C = bintoc.c

ifndef CC
  CC=gcc
endif

COMPILE_PLATFORM=$(shell uname | sed -e s/_.*//|tr '[:upper:]' '[:lower:]' | sed -e 's/\//_/g')
COMPILE_ARCH=$(shell uname -m | sed -e s/i.86/x86/ | sed -e 's/^arm.*/arm/')

OPTIMIZE = -O2 -ffast-math

#Possible values "gnu90", "gnu99" and "gnu11". 
#Note that the engine uses gnu-extensions. 
#gnu90 is broken in the commit where this message is added.

CFLAGS= -Wall -std=gnu99 $(OPTIMIZE)


# SOURCE_DIR = ~/game/vkQuake3/code/linux/
# BINARY_DIR = ~/game/vkQuake3/code/linux/

#  XCB_CFLAGS = $(shell PKG_CONFIG --silence-errors --cflags xcb)
XCB_LIBS = $(shell pkg-config --cflags --libs xcb)
RENDERER_LIBS = $(shell pkg-config --cflags --libs vulkan)

ALL_LIBS = $(XCB_LIBS) $(RENDERER_LIBS) -ldl -lm

# Default target executed when no arguments are given to make.
ALL: main cube bintoc

# the main load the libvulkan.so at run time
main: clean info
	$(CC) $(CFLAGS) $(SRC2) -o main $(XCB_LIBS) -ldl -lm

# link the vulkan at build time
cube: clean info
	$(CC) $(CFLAGS) $(SRC) -o cube  $(XCB_LIBS) $(RENDERER_LIBS) -ldl -lm

bintoc:
	$(CC) $(CFLAGS) $(SRC_BIN_TO_C) -o bintoc

info:
	@echo ""
	@echo "  COMPILE_PLATFORM: $(COMPILE_PLATFORM)"
	@echo "  COMPILE_ARCH: $(COMPILE_ARCH)"
	@echo "  CC: $(CC)"
	@echo ""
	@echo "  CFLAGS:" $(CFLAGS)
	@echo "  LDFLAGS:" $(LDFLAGS)
	@echo "  XCB_LIBS:" $(XCB_LIBS)
	@echo "  VULKAN_LIBS:" $(RENDERER_LIBS)
	
clean:
	rm -rf main cube bintoc
