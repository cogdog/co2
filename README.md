# Single Page Update Screen for CO2 Data

By request of Mike Kelly for a museum kiosk to display data on C02 emissions measure at Manau Loa and reported by the [NOAA Earth Science Research Data trends site](http://www.esrl.noaa.gov/gmd/ccgg/trends/index.html).

The RSS Feed http://www.esrl.noaa.gov/gmd/webdata/ccgg/trends/rss.xml contains a mixture of monthly and weekly updates; it is the Weekly one desired because they give the data from the current week, a year ago, and 10 years ago. A sample entry:

```
<item>
  <title>Weekly CO2 Update for August 21, 2016</title>
  <link>
    http://www.esrl.noaa.gov/gmd/ccgg/trends/weekly.html
  </link>
  <guid isPermaLink="false">2016-8-21</guid>
  <description>
    Up-to-date weekly average CO2 at Mauna Loa <br> Week starting on August 21, 2016: 401.97 ppm <br> Weekly value from 1 year ago: 398.97 ppm <br> Weekly value from 10 years ago: 379.88 ppm
  </description>
  <pubDate>Mon, 29 Aug 2016 05:01:28 MDT</pubDate>
</item>
```


My solution for this demonstration was to use [Yahoo Query Language](https://developer.yahoo.com/yql/) on this feed to return from the  results just the description text where the item title starts with `Weekly CO2 Update` and to limit the results to 1 item, e.g. the most recent. The YQL query for this is

`https://query.yahooapis.com/v1/public/yql?q=select%20description%20from%20feed%20where%20url%3D'http%3A%2F%2Fwww.esrl.noaa.gov%2Fgmd%2Fwebdata%2Fccgg%2Ftrends%2Frss.xml'%20and%20title%20LIKE%20'Weekly%20CO2%20Update%25'%20LIMIT%201&format=json&diagnostics=true&callback=output_latest_co2_stats`

also available via an alias

http://query.yahooapis.com/v1/public/yql/cogdog/c02

This makes the parsing simple, we just need to populate a div with the value of the 1st result item. To make ot self updating, a JavaScript `setTimeout()` function is used to reload the page every 24 hours.



