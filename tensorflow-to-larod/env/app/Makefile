PROG1	= tensorflow_to_larod
OBJS1	= $(PROG1).c argparse.c imgconverter.c imgprovider.c
PROGS	= $(PROG1)

PKGS = gio-2.0 vdostream gio-unix-2.0 liblarod

CFLAGS  += -Iinclude

LDLIBS  += -lyuv
LDFLAGS += -L./lib -Wl,-rpath,'$$ORIGIN/lib'

CFLAGS += $(shell PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) pkg-config --cflags $(PKGS))
LDLIBS += $(shell PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) pkg-config --libs $(PKGS))

CFLAGS += -Wall

all:	$(PROGS) \
		model/converted_model.larod

$(PROG1): $(OBJS1)
	$(CC) $(CFLAGS) $(LDFLAGS) $^ $(LDLIBS) -o $@

model/converted_model.larod:
	mkdir -p model
	cp converted_model_edgetpu.tflite model/converted_model.tflite
	larod-convert.py tflite model/converted_model.tflite
	rm -f model/*.tflite

clean:
	rm -f $(PROGS) *.o *.eap lib/* include/* model/* label/*
