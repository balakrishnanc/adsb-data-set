TS:=$(shell date +'%s%z')

# Output data path.
DATA_DIR:=./data
RAW_DATA:=$(DATA_DIR)/raw
PROC_DATA:=$(DATA_DIR)/proc

# URL that provides a listing of _all_ airborne aircraft.
BASE_URL:=https://opensky-network.org/api/states/all


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
# OpenSky REST API supports certain query parameters for selecting, filtering
#  data, and avoiding rate limiting by the server. Refer to the documentation on
#  accessing the data at https://opensky-network.org/apidoc/rest.html for more
#  details.
$(RAW_DATA)/$(TS)-aclist.json:
	@mkdir -p $(RAW_DATA)
	@wget -q -O $@ $(BASE_URL)

$(PROC_DATA)/%-acpos.txt: $(RAW_DATA)/%-aclist.json
	@mkdir -p $(PROC_DATA)
	@jq -r '.states[]																	| \
			{         icao24: .[ 0],		\
			        callsign: .[ 1],		\
			  origin_country: .[ 2],		\
			   time_position: .[ 3],		\
			    last_contact: .[ 4],		\
			       longitude: .[ 5],		\
			        latitude: .[ 6],		\
			    geo_altitude: .[ 7],		\
			       on_ground: .[ 8],		\
			        velocity: .[ 9],		\
			      true_track: .[10],	\
			   vertical_rate: .[11],	\
			         sensors: .[12],	\
			   baro_altitude: .[13],	\
			           sqawk: .[14],	\
			             spi: .[15],	\
			 position_source: .[16]}													| \
			select(.time_position and .position_source == 0)							| \
			[.time_position, .latitude, .longitude, .baro_altitude, .callsign, .icao24]	| \
			@csv' < $< > $@
