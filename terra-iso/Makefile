all:

install: all
	# install to sbin since script only usable by root
	install -D -m 755 mkterraiso $(DESTDIR)/usr/sbin/mkterraiso
	install -D -m 755 ../mkterraroot/mkterraroot $(DESTDIR)/usr/sbin/mkterraroot
	# testiso can be used by anyone
	install -D -m 755 testiso $(DESTDIR)/usr/bin/testiso
	# hooks/install are needed by mkinitcpio
	install -D -m 644 hooks/terraiso $(DESTDIR)/lib/initcpio/hooks/terraiso
	install -D -m 644 install/terraiso $(DESTDIR)/lib/initcpio/install/terraiso
	install -D -m 644 hooks/terraiso_pxe_nbd $(DESTDIR)/lib/initcpio/hooks/terraiso_pxe_nbd
	install -D -m 644 install/terraiso_pxe_nbd $(DESTDIR)/lib/initcpio/install/terraiso_pxe_nbd
	install -D -m 644 hooks/terraiso_loop_mnt $(DESTDIR)/lib/initcpio/hooks/terraiso_loop_mnt
	install -D -m 644 install/terraiso_loop_mnt $(DESTDIR)/lib/initcpio/install/terraiso_loop_mnt
	# install docs and examples
	install -d -m 755 $(DESTDIR)/usr/share/terraiso/
	cp -r ../configs $(DESTDIR)/usr/share/terraiso/configs
	install -D -m 644 ../README $(DESTDIR)/usr/share/doc/terraiso/README

uninstall:
	rm -f $(DESTDIR)/usr/sbin/mkterraiso
	rm -f $(DESTDIR)/usr/sbin/mkterraroot
	rm -f $(DESTDIR)/usr/bin/testiso
	rm -f $(DESTDIR)/lib/initcpio/hooks/terraiso
	rm -f $(DESTDIR)/lib/initcpio/install/terraiso
	rm -f $(DESTDIR)/lib/initcpio/hooks/terraiso_pxe_nbd
	rm -f $(DESTDIR)/lib/initcpio/install/terraiso_pxe_nbd
	rm -rf $(DESTDIR)/usr/share/terraiso/
	rm -rf $(DESTDIR)/usr/share/doc/terraiso/
