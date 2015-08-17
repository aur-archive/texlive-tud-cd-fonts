# Maintainer: Kevin Mäder <kmaeder[AT]kevin-maeder[dot]de>
# TU Dresden corporate design font install

# copy DIN_Bd_PS.zip and Univers_ps.zip to directory with PKGBUILD

pkgname=texlive-tud-cd-fonts
pkgver=0.2
pkgrel=2
pkgdesc="TeX Live - TU Dresden corporate design fonts"
license=('custom:tud-cd-fonts')
arch=('any')
depends=('texlive-core')
url=("http://latex.wcms-file3.tu-dresden.de/phpBB3/index.php")
install=texlive.install
source=("DIN_Bd_PS.zip" "Univers_ps.zip" "texlive-tud-cd-fonts.maps" "LICENSE")
md5sums=('SKIP' 'SKIP' '30ccb4195c22f5cba515d3f1db56c707' '6cfb9405ed5429fb03883fad19153fdc')

# if you change the path here make sure to change it in texlive.install too
TEXPATH=/usr/share/texmf

build() {
	cd $srcdir
	echo ">>> rename font files"
	mv -f DINBd___.pfb dinb8a.pfb
	mv -f DINBd___.afm dinb8a.afm
	mv -f uvceb___.pfb aunb8a.pfb
	mv -f uvceb___.afm aunb8a.afm
	mv -f uvcel___.pfb aunl8a.pfb
	mv -f uvcel___.afm aunl8a.afm
	mv -f uvceo___.pfb aunro8a.pfb
	mv -f uvceo___.afm aunro8a.afm
	mv -f uvxbo___.pfb aunbo8a.pfb
	mv -f uvxbo___.afm aunbo8a.afm
	mv -f uvxlo___.pfb aunlo8a.pfb
	mv -f uvxlo___.afm aunlo8a.afm
	mv -f uvce____.pfb aunr8a.pfb
	mv -f uvce____.afm aunr8a.afm
	mv -f uvczo___.pfb aubro8a.pfb
	mv -f uvczo___.afm aubro8a.afm
	mv -f uvcz____.pfb aubr8a.pfb
	mv -f uvcz____.afm aubr8a.afm

echo ">>> create latex font and metric files"
cat > convert.tex <<%EOF
\\input fontinst.sty
\\needsfontinstversion{1.933}
\\recordtransforms{tud-cd-fonts-rec-din.tex}
\\latinfamily{din}{}
\\endrecordtransforms
\\bye
%EOF
latex convert.tex

cat > convert.tex <<%EOF
\\input fontinst.sty
\\needsfontinstversion{1.933}
\\recordtransforms{tud-cd-fonts-rec-univers.tex}
\\latinfamily{aun}{}
\\latinfamily{aub}{}
\\endrecordtransforms
\\bye
%EOF
latex convert.tex

# now we have files of type .afm .fd, .mtx, .pfb, .pl, .vpl
echo ">>> convert font and metric files to machine readable format"
for f in *.pl ; do
    pltotf $f
done

for f in *.vpl ; do
    vptovf $f
done

echo ">>> create map files"
cat > convert.tex <<%EOF
\\input finstmsc.sty
\\resetstr{PSfontsuffix}{.pfb}
\\adddriver{dvips}{dinbold.map}
\\input tud-cd-fonts-rec-din.tex
\\donedrivers
\\bye
%EOF
latex convert.tex

cat > convert.tex <<%EOF
\\input finstmsc.sty
\\resetstr{PSfontsuffix}{.pfb}
\\adddriver{dvips}{univers.map}
\\input tud-cd-fonts-rec-univers.tex
\\donedrivers
\\bye
%EOF
latex convert.tex
}

package() {
	install -m755 -d $pkgdir/var/lib/texmf/arch/installedpkgs
	install -m644 $pkgname.maps $pkgdir/var/lib/texmf/arch/installedpkgs/
    touch $pkgdir/var/lib/texmf/arch/installedpkgs/${pkgname}_${pkgver}-${pkgrel}.pkgs
    chmod 644 $pkgdir/var/lib/texmf/arch/installedpkgs/${pkgname}_${pkgver}-${pkgrel}.pkgs

	install -m755 -d $pkgdir$TEXPATH/tex/latex/tud-cd-fonts
	install -m755 -d $pkgdir$TEXPATH/fonts/tfm/adobe/tud-cd-fonts
	install -m755 -d $pkgdir$TEXPATH/fonts/vf/adobe/tud-cd-fonts
	install -m755 -d $pkgdir$TEXPATH/fonts/type1/adobe/tud-cd-fonts
	install -m755 -d $pkgdir$TEXPATH/fonts/map

	install -m644 *.fd  $pkgdir$TEXPATH/tex/latex/tud-cd-fonts/
	install -m644 *.tfm $pkgdir$TEXPATH/fonts/tfm/adobe/tud-cd-fonts/
	install -m644 *.vf  $pkgdir$TEXPATH/fonts/vf/adobe/tud-cd-fonts/
	install -m644 *.pfb $pkgdir$TEXPATH/fonts/type1/adobe/tud-cd-fonts/
	install -m644 univers.map $pkgdir$TEXPATH/fonts/map
	install -m644 dinbold.map $pkgdir$TEXPATH/fonts/map

	install -D -m 644 $srcdir/LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE:tud-cd-fonts"
}
