deps:
	dnf -y install rpkg git

srpm: deps
	rpkg --path rpm/ srpm --outdir $(outdir)

all: srpm

.PHONY: all
