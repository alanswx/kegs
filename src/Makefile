# $Id: release_makefile_base,v 1.15 2003/11/21 20:00:42 kentd Exp kentd $

OBJECTS1 = adb.o clock.o config.o dis.o engine_c.o scc.o iwm.o \
	joystick_driver.o moremem.o paddles.o sim65816.o smartport.o \
	sound.o sound_driver.o video.o scc_socket_driver.o scc_windriver.o \
	scc_macdriver.o

include vars

.SUFFIXES: .dep .proto

AS = $(CC)

XLIBS = -L/usr/X11R6/lib
PERL = perl

all: $(TARGET)

specials: 8inst_s 16inst_s 8size 16size 8inst_c 16inst_c size_c size_s

specials_clean:
	rm -f 8inst_s 16inst_s 8size 16size 8inst_c 16inst_c size_c size_s

clean:
	rm $(OBJECTS)

# Mac builds:
kegsmac: $(OBJECTS) compile_time.o
	$(CC) $(CCOPTS) -arch i386 $(LDOPTS) $(OBJECTS) compile_time.o $(LDFLAGS) -o kegsmac $(EXTRA_LIBS) -prebind -framework Carbon -framework Quicktime
	mkdir -p ../KEGSMAC.app/Contents/Resources/English.lproj/main.nib
	mkdir -p ../KEGSMAC.app/Contents/MacOS
	mv kegsmac ../KEGSMAC.app/Contents/MacOS/KEGSMAC
	echo "APPL????" > ../KEGSMAC.app/Contents/PkgInfo
	cp -f Info.plist ../KEGSMAC.app/Contents/
	cp -f info.nib ../KEGSMAC.app/Contents/Resources/English.lproj/main.nib
	cp -f classes.nib ../KEGSMAC.app/Contents/Resources/English.lproj/main.nib
	cp -f objects.xib ../KEGSMAC.app/Contents/Resources/English.lproj/main.nib
	cp -f kegsicon.icns ../KEGSMAC.app/Contents/Resources/
	cp -f 525.icns ../KEGSMAC.app/Contents/Resources/
	cp -f 2mg.icns ../KEGSMAC.app/Contents/Resources/
	touch '../KEGSMAC.app/Icon?'

# Linux for X builds:
xkegs: $(OBJECTS) compile_time.o
	$(CC) $(CCOPTS) $(LDOPTS) $(OBJECTS) compile_time.o $(LDFLAGS) -o $(NAME)$(SUFFIX) $(XLIBS) $(EXTRA_LIBS) -lX11
	mv xkegs ..

jkegs: $(OBJECTS) compile_time.o
	$(LD) $(LDOPTS) $(OBJECTS) compile_time.o $(LDFLAGS) -o $(NAME)$(SUFFIX).html $(XLIBS) $(EXTRA_LIBS) -s ALLOW_MEMORY_GROWTH=1 -o $(NAME)$(SUFFIX).html --preload-file  kegs_conf --preload-file System.hdv --preload-file ROM.01 --preload-file bram.data.1 -s USE_SDL=2


# Cygwin for X builds:
kegs.exe: $(OBJECTS) compile_time.o
	$(CC) $(CCOPTS) $(LDOPTS) $(OBJECTS) compile_time.o $(LDFLAGS) -o $(NAME)$(SUFFIX) $(XLIBS) $(EXTRA_LIBS) -lXext -lX11 -lm
	mv kegs.exe ..

# Mingw32 (native windows) builds:
kegswin.exe: $(OBJECTS) compile_time.o
	$(CC) $(CCOPTS) $(LDOPTS) $(OBJECTS) compile_time.o $(LDFLAGS) -o $(NAME)$(SUFFIX) $(EXTRA_LIBS) -lwinmm -lgdi32 -ldsound -lcomctl32 -lws2_32
	mv $(NAME)$(SUFFIX) ..


8inst_c.h: instable.h
	$(PERL) make_inst c 8 instable.h > 8inst_c.h

16inst_c.h: instable.h
	$(PERL) make_inst c 16 instable.h > 16inst_c.h

size_c.h: size_tab.h
	$(PERL) make_size c size_tab.h > size_c.h

engine_c.o: 8inst_c.h 16inst_c.h size_c.h

8inst_s.h: instable.h
	$(PERL) make_inst s 8 instable.h > 8inst_s.h

16inst_s.h: instable.h
	$(PERL) make_inst s 16 instable.h > 16inst_s.h

size_s.h: size_tab.h
	$(PERL) make_size s size_tab.h > size_s.h

8size_s.h: size_tab.h
	$(PERL) make_size 8 size_tab.h > 8size_s.h

16size_s.h: size_tab.h
	$(PERL) make_size 16 size_tab.h > 16size_s.h

engine_s.o: 8inst_s.h 16inst_s.h 8size_s.h 16size_s.h size_s.h

.s.o:
	$(AS) -c $(OPTS) -I. $*.s

.c.o:
	$(CC) $(CCOPTS) $(XOPTS) -c $(OPTS) -I. $*.c

partls: partls.c
	cc $(CCOPTS) $(XOPTS) $(OPTS) -o partls partls.c

to_pro: prodos.h prodos_protos.h to_pro.c
	cc $(CCOPTS) $(XOPTS) $(OPTS) -o to_pro to_pro.c


compile_time.o: $(OBJECTS)


# dependency stuff
adb.o: adb.c adb.h defc.h defcomm.h iwm.h protos.h
engine_c.o: engine_c.c defc.h defcomm.h iwm.h protos.h protos_engine_c.h size_c.h op_routs.h defs_instr.h 8inst_c.h 16inst_c.h
clock.o: clock.c defc.h defcomm.h iwm.h protos.h
compile_time.o: compile_time.c
config.o: config.c defc.h defcomm.h iwm.h protos.h config.h
dis.o: dis.c defc.h defcomm.h iwm.h protos.h disas.h
scc.o: scc.c defc.h defcomm.h iwm.h protos.h scc.h
scc_socket_driver.o: scc_socket_driver.c defc.h defcomm.h iwm.h protos.h scc.h
scc_windriver.o: scc_windriver.c defc.h defcomm.h iwm.h protos.h scc.h
scc_macdriver.o: scc_macdriver.c defc.h defcomm.h iwm.h protos.h scc.h
iwm.o: iwm.c defc.h defcomm.h iwm.h protos.h iwm_35_525.h
joystick_driver.o: joystick_driver.c defc.h defcomm.h iwm.h protos.h
moremem.o: moremem.c defc.h defcomm.h iwm.h protos.h
paddles.o: paddles.c defc.h defcomm.h iwm.h protos.h
sim65816.o: sim65816.c defc.h defcomm.h iwm.h protos.h
smartport.o: smartport.c defc.h defcomm.h iwm.h protos.h
sound.o: sound.c defc.h defcomm.h iwm.h protos.h sound.h
sound_driver.o: sound_driver.c defc.h defcomm.h iwm.h protos.h sound.h
video.o: video.c defc.h defcomm.h iwm.h protos.h superhires.h kegsfont.h
macdriver.o: macdriver.c defc.h defcomm.h iwm.h protos.h protos_macdriver.h
macsnd_driver.o: macsnd_driver.c defc.h defcomm.h iwm.h protos.h sound.h
windriver.o: windriver.c defc.h defcomm.h iwm.h protos.h protos_windriver.h winresource.h
win32snd_driver.o: win32snd_driver.c defc.h defcomm.h iwm.h protos.h sound.h
