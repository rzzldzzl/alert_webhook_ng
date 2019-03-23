# alert_webhook_ng

Modified Splunk webhook alert action to send all results.

See: https://docs.splunk.com/Documentation/Splunk/7.2.5/Alert/Webhooks

CsvResultParser.py borrowed from: https://github.com/simcen/alert_manager/blob/develop/src/bin/lib/CsvResultParser.py

todo:
- JSON validation

Example:
```
index=_internal sourcetype=splunkd OR sourcetype=splunkd_access
| bin span=5m _time
| stats count by _time sourcetype
| sendalert webhook_ng param.url="<URL>" param.metadata_json="{'trigger_time':'$trigger_time$','app':'$app$','foo':'bar'}"

           _time              sourcetype   count
--------------------------- -------------- -----
2019-03-22 19:00:00.000 CDT splunkd          824
2019-03-22 19:00:00.000 CDT splunkd_access    13
2019-03-22 19:05:00.000 CDT splunkd          929
2019-03-22 19:05:00.000 CDT splunkd_access    19
```
Output:
```
{
  "sid": "1553300107.105",
  "search_name": "",
  "results": [
    {
      "count": "824",
      "_time": "1553299200",
      "sourcetype": "splunkd"
    },
    {
      "count": "13",
      "_time": "1553299200",
      "sourcetype": "splunkd_access"
    },
    {
      "count": "929",
      "_time": "1553299500",
      "sourcetype": "splunkd"
    },
    {
      "count": "19",
      "_time": "1553299500",
      "sourcetype": "splunkd_access"
    }
  ],
  "app": "search",
  "foo": "bar",
  "trigger_time": "1553300108.167966"
}
```
