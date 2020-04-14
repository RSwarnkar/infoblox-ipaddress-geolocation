# Map IP addresses to locations
If you don't see performance data for some of your customers on the world map, it may be because those customers have private IP addresses. You can map such private IP addresses to geographic regions to make them visible on the map. You can even override settings for customer IP addresses if necessary for mapping purposes.

Map your customers’ private IP addresses to geographic regions so that you can analyze the application performance that your customers experience based on where they access your application from. Use the CSV import for a batch upload of your custom mappings from IP addresses to geographic regions, see the sample CSV file for complete instructions. Enter either one single IP address or a range like from 196.1.20.33 to 196.1.20.41 for one location or you can also use the CIDR notation with an IP address and character "/" followed by the subnetmask, for example: 196.1.29.0/26. The subnetmask attribute value is the number of non-zero bits as used in the CIDR notation.

Granularity of regional performance analysis increases as the number of detected user requests goes up in a specific region (continent, country, state, or city). You can even override auto-detected IP addresses if necessary to improve mapping accuracy.



# Dynatrace built in IP mapping

Dynatrace uses an IP address mapping to geolocation service offered by Maxmind.com GeoIP2. The names for regions and cities are following the Geonames.org database.

To find out which names and ids are used out of the box, you can download a list of regions for each country and a list of country files with city names.

