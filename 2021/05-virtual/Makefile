# Makefile --- A simple Makefile for working on specs.

# This Makefile assumes you have doctoc installed. This is a tool that
# automatically generates Table of Contents for Markdown files. It can
# be installed like any other NPM module:
#
#    npm install -g doctoc

.PHONY: update-explainer-toc

update-explainer-toc: README.md Makefile
	doctoc --title "## Table of Contents" $<
