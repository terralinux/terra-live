ver=$(shell date +%Y.%m)
#ver=2011.04-r1-rc2

WORKDIR=work-$(ARCH)
INSTALL_DIR=terra
COMPRESS=xz
LABLE=TERRA_PLANKTON_r1

ARCH?=$(shell uname -m)

PWD=$(shell pwd)
LIVEname=$(PWD)/terra-$(ver)-$(ARCH).iso

PACKAGES="$(shell grep -v '\#' packages.$(ARCH))"

kver_FILE=$(WORKDIR)/root-image/etc/mkinitcpio.d/kernel26.kver

all: live-iso

# Rules for each type of image
live-iso: $(LIVEname)

$(LIVEname): base-fs overlay-pkgs
	mkterraiso -v -f -L $(LABLE) -D $(INSTALL_DIR) -c $(COMPRESS) -x iso $(WORKDIR) $@

# This is the main rule for make the working filesystem.
base-fs: root-image bootfiles initcpio overlay iso-mounts


# Rules for make the root-image for base filesystem.
root-image: $(WORKDIR)/root-image/.terra-chroot
$(WORKDIR)/root-image/.terra-chroot:
	mkterraiso -v -C overlay/etc/pacman.conf -D $(INSTALL_DIR) -p $(PACKAGES) create $(WORKDIR)
	pacman -Qr $(WORKDIR)/root-image > $(WORKDIR)/root-image/root-image-pkgs.txt

# Rule for make /boot
bootfiles: root-image
	mkdir -p $(WORKDIR)/iso/$(INSTALL_DIR)/boot/$(ARCH)
	cp $(WORKDIR)/root-image/boot/System.map26 $(WORKDIR)/iso/$(INSTALL_DIR)/boot/$(ARCH)/
	cp $(WORKDIR)/root-image/boot/vmlinuz26 $(WORKDIR)/iso/$(INSTALL_DIR)/boot/$(ARCH)/
	cp -r boot-files/isolinux $(WORKDIR)/iso/
	cp $(WORKDIR)/root-image/usr/lib/syslinux/isolinux.bin $(WORKDIR)/iso/isolinux/

# Rules for initcpio images
initcpio: $(WORKDIR)/iso/$(INSTALL_DIR)/boot/$(ARCH)/terraiso.img
$(WORKDIR)/iso/$(INSTALL_DIR)/boot/$(ARCH)/terraiso.img: mkinitcpio.conf $(WORKDIR)/root-image/.terra-chroot
	mkdir -p $(WORKDIR)/iso/$(INSTALL_DIR)/boot/$(ARCH)/
	mkinitcpio -c ./mkinitcpio.conf -b $(WORKDIR)/root-image -k $(shell grep ^ALL_kver $(kver_FILE) | cut -d= -f2) -g $@


# overlay filesystem
overlay:
	mkdir -p $(WORKDIR)/overlay/etc/pacman.d
	cp -r overlay $(WORKDIR)/
	wget -O $(WORKDIR)/overlay/etc/pacman.d/mirrorlist http://archive.terralinux.org/mirrorlist/mirrorlist
	sed -i "s/#Server/Server/g" $(WORKDIR)/overlay/etc/pacman.d/mirrorlist
	sed -i -e "s/@carch@/$(ARCH)/g" $(WORKDIR)/overlay/etc/pacman.d/mirrorlist
	cp $(WORKDIR)/overlay/etc/locale.gen $(WORKDIR)/root-image/etc
	chroot "$(WORKDIR)/root-image" locale-gen
	cp $(WORKDIR)/root-image/etc/locale.gen.bak $(WORKDIR)/root-image/etc/locale.gen
	mkdir -p $(WORKDIR)/overlay/usr/lib/locale/
	mv $(WORKDIR)/root-image/usr/lib/locale/locale-archive $(WORKDIR)/overlay/usr/lib/locale/
	chmod -R 755 $(WORKDIR)/overlay/home


# Rule to process isomounts file.
iso-mounts: $(WORKDIR)/iso/$(INSTALL_DIR)/isomounts
$(WORKDIR)/iso/$(INSTALL_DIR)/isomounts: isomounts root-image
	sed "s|@ARCH@|$(ARCH)|g" isomounts > $@


# Rule for make the overlay-packages
overlay-pkgs:
	./overlay-pkgs.sh $(ARCH)


# Clean-up all work
clean:
	rm -rf $(WORKDIR) $(LIVEname)


.PHONY: all
.PHONY: base-fs
.PHONY: root-image bootfiles initcpio overlay iso-mounts
.PHONY: overlay-pkgs
.PHONY: clean
