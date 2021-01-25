SRC := $(shell find 'src' -name '*.md')
OUT := $(SRC:%.md=%.html)
CSS = /style.css

all: $(OUT)

src/%.html: src/%.md default.html
	pandoc 	--css $(CSS) \
		--email-obfuscation=javascript \
		--mathjax \
		--toc --toc-depth=2 \
		--standalone \
		--template default.html \
		--output $@ $<

# Clone the GitLab repository to ~/.cache/polhode.org before deploying.
deploy: all
	rsync --archive --delete --exclude='*.md' --exclude='.git' --update src/ ~/.cache/polhode.org/
	cd ~/.cache/polhode.org ; \
	git add --all ; \
	git diff-index --quiet HEAD || git commit --allow-empty-message -m '' ; \
	git push -f

server: all
	cd src; python3 -m http.server

clean:
	rm -f $(OUT)

.PHONY: clean
