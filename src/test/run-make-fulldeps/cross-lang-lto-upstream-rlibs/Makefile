
-include ../tools.mk

# This test makes sure that we don't loose upstream object files when compiling
# staticlibs with -Zcross-lang-lto

all: staticlib.rs upstream.rs
	$(RUSTC) upstream.rs -Z cross-lang-lto -Ccodegen-units=1

	# Check No LTO
	$(RUSTC) staticlib.rs -Z cross-lang-lto -Ccodegen-units=1 -L. -o $(TMPDIR)/staticlib.a
	(cd $(TMPDIR); llvm-ar x ./staticlib.a)
	# Make sure the upstream object file was included
	ls $(TMPDIR)/upstream.*.rcgu.o

	# Cleanup
	rm $(TMPDIR)/*

	# Check ThinLTO
	$(RUSTC) upstream.rs -Z cross-lang-lto -Ccodegen-units=1 -Clto=thin
	$(RUSTC) staticlib.rs -Z cross-lang-lto -Ccodegen-units=1 -Clto=thin -L. -o $(TMPDIR)/staticlib.a
	(cd $(TMPDIR); llvm-ar x ./staticlib.a)
	ls $(TMPDIR)/upstream.*.rcgu.o
