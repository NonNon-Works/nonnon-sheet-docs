# Import and Export CSV and JSON

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
NonNonTable supports importing and exporting data in CSV and JSON formats.

You can perform these operations from the NonNonTable asset's Inspector.

This feature only works in the Unity Editor; it is not available in player builds.
{% endcolumn %}
{% endcolumns %}

## CSV

CSV import / export expects the following:

* Field Separator: `,`&#x20;
* Header Row: Yes
  * On import the first row is skipped, so its contents are not strictly validated.

## JSON

JSON import / export expects the following:

* Top-level array structure
* Each array element is an instance of the table's data type
