LD = ld
CC = cc
PKG_CONFIG = pkg-config
INSTALL = install
CFLAGS = -g -O2 -Wall -Wextra
LDFLAGS =
LIBS = -lm
VLC_PLUGIN_CFLAGS := $(shell $(PKG_CONFIG) --cflags vlc-plugin)
VLC_PLUGIN_LIBS := $(shell $(PKG_CONFIG) --libs vlc-plugin)
VLC_AVCODEC_LIBS := $(shell $(PKG_CONFIG) --libs libavcodec)
VLC_AVUTILS_LIBS := $(shell $(PKG_CONFIG) --libs libavutil)
VLC_PLUGIN_DIR := $(shell $(PKG_CONFIG) --variable=pluginsdir vlc-plugin)

plugindir = $(VLC_PLUGIN_DIR)/codec

override CC += -std=gnu11
override CPPFLAGS += -DPIC -DHAVE_LIBAVCODEC_AVCODEC_H -DHAVE_LIBAVUTIL_AVUTIL_H -DHAVE_ATTRIBUTE_PACKED
override CPPFLAGS += -DDOMAIN=\"vlc-myplugin\"
override CPPFLAGS += -D"_(str)=dgettext(DOMAIN,str)"
override CPPFLAGS += -D"N_(str)=(str)"
override CPPFLAGS += -I. -I/usr/src/vlc-3.0.8/include -I/usr/src/vlc-3.0.8/modules/codec
override CFLAGS += -fPIC
override LDFLAGS += -Wl,-no-undefined,-z,defs

override CPPFLAGS += -DMODULE_STRING=\"ntff_avcodec\"
override CFLAGS += $(VLC_PLUGIN_CFLAGS)
override LIBS += $(VLC_PLUGIN_LIBS)  $(VLC_AVCODEC_LIBS) $(VLC_AVUTILS_LIBS)

all: libntff_avcodec_plugin.so

install: all
	mkdir -p -- $(DESTDIR)$(plugindir)
	$(INSTALL) --mode 0755 libntff_avcodec_plugin.so $(DESTDIR)$(plugindir)

install-strip:
	$(MAKE) install INSTALL="$(INSTALL) -s"

uninstall:
	rm -f $(plugindir)/libntff_avcodec_plugin.so

clean:
	rm -f -- libntff_avcodec_plugin.so *.o

mostlyclean: clean

SOURCES = audio.c chroma.c va_surface.c avcodec.c subtitle.c encoder.c fourcc.c va.c video.c
#dxva2.c dxva_blacklist.c d3d11va.c directx_va.c

$(SOURCES:%.c=%.o): $(SOURCES:%.c=%.c)

libntff_avcodec_plugin.so: $(SOURCES:%.c=%.o)
	$(CC) $(LDFLAGS) -shared -o $@ $^ $(LIBS)

.PHONY: all install install-strip uninstall clean mostlyclean
