ALL:=data-api-doc.html


.PHONY: all clean

all: $(ALL)

clean:
	@rm -f $(ALL)



data-api-doc.html:
	@wget -q https://opensky-network.org/apidoc/rest.html -O $@
