PKG = season_plot

$(PKG).gfn: $(PKG).inp $(PKG).spec $(PKG)_sample.inp $(PKG)_help.md
	gretlcli --makepkg $(PKG).inp

install: $(PKG).gfn
	echo pkg install $(PKG).gfn --local | gretlcli -b -e -
	cat $(PKG)_help.md | tee ../README.md

clean:
	rm -f $(PKG).gfn
	rm -f *.png
	rm -f session.inp
