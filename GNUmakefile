wsrc := ./web2c/source/texk/web2c
wbin := ./web2c/build/texk/web2c

tangle := $(wbin)/tangle
web2c := $(wbin)/web2c/web2c
fixwrites := $(wbin)/web2c/fixwrites
splitup := $(wbin)/web2c/splitup
makecpool := $(wbin)/web2c/makecpool

cvtmf1 := sed -f $(wsrc)/web2c/cvtmf1.sed
cvtmf2 := sed -f $(wsrc)/web2c/cvtmf2.sed

texmfmp := $(wsrc)/texmfmp.h

defines := \
	$(wsrc)/web2c/common.defines \
	$(wsrc)/web2c/texmf.defines \
	$(wsrc)/web2c/mfmp.defines

cflags := -DHAVE_CONFIG_H -DMFNOWIN -I$(wbin) -I$(wsrc) -I$(wbin)/w2c -g 

%.p %.pool : %.web
	$(tangle) $<

%d.h %ini.c %coerce.h %0.c: %.p
	cat $(defines) $< | $(cvtmf1) | $(web2c) -h$(texmfmp) -m -c$*coerce | $(cvtmf2) | $(fixwrites) $* | $(splitup) -i -l 65000 $*

%-pool.c: %.pool
	$(makecpool) $* > $@

mfextra.o: $(wsrc)/mfextra.c
	gcc $(cflags) -o $@ -c $<

%.o: %.c
	gcc $(cflags) -o $@ -c $<

mf-nowin: mfini.o mf0.o mf-pool.o mfextra.o $(wbin)/lib/lib.a -lkpathsea $(wbin)/window/libwindow.a -lm
	gcc -o $@ $^

clean:
	rm -f mf.pool mfd.h mfcoerce.h mf0.c mfini.c mf-pool.c mf-nowin *.o 

.PHONY: clean
