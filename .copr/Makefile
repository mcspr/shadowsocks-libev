deps:
	dnf -y install rpkg

srpm: deps
	rpkg --path rpm/ srpm --outdir $(outdir)

all: srpm

.PHONY: all
