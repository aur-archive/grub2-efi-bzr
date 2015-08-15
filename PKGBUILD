# Maintainer : Keshav P R <(the.ridikulus.rat) (aatt) (gemmaeiil) (ddoott) (ccoomm)>

_pkgname="grub2-efi-bzr"

pkgname="${_pkgname}"                   ## Dummy pkgname for AUR interface

# pkgname="grub2-efi-x86_64-bzr"        ## Uncomment for grub BZR Main Trunk for x86_64-UEFI
# pkgname="grub2-efi-x86_64-bzr-exp"    ## Uncomment for grub BZR Experimental Branch for x86_64-UEFI
# pkgname="grub2-efi-i386-bzr"          ## Uncomment for grub BZR Main Trunk for i386-UEFI
# pkgname="grub2-efi-i386-bzr-exp"      ## Uncomment for grub BZR Experimental Branch for i386-UEFI

pkgver="4343"
url="http://www.gnu.org/software/grub/"
arch=('i686' 'x86_64')
license=('GPL3')

makedepends=('bzr' 'rsync' 'xz' 'bdf-unifont' 'python' 'autogen' 'texinfo' 'help2man' 'gettext' 'device-mapper' 'fuse')
depends=('xz' 'freetype2' 'device-mapper' 'fuse' 'gettext' 'dosfstools' 'efibootmgr' 'sh')
optdepends=('libisoburn: provides xorriso for generating grub rescue iso using grub-mkrescue'
            'mtools: for manipulating FAT fs image files')

install="${_pkgname}.install"
backup=('boot/grub/grub.cfg' 'etc/default/grub' 'etc/grub.d/40_custom')
conflicts=('grub2-common')
provides=('grub2-common')

source=('grub.default'
        'grub.cfg'
        'archlinux_grub_mkconfig_fixes.patch')

sha256sums=('df764fbd876947dea973017f95371e53833bf878458140b09f0b70d900235676'
            'fcace8f0f0b4b8dc013c85296d59663f9f6695c49e92c1cb492b7a0f84164070'
            '9f570f72475076c54ee1c1b0e0646edd2a712846c519dfb17a4d8bcb3489ceba')


case "${pkgname}" in
	grub2-efi-x86_64-bzr )
		pkgdesc="The GNU GRand Unified Bootloader version 2 - x86_64 UEFI - BZR Main Trunk with grub-extras"
		pkgrel="1"
		conflicts+=('grub2-efi-x86_64')
		provides+=('grub2-efi-x86_64')
		_UEFI_ARCH="x86_64"
		_bzrtrunk="bzr://bzr.savannah.gnu.org/grub/trunk/grub/"
		# _bzrtrunk="lp:grub/grub2"
		_bzrmod="grub"
	;;
	grub2-efi-x86_64-bzr-exp )
		pkgdesc="The GNU GRand Unified Bootloader version 2 - x86_64 UEFI - BZR Experimental branch with grub-extras"
		pkgrel="1"
		conflicts+=('grub2-efi-x86_64')
		provides+=('grub2-efi-x86_64')
		_UEFI_ARCH="x86_64"
		_bzrtrunk="bzr://bzr.savannah.gnu.org/grub/branches/experimental/"
		# _bzrtrunk="lp:~the-ridikulus-rat/grub/grub2-bzr-exp"
		_bzrmod="grub_exp"
	;;
	grub2-efi-i386-bzr )
		pkgdesc="The GNU GRand Unified Bootloader version 2 - i386 UEFI - BZR Main Trunk with grub-extras"
		pkgrel="1"
		conflicts+=('grub2-efi-i386')
		provides+=('grub2-efi-i386')
		_UEFI_ARCH="i386"
		_bzrtrunk="bzr://bzr.savannah.gnu.org/grub/trunk/grub/"
		# _bzrtrunk="lp:grub/grub2"
		_bzrmod="grub"
	;;
	grub2-efi-i386-bzr-exp )
		pkgdesc="The GNU GRand Unified Bootloader version 2 - i386 UEFI - BZR Experimental branch with grub-extras"
		pkgrel="1"
		conflicts+=('grub2-efi-i386')
		provides+=('grub2-efi-i386')
		_UEFI_ARCH="i386"
		_bzrtrunk="bzr://bzr.savannah.gnu.org/grub/branches/experimental/"
		# _bzrtrunk="lp:~the-ridikulus-rat/grub/grub2-bzr-exp"
		_bzrmod="grub_exp"
	;;
	grub2-efi-bzr )
		pkgdesc="The GNU GRand Unified Bootloader version 2 for UEFI systems - BZR Development Version with grub-extras - Dummy pkgname - Edit PKGBUILD for actual pkgname"
		pkgrel="1"
		_UEFI_ARCH="NULL"
	;;
esac

case "${_UEFI_ARCH}" in
	x86_64 )
		if [[ "${CARCH}" != "x86_64" ]]; then
			echo "${pkgname} package can be built only in a x86_64 system. Exiting."
			exit 1
		fi
	;;
	i386 )
		if [[ "${CARCH}" == "x86_64" ]]; then
			# makedepends+=('gcc-multilib' 'gcc-libs-multilib' 'binutils-multilib' 'libtool-multilib')
			echo
		fi
	;;
esac


## grub-extras bzr repo locations

_bzrtrunk_lua="bzr://bzr.savannah.gnu.org/grub-extras/lua/"
# _bzrtrunk_lua="lp:~the-ridikulus-rat/grub/grub2-extras-lua"

