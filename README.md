# maps-docker-compose

It should init into a working OpenStreetMap server with tiles, osrm and vroom, and nominatim.
You'll want to edit the following files 

- `postgres/postgresql.conf` with suitable values for your server
- `osm.env` with the pbf you want to import
- `osm-config.sh` 
- `osrm-frontend/leaflet_options.js` 
- `nginx/default.conf` will likely need to be tweaked.

After running `docker-compose up -d` depending on what pbf you've configured to import,
it will download 400G or more, then process/import/setup. It might take an hour or longer
depending on how fast your internet and server is. After it finishes importing, 
`renderd-initdb` and `nominatim-initdb` will exit 0. 

You can keep everything updated by running daily `docker-compose start renderd-updatedb` and 
`docker-compose start nominatim-updatedb`

The osrm-backend container will detect if the binary gets updated with a `docker-compose pull` and regenerate the 
osrm files.

If you `grep -r maps.localnet *` in the `maps-docker-compose` directory, you'll see every file 
you'll need to edit if you want to assign a domain name. Otherwise for testing you can edit
your `/etc/hosts` file with (ie) `127.0.0.1 maps.localnet nominatim.maps.localnet` if you were
testing locally. Then after it's finished starting up, you can in a web browser, browse to
`http://maps.localnet:8000` and `http://nominatim.maps.localnet:8000`

I'm happy to hear about any comments or issues, open an issue in github.
