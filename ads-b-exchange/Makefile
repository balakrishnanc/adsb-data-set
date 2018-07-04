TS:=$(shell date +'%s%z')

# Output data path.
DATA_DIR:=./data
RAW_DATA:=$(DATA_DIR)/raw
PROC_DATA:=$(DATA_DIR)/proc

# URL that provides a listing of _all_ airborne aircraft.
BASE_URL:=http://public-api.adsbexchange.com/VirtualRadar/AircraftList.json


.PHONY: all clean clean-all


all: $(RAW_DATA)/$(TS)-aclist.json \
	$(PROC_DATA)/$(TS)-acpos.txt


clean:
	@rm -rf $(PROC_DATA)
	@mkdir -p $(PROC_DATA)

clean-all:
	@rm -rf $(DATA_DIR)
	@mkdir -p $(DATA_DIR)


# Fetch aircraft listing.
# The VRS software supports certain query parameters for filtering data or
#  avoiding duplicates. Refer to the documentation on accessing the data at
#  https://www.adsbexchange.com/data/ for more details.
$(RAW_DATA)/$(TS)-aclist.json:
	@mkdir -p $(RAW_DATA)	
	@wget -q -O $@ $(BASE_URL)

$(PROC_DATA)/%-acpos.txt: $(RAW_DATA)/%-aclist.json
	@mkdir -p $(PROC_DATA)
	@jq 'select(.src == 1)' < $< | \
	 jq -r '.acList[]																| \
			select(.Rcvr == 1 and .Bad == false and .CallSus == false and .PosTime)	| \
			[.PosTime, .Lat, .Long, .Alt, .Id, .Icao]								| \
			@csv' > $@
