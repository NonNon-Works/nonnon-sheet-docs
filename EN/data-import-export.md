# Import and export csv and json

{% columns %}
{% column %}
<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
NonNonTable supports import and export in csv and json formats.

This can be executed from the inspector of the NonNonTable asset.

This feature only work in Unity Editor - please note that it cannot work in build.
{% endcolumn %}
{% endcolumns %}

## csv

Supports importing and exporting CSV files in the following formats:

* Field Separator: `,`&#x20;
* Header Row: Yes
  * When importing, we only skip the header row, so the contents of the header row don't necessarily need to be correct.

## json

Supports importing and exporting JSON in the following formats:

* Top-level array structure
* Array elements are the data types of the table
