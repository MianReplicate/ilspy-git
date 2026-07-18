pkgname=ilspy-git
pkgver=v10.1.r832.gc40c299ff
pkgrel=1
pkgdesc=".NET Decompiler with support for PDB generation, ReadyToRun, Metadata (&more)"
arch=('any')
url="https://github.com/icsharpcode/ILSpy"
depends=('dotnet-runtime')
makedepends=('git' 'dotnet-sdk-preview-bin' 'powershell' 'imagemagick')
license=('MIT')
source=("git+https://github.com/icsharpcode/ILSpy")
sha512sums=('SKIP')
options=(!debug)

pkgver() {
    cd "$srcdir/ILSpy"
		git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

# https://github.com/icsharpcode/ILSpy?tab=readme-ov-file#how-to-build
# `ILSpy/ILSpy.csproj` is the `ilspy` project
build() {
	cd "$srcdir/ILSpy"
	dotnet publish -c Release -o ../publish --no-self-contained ILSpy/ILSpy.csproj
}

package() {
	mkdir -p "$pkgdir/opt/ilspy/"
	mkdir -p "$pkgdir/usr/bin/"
	cp -R "$srcdir/publish/"* "$pkgdir/opt/ilspy/"
	chmod 755 "$pkgdir/opt/ilspy/ILSpy"
	ln -s /opt/ilspy/ILSpy "$pkgdir/usr/bin/ilspy"
	   
	magick \
	"$srcdir/ILSpy/ILSpy/Assets/ILSpy.ico[0]" \
	 "$srcdir/ILSpy/ILSpy/Assets/ILSpy.png"
	    
	install -Dm644 \
        "$srcdir/ILSpy/ILSpy/Assets/ILSpy.png" \
        "$pkgdir/usr/share/icons/hicolor/48x48/apps/ilspy.png"
    
    install -Dm644 /dev/stdin \
        "$pkgdir/usr/share/applications/ilspy.desktop" <<EOF
[Desktop Entry]
Type=Application
Name=ILSpy
Comment=.NET Decompiler with support for PDB generation, ReadyToRun, Metadata (&more)
Exec=ilspy %F
Icon=ilspy
Terminal=false
Categories=Development;
EOF
}