_bzrtrunk_gpxe="bzr://bzr.savannah.gnu.org/grub-extras/gpxe/"
# _bzrtrunk_gpxe="lp:~the-ridikulus-rat/grub/grub2-extras-gpxe"


_update_bzr() {
	
	msg "Connecting to BZR server..."
	
	if [[ -d "${srcdir}/${_bzrmod}" ]]; then
		cd "${srcdir}/${_bzrmod}"
		bzr pull "${_bzrtrunk}"
		msg "GRUB BZR Local repository updated."
	else
		cd "${srcdir}/"
		bzr branch "${_bzrtrunk}" "${_bzrmod}"
		msg "GRUB BZR repository cloned."
	fi
	
	if [[ -d "${srcdir}/${_bzrmod}/grub-extras" ]]; then
		cd "${srcdir}/${_bzrmod}/grub-extras/"
		
		if [[ -d "${srcdir}/${_bzrmod}/grub-extras/lua" ]]; then
			cd "${srcdir}/${_bzrmod}/grub-extras/lua"
			bzr pull "${_bzrtrunk_lua}"
			echo
		else
			bzr branch "${_bzrtrunk_lua}" lua
			echo
		fi
		
		if [[ -d "${srcdir}/${_bzrmod}/grub-extras/gpxe" ]]; then
			cd "${srcdir}/${_bzrmod}/grub-extras/gpxe"
			bzr pull "${_bzrtrunk_gpxe}"
			echo
		else
			bzr branch "${_bzrtrunk_gpxe}" gpxe
			echo
		fi
	else
		mkdir -p "${srcdir}/${_bzrmod}/grub-extras/"
		cd "${srcdir}/${_bzrmod}/grub-extras/"
		
		bzr branch "${_bzrtrunk_lua}" lua
		echo
		bzr branch "${_bzrtrunk_gpxe}" gpxe
		echo
	fi
	
	cd "${srcdir}/${_bzrmod}/"
	rsync -Lrtvz translationproject.org::tp/latest/grub/ "${srcdir}/${_bzrmod}/po" || true
	(cd "${srcdir}/${_bzrmod}/po" && ls *.po | cut -d. -f1 | xargs) > "${srcdir}/${_bzrmod}/po/LINGUAS"
	
}


build() {
	
	if [[ "${pkgname}" == "${_pkgname}" ]]; then
		echo "Please edit the PKGBUILD and uncomment the appropriate pkgname as per your UEFI arch and the grub bzr branch you want to compile."
		exit 1
	else
		_update_bzr
	fi
	
	rm -rf "${srcdir}/${_bzrmod}_build" || true
	
	rm -rf "${srcdir}/${_bzrmod}/grub-extras/zfs" || true
	
	cp -r "${srcdir}/${_bzrmod}" "${srcdir}/${_bzrmod}_build"
	cd "${srcdir}/${_bzrmod}_build"
	
	## Apply Archlinux specific fixes to enable grub-mkconfig detect Arch kernels and initramfs
	patch -Np1 -i "${srcdir}/archlinux_grub_mkconfig_fixes.patch"
	echo
	
	export GRUB_CONTRIB="${srcdir}/${_bzrmod}_build/grub-extras/"
	
	## Requires python2
	install -D -m0755 "${srcdir}/${_bzrmod}_build/autogen.sh" "${srcdir}/${_bzrmod}_build/autogen_unmodified.sh"
	# sed 's|python |python2 |g' -i "${srcdir}/${_bzrmod}_build/autogen.sh"
	echo
	
	"${srcdir}/${_bzrmod}_build/autogen.sh"
	echo
	
	## fix unifont.bdf location so grub-mkfont can create *.pf2 files
	sed 's|/usr/share/fonts/unifont|/usr/share/fonts/misc|g' -i "${srcdir}/${_bzrmod}_build/configure"
	
	mkdir -p "${srcdir}/${_bzrmod}_build/BUILD_UEFI_${_UEFI_ARCH}"
	cd "${srcdir}/${_bzrmod}_build/BUILD_UEFI_${_UEFI_ARCH}"
	
	CFLAGS="" ../configure \
		--with-platform="efi" \
		--target="${_UEFI_ARCH}" \
		--host="${CARCH}-unknown-linux-gnu" \
		--disable-efiemu \
		--enable-mm-debug \
		--enable-nls \
		--enable-device-mapper \
		--enable-cache-stats \
		--enable-grub-mkfont \
		--enable-grub-mount \
		--prefix="/usr" \
		--bindir="/usr/bin" \
		--sbindir="/usr/sbin" \
		--mandir="/usr/share/man" \
		--infodir="/usr/share/info" \
		--datarootdir="/usr/share" \
		--sysconfdir="/etc" \
		--program-prefix="" \
		--with-bootdir="/boot" \
		--with-grubdir="grub" \
		--disable-werror
	echo
	
	CFLAGS="" make
	echo
	
}


package() {
	
	cd "${srcdir}/${_bzrmod}_build/BUILD_UEFI_${_UEFI_ARCH}"
	make DESTDIR="${pkgdir}/" bashcompletiondir="/usr/share/bash-completion/completions/" install
	echo
	
	## install /etc/default/grub
	install -D -m0644 "${srcdir}/grub.default" "${pkgdir}/etc/default/grub"
	
	## install example grub config file
	install -D -m0644 "${srcdir}/grub.cfg" "${pkgdir}/boot/grub/grub.cfg"
	
	sed "s|^\(_UEFI_ARCH\)=.*|\1=${_UEFI_ARCH}|g" -i "${startdir}/${_pkgname}.install"
	
}
