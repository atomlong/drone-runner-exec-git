# Maintainer: Markus Richter <mqus at disroot dot org>

pkgname=drone-runner-exec-git
_pkgbase=drone-runner-exec
pkgver=v1.0.0.beta.9.r2.gc0a612e
pkgrel=4
pkgdesc="A CI/CD runner for drone, which runs its tasks locally on the machine without any virtualization."
arch=('i686' 'x86_64' 'armv7h' 'aarch64')
url="https://github.com/drone-runners/drone-runner-exec"
license=('custom')
depends=()
optdepends=()
makedepends=('go' 'git')
provides=("$_pkgbase") 
conflicts=("$_pkgbase-bin")
backup=('etc/drone-runner/drone-runner-exec.ini')
install=drone-runner-exec.install
source=('git+https://github.com/drone-runners/drone-runner-exec.git'
        "https://polyformproject.org/wp-content/uploads/2020/05/PolyForm-Small-Business-1.0.0.txt"
        "https://polyformproject.org/wp-content/uploads/2020/05/PolyForm-Free-Trial-1.0.0.txt"
	"config.template"
	"${_pkgbase}.install"
	"${_pkgbase}.service"
	"${_pkgbase}.sysusers.conf"
	"${_pkgbase}.tmpfiles.conf")
sha512sums=('SKIP'
            '34514288fd78afcc21b86ff5d23dc0e4ccc8dc67d2db032480a3cfb89de5ad9bb19370cad8049a104fa3d44b5e03c208dc8425aa24bcd7e763c3673c7514beb6'
            '66e97a91ce27f2711a102f47660fc407b0fdde35564c64ab3c2fae55cbdb3fadf957cafcb0b19c6be94d6a2336cbfb92ee2d8ecf58c8ef627ba601e7bfadcee0'
            'bcd8e0fcef52473e4c02e711a30d58e87329c30036ae7761f4130863fa8b8b9a50e3469624b5d155c1d4b7390616eacf2303f3f71bbfbad0223048c71918affa'
            'd2de6e3d4a13a9f833ead5638bb9f6096e7611afb31eeeb3f0b85490814fb2110624239d303b40c7120898fb8d1240ff8fc3109ae5f105fc966210744a32553a'
            'bf10ee608a36d61a54ccbfe75a421989793ba3f3490624b1849c13b2f772426ff432a3b8e8500990de3391da10484e731104da3d5a5de692f88e2237974a1af1'
            '5873dfc97c54ba4be052d49ffbb7c6eb7f8b472452a13c365eb5fad8c4e5e92046c10399ceee318e8cc91c61c865888148116db3dc884cf9cc076828790b22ef'
            '4d679920e3e35d8d1f87ce70e467c99abd045499d59f4057a75e7ebd026241e2a6315ca962c71051ccd2d208b3409928edd8fe11efbe42a746fde9479141d917')


pkgver() {
	cd "$srcdir/$_pkgbase"
	( set -o pipefail
	    git describe --long --tags 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
	    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
	)
}

build() {
	#build drone-runner-exec
	cd "$srcdir/$_pkgbase"
	
	go build \
		-trimpath \
		-buildmode=pie \
		-mod=readonly \
		-modcacherw \
		-ldflags "-linkmode external -extldflags \"${LDFLAGS}\""
}

package() {
	# setup systemd service
	install -D -m 0644 "$srcdir/drone-runner-exec.service" "$pkgdir/usr/lib/systemd/system/drone-runner-exec.service"
	
	# declarative setup of user and directory
	install -D -m 0644 "$srcdir/drone-runner-exec.sysusers.conf" "$pkgdir/usr/lib/sysusers.d/drone-runner-exec.conf"
	install -D -m 0644 "$srcdir/drone-runner-exec.tmpfiles.conf" "$pkgdir/usr/lib/tmpfiles.d/drone-runner-exec.conf"
	
	# copy license files
	install -D -m 0644 "$srcdir/PolyForm-Small-Business-1.0.0.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
	install -D -m 0644 "$srcdir/PolyForm-Free-Trial-1.0.0.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
	
	# copy default config file
	install -D -m 0644 "$srcdir/config.template" "$pkgdir/etc/drone-runner/drone-runner-exec.ini"
	
	# copy binary
	install -D -m 0755 "$srcdir/$_pkgbase/drone-runner-exec" "$pkgdir/usr/bin/drone-runner-exec"
}
