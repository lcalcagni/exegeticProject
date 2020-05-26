---
layout: default
title: Getting familiar with the API using Python
nav_order: 2
---

# Getting familiar with the API using Python
{: .no_toc }

In this section I explore the Trundler API using Python.

I convert the response of the requests into a Pandas DataFrame, then I get the names of the columns and shape in order to know the type of data I am dealing with.

{: .fs-6 .fw-300 }

[Download Jupyter Notebook for this section](https://github.com/lcalcagni/exegeticProject/blob/master/notebooks/APIpython.ipynb){: .btn .fs-3 .mb-2 .mb-md-0 }

---
## Requests and responses
I write the following script to get the desired information:

```python
import requests
import json
import pandas as pd

headers = {
    'accept': 'application/json',
    'X-Api-Key': {my_API_key},
}

retailer_id = {retailer_id}
product_id = {product_id}

endpoints = ['/retailer',
            '/retailer/'+ str(retailer_id),
            '/retailer/'+ str(retailer_id) +'/product',
            '/product',
            '/product/'+ str(product_id),
            '/product/'+ str(product_id) +'/price'
           ]

for end in endpoints:
    response = requests.get('https://api.trundler.dev/'+end, headers=headers)
    j_response = json.loads(response.text)
    df = pd.DataFrame(pd.json_normalize(j_response))

    print("--Endpoint--", end)
    print("Shape: ",df.shape)
    print("Columns: ",df.columns,"\n")
```
<br><br>

The results can be summarized in the following tables:
<br>


|         | /retailer | /retailer/{id} | retailer/{id}/product |
|---------|-----------|---------------:|-----------------------|
| **shape**   |   (#num_retailers,4)        |    (1,4)            |  (#num_retailer_products,7)                     |
| **columns** |   id, retailer, url, currency        |   id, retailer, url, currency               |  url, product, model, sku, retailer_id, id, brand  |

<br>


|         |  /product | /product/{id} |/product/{id}/price
|---------|----------|---------------|---------------|
| **shape**   |   (#num_products,7)       |      (1,8)         |    (#num_prices,5)         |
| **columns** |  url, product, model, sku, retailer_id, id, brand | url, product, barcodes, model, sku, retailer_id, id, brand | product_id, time, price, price_promotion, available  |

<br>
<br>

## Conclusion of this section
It is a good occasion to get used to R and make use of the package already written in R :)
