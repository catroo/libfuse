## Process this file with automake to produce Makefile.in

ACLOCAL_AMFLAGS = -I m4

SUBDIRS = @subdirs2@ doc

EXTRA_DIST = \
	fuse3.pc.in \
	README* \
	test/*.py \
	test/pytest.ini

pkgconfigdir = @pkgconfigdir@
pkgconfig_DATA = fuse3.pc

$(pkgconfig_DATA): config.status

.PHONY: setuid_fusermount
setuid_fusermount:
	@echo "Attempting to use sudo to make util/fusermount setuid root"
	@echo "If this fails, set permissions manually and re-run make test"
	test $$(ls -n util/fusermount | awk 'NR==1 {print $$3}') -eq 0 || \
	    sudo chown root util/fusermount
	test -u util/fusermount || \
	    sudo chmod u+s util/fusermount

# If we are not root, util/fusermount needs to be setuid root
# for tests to work.

test_deps = $(shell [ "$${UID}" -eq 0 ] || echo setuid_fusermount)
.PHONY: test
test: all $(test_deps)
	python3 -m pytest test/
