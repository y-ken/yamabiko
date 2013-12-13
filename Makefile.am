SUBDIRS = deps/jemalloc deps/ruby

#bin_SCRIPTS = td
sbin_SCRIPTS = yamabiko

MOSTLYCLEANFILES = $(sbin_SCRIPTS)

unexport GEM_HOME
unexport GEM_PATH
DEST_RUBY = $(DESTDIR)$(RUBY_BINDIR)/ruby
DEST_GEM = $(DEST_RUBY) -rrubygems -rrubygems/gem_runner -rrubygems/exceptions -rrubygems/installer -e 'class Gem::Installer; def shebang(bin_file_name) "\#\!$(RUBY_BINDIR)/ruby" end; end; Gem::GemRunner.new.run ARGV'
DEST_FLUENTD_GEM = $(DEST_RUBY) $(DESTDIR)$(RUBY_BINDIR)/gem build fluentd.gemspec
DEST_RUBY_LIBDIR = $(DESTDIR)$(RUBY_LIBDIR)/ruby
PLUGINS = fluent-plugin-td-monitoring-0.1.0.gem

#td: plugins
#	echo '#!/bin/sh' > $@
#	echo 'export GEM_HOME="$(RUBY_LIBDIR)/ruby/gems/1.9.1/"' >> $@
#	echo 'export GEM_PATH="$(RUBY_LIBDIR)/ruby/gems/1.9.1/"' >> $@
#	echo '$(RUBY_BINDIR)/td "$$@"' >> $@
#	chmod 755 $@

yamabiko: plugins
	echo '#!$(RUBY_BINDIR)/ruby' > $@
	echo 'ENV["GEM_HOME"]="$(RUBY_LIBDIR)/ruby/gems/1.9.1/"' >> $@
	echo 'ENV["GEM_PATH"]="$(RUBY_LIBDIR)/ruby/gems/1.9.1/"' >> $@
	echo 'ENV["FLUENT_CONF"]="$(sysconfdir)/yamabiko/yamabiko.conf"' >> $@
	echo 'ENV["FLUENT_PLUGIN"]="$(sysconfdir)/yamabiko/plugin"' >> $@
	echo 'ENV["FLUENT_SOCKET"]="$(localstatedir)/run/yamabiko/yamabiko.sock"' >> $@
	echo 'load "$(RUBY_BINDIR)/fluentd"' >> $@
	chmod 755 $@

plugins:
	mkdir -p "$(srcdir)/plugins/"
	for gem in $(PLUGINS); do \
		if [ ! -f "$(srcdir)/plugins/$$gem" ]; then \
			wget "http://rubygems.org/downloads/$$gem" -O "$(srcdir)/plugins/$$gem"; \
		fi; \
	done

install: install-recursive
	for gem in $(srcdir)/deps/*.gem; do \
		RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed 's/.*\[\(.*\)\]/\1/')" \
			$(DEST_GEM) install --no-rdoc --no-ri "$$gem"; \
	done
	cd "$(srcdir)" && \
		RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed 's/.*\[\(.*\)\]/\1/')" \
		$(DEST_FLUENTD_GEM); \
		mkdir -p $(srcdir)/pkg; \
		cp -f fluentd-*.gem $(srcdir)/pkg
	for gem in $(srcdir)/pkg/*.gem; do \
		RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed 's/.*\[\(.*\)\]/\1/')" \
			$(DEST_GEM) install --no-rdoc --no-ri "$$gem"; \
	done
	for gem in $(PLUGINS); do \
		RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed 's/.*\[\(.*\)\]/\1/')" \
			$(DEST_GEM) install --no-rdoc --no-ri "$(srcdir)/plugins/$$gem"; \
	done
	mkdir -p $(DESTDIR)$(sysconfdir)/yamabiko
	cp -f $(srcdir)/yamabiko.conf $(DESTDIR)$(sysconfdir)/yamabiko/yamabiko.conf.tmpl
	mkdir -p $(DESTDIR)$(sysconfdir)/yamabiko/prelink.conf.d
	cp -f $(srcdir)/yamabiko.prelink.conf $(DESTDIR)$(sysconfdir)/yamabiko/prelink.conf.d/yamabiko.conf
	mkdir -p $(DESTDIR)$(sysconfdir)/yamabiko/logrotate.d
	cp -f $(srcdir)/yamabiko.logrotate $(DESTDIR)$(sysconfdir)/yamabiko/logrotate.d/yamabiko.logrotate
	mkdir -p $(DESTDIR)$(sysconfdir)/yamabiko/plugin

dist:
	cd $(srcdir) && ./make_dist.sh

.PHONY: plugins
