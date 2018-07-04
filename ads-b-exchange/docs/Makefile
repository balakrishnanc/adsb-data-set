ALL:=data-access-doc.html data-format-doc.html


.PHONY: all clean

all: $(ALL)

clean:
	@rm -f $(ALL)



data-access-doc.html:
	@wget -q https://www.adsbexchange.com/data/ -O $@


data-format-doc.html:
	@wget -q http://www.virtualradarserver.co.uk/Documentation/Formats/AircraftList.aspx -O $@
