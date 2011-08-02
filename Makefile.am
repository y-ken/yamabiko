
SUBDIRS = deps/ruby

sbin_SCRIPTS = td-agent

MOSTLYCLEANFILES = $(sbin_SCRIPTS)

DEST_RUBY = $(DESTDIR)$(RUBY_BINDIR)/ruby
DEST_GEM = $(DEST_RUBY) -rrubygems -rrubygems/gem_runner -rrubygems/exceptions -rrubygems/installer -e 'class Gem::Installer; def shebang(bin_file_name) "\#\!$(RUBY_BINDIR)/ruby" end; end; Gem::GemRunner.new.run ARGV'
DEST_RAKE = $(DEST_RUBY) -rrake -e 'Rake.application.run'
DEST_RUBY_LIBDIR = $(DESTDIR)$(RUBY_LIBDIR)/ruby

td-agent:
	echo '#!$(RUBY_BINDIR)/ruby' > $@
	echo 'ENV["FLUENT_CONF"]="$(sysconfdir)/td-agent/td-agent.conf"' >> $@
	echo 'ENV["FLUENT_PLUGIN"]="$(sysconfdir)/td-agent/plugin"' >> $@
	echo 'ENV["FLUENT_SOCKET"]="$(localstatedir)/run/td-agent/td-agent.sock"' >> $@
	echo 'load "$(RUBY_BINDIR)/fluentd"' >> $@
	chmod 755 $@

install: install-recursive
	for gem in $(srcdir)/deps/*.gem; do \
		RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed -E 's/.*\[(.*)\]/\1/')" \
			$(DEST_GEM) install --no-rdoc --no-ri "$$gem"; \
	done
	cd "$(srcdir)" && \
		RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed -E 's/.*\[(.*)\]/\1/')" \
		$(DEST_RAKE)
	RUBYLIB="$(DEST_RUBY_LIBDIR)/1.9.1:$(DEST_RUBY_LIBDIR)/1.9.1/$(shell $(DEST_RUBY) --version | sed -E 's/.*\[(.*)\]/\1/')" \
		$(DEST_GEM) install --no-rdoc --no-ri "$(srcdir)/pkg/fluent-$(VERSION).gem"
	mkdir -p $(DESTDIR)$(sysconfdir)/td-agent
	cp -f $(srcdir)/td-agent.conf $(DESTDIR)$(sysconfdir)/td-agent/td-agent.conf
	mkdir -p $(DESTDIR)$(sysconfdir)/td-agent/plugin

dist:
	cd $(srcdir) && ./make_dist.sh
