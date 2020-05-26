---
layout: default
title: US vs NZ retail prices comparison during COVID-19
nav_order: 3
---

# US vs NZ: retail prices comparison during COVID-19
{: .no_toc }


There is a retailer located both in the USA and in New Zealand. Since these are two countries with such a different evolution  of the number of COVID-19 infected people [1], I want to evaluate if there is some variability in the price of the products sold by the retailer that are shared in both countries and if it has a correlation with the number of COVID-19 cases per day in each country.

{: .fs-6 .fw-300 }

[Download Jupyter Notebook for this section](https://github.com/lcalcagni/exegeticProject/blob/master/notebooks/nzus.ipynb){: .btn .fs-3 .mb-2 .mb-md-0 }

---



```R
library(trundler)
library(dplyr)
library(sqldf)
```

```R
set_api_key({my_API_key})
```
<br>
I found this retailer which operates in US and NZ:

```R
retailer(105)
retailer(109)
```


<table>
<thead><tr><th scope="col">retailer_id</th><th scope="col">retailer</th><th scope="col">retailer_url</th><th scope="col">currency</th></tr></thead>
<tbody>
	<tr><td>105                     </td><td>CottonOn (NZ)           </td><td>https://cottonon.com/NZ/</td><td>NZD                     </td></tr>
</tbody>
</table>


<table>
<thead><tr><th scope=col>retailer_id</th><th scope=col>retailer</th><th scope=col>retailer_url</th><th scope=col>currency</th></tr></thead>
<tbody>
	<tr><td>109                     </td><td>CottonOn (US)           </td><td>https://cottonon.com/US/</td><td>USD                     </td></tr>
</tbody>
</table>
<br>
I focus on the brand products that the retailer produces and then I get all that products in NZ and US

```R
cot_on=products(brand="Cotton On")
```

```R
nz_cot_on=filter(cot_on, cot_on$retailer_id==105)
us_cot_on=filter(cot_on, cot_on$retailer_id==109)
```
<br>

I combine both DataFrames getting all the shared products in both countries (same SKU). I certainly couldn't use the ``sqldf`` package, but... I'm still getting used to R.

```R
combined_nzus=sqldf("
  SELECT
    nz_cot_on.sku,
    nz_cot_on.product,
    nz_cot_on.product_id as nz_prod_id,
    us_cot_on.product_id as us_prod_id
  FROM
    nz_cot_on
  INNER JOIN
    us_cot_on
  ON
    nz_cot_on.sku = us_cot_on.sku

")
```

<br>
```R
combined_nzus
```


<table>
<thead><tr><th scope=col>sku</th><th scope=col>product</th><th scope=col>nz_prod_id</th><th scope=col>us_prod_id</th></tr></thead>
<tbody>
	<tr><td>2010837-11                              </td><td>The Original Graphic Tee                </td><td>2415630                                 </td><td>2601194                                 </td></tr>
	<tr><td>2010837-10                              </td><td>The Original Graphic Tee                </td><td>2415633                                 </td><td>2601195                                 </td></tr>
	<tr><td>2010837-08                              </td><td>The Original Graphic Tee                </td><td>2415641                                 </td><td>2601197                                 </td></tr>
	<tr><td>2010837-05                              </td><td>The Original Graphic Tee                </td><td>2415666                                 </td><td>2601198                                 </td></tr>
	<tr><td>2010837-02                              </td><td>The Original Graphic Tee                </td><td>2415671                                 </td><td>2601200                                 </td></tr>
	<tr><td>2010808-01                              </td><td>Curve Straight Stretch High Rise Jean   </td><td>2415745                                 </td><td>2601212                                 </td></tr>
	<tr><td>2010767-03                              </td><td>Curve The One Crew Tee                  </td><td>2415903                                 </td><td>2601226                                 </td></tr>
	<tr><td>2010767-01                              </td><td>Curve The One Crew Tee                  </td><td>2415905                                 </td><td>2601232                                 </td></tr>
	<tr><td>2010768-02                              </td><td>Curve The One Scoop Tee                 </td><td>2415882                                 </td><td>2601218                                 </td></tr>
	<tr><td>2010768-01                              </td><td>Curve The One Scoop Tee                 </td><td>2415884                                 </td><td>2601223                                 </td></tr>
	<tr><td>2010721-09                              </td><td>Sheer Vintage Scoop Long Sleeve Top     </td><td>2416010                                 </td><td>2601244                                 </td></tr>
	<tr><td>2010721-06                              </td><td>Sheer Vintage Scoop Long Sleeve Top     </td><td>2416020                                 </td><td>2601245                                 </td></tr>
	<tr><td>2010721-02                              </td><td>Sheer Vintage Scoop Long Sleeve Top     </td><td>2416025                                 </td><td>2601246                                 </td></tr>
	<tr><td>2010721-01                              </td><td>Sheer Vintage Scoop Long Sleeve Top     </td><td>2416031                                 </td><td>2601247                                 </td></tr>
	<tr><td>2010569-05                              </td><td>The Kyle Oversize Long Sleeve Tee       </td><td>2416152                                 </td><td>2601273                                 </td></tr>
	<tr><td>2010569-01                              </td><td>The Kyle Oversize Long Sleeve Tee       </td><td>2416179                                 </td><td>2601274                                 </td></tr>
	<tr><td>2010754-01                              </td><td>Curve 90S Baggy Denim Jacket            </td><td>2415933                                 </td><td>2601233                                 </td></tr>
	<tr><td>2010383-02                              </td><td>Not Your Boyfriends Denim Trucker Jacket</td><td>2416413                                 </td><td>2601288                                 </td></tr>
	<tr><td>2010383-01                              </td><td>Not Your Boyfriends Denim Trucker Jacket</td><td>2416423                                 </td><td>2601289                                 </td></tr>
	<tr><td>2010375-01                              </td><td>Paperbag Pant                           </td><td>2416463                                 </td><td>2690286                                 </td></tr>
	<tr><td>2010375-01                              </td><td>Paperbag Pant                           </td><td>2416463                                 </td><td>2690286                                 </td></tr>
	<tr><td>2010483-01                              </td><td>Os Denim Jacket                         </td><td>2416234                                 </td><td>2601278                                 </td></tr>
	<tr><td>2009940-04                              </td><td>Curve Oversized Graphic License Tee     </td><td>2417544                                 </td><td>2601399                                 </td></tr>
	<tr><td>2009902-05                              </td><td>Curve Graphic Fitted Tee                </td><td>2417603                                 </td><td>2601401                                 </td></tr>
	<tr><td>2009901-07                              </td><td>Curve Oversized Graphic Tee             </td><td>2417607                                 </td><td>2601402                                 </td></tr>
	<tr><td>2009901-06                              </td><td>Curve Oversized Graphic Tee             </td><td>2417624                                 </td><td>2601403                                 </td></tr>
	<tr><td>2009901-05                              </td><td>Curve Oversized Graphic Tee             </td><td>2417632                                 </td><td>2601406                                 </td></tr>
	<tr><td>2009882-06                              </td><td>Curve Oversized Graphic Tee             </td><td>2417735                                 </td><td>2601410                                 </td></tr>
	<tr><td>2009882-05                              </td><td>Curve Oversized Graphic Tee             </td><td>2417746                                 </td><td>2601417                                 </td></tr>
	<tr><td>2009665-02                              </td><td>Sheer Vintage Scoop Tee                 </td><td>2417886                                 </td><td>2601474                                 </td></tr>
	<tr><td>...</td><td>...</td><td>...</td><td>...</td></tr>
	<tr><td>791735-33            </td><td>Baby Leggings        </td><td>2669646              </td><td>2685477              </td></tr>
	<tr><td>791735-24            </td><td>Baby Leggings        </td><td>2669650              </td><td>2685478              </td></tr>
	<tr><td>791735-24            </td><td>Baby Leggings        </td><td>2669650              </td><td>2685478              </td></tr>
	<tr><td>790341-88            </td><td>Hair Clips           </td><td>2669743              </td><td>2584797              </td></tr>
	<tr><td>790341-87            </td><td>Hair Clips           </td><td>2669745              </td><td>2584800              </td></tr>
	<tr><td>790341-85            </td><td>Hair Clips           </td><td>2669749              </td><td>2584802              </td></tr>
	<tr><td>790341-84            </td><td>Hair Clips           </td><td>2669752              </td><td>2584806              </td></tr>
	<tr><td>790341-83            </td><td>Hair Clips           </td><td>2669756              </td><td>2584812              </td></tr>
	<tr><td>790341-82            </td><td>Hair Clips           </td><td>2669759              </td><td>2584815              </td></tr>
	<tr><td>790341-81            </td><td>Hair Clips           </td><td>2669761              </td><td>2584819              </td></tr>
	<tr><td>790319-75            </td><td>Scarf Scrunchie      </td><td>2669781              </td><td>2584836              </td></tr>
	<tr><td>790319-70            </td><td>Scarf Scrunchie      </td><td>2669784              </td><td>2584842              </td></tr>
	<tr><td>790274-19            </td><td>Novelty Headband     </td><td>2669829              </td><td>2584886              </td></tr>
	<tr><td>790274-18            </td><td>Novelty Headband     </td><td>2669831              </td><td>2584891              </td></tr>
	<tr><td>790269-72            </td><td>Statement Bows       </td><td>2669836              </td><td>2584903              </td></tr>
	<tr><td>790269-09            </td><td>Statement Bows       </td><td>2669839              </td><td>2584914              </td></tr>
	<tr><td>790107-01            </td><td>Baby Opaque Tights   </td><td>2669863              </td><td>2584962              </td></tr>
	<tr><td>790101-14            </td><td>Kids Classic Trainers</td><td>2669874              </td><td>2685550              </td></tr>
	<tr><td>790101-14            </td><td>Kids Classic Trainers</td><td>2669874              </td><td>2685550              </td></tr>
	<tr><td>790298-28            </td><td>Fashion Headband     </td><td>2669805              </td><td>2584854              </td></tr>
	<tr><td>790284-49            </td><td>Mini Scrunchie       </td><td>2669810              </td><td>2584858              </td></tr>
	<tr><td>790284-43            </td><td>Mini Scrunchie       </td><td>2669812              </td><td>2584869              </td></tr>
	<tr><td>2008466-06           </td><td>The Baby Tee         </td><td>2731624              </td><td>2741473              </td></tr>
	<tr><td>2008466-04           </td><td>The Baby Tee         </td><td>2731626              </td><td>2741474              </td></tr>
	<tr><td>2008466-02           </td><td>The Baby Tee         </td><td>2731627              </td><td>2741475              </td></tr>
	<tr><td>2008466-01           </td><td>The Baby Tee         </td><td>2731628              </td><td>2741476              </td></tr>
	<tr><td>2008466-06           </td><td>The Baby Tee         </td><td>2731624              </td><td>2741473              </td></tr>
	<tr><td>2008466-04           </td><td>The Baby Tee         </td><td>2731626              </td><td>2741474              </td></tr>
	<tr><td>2008466-02           </td><td>The Baby Tee         </td><td>2731627              </td><td>2741475              </td></tr>
	<tr><td>2008466-01           </td><td>The Baby Tee         </td><td>2731628              </td><td>2741476              </td></tr>
</tbody>
</table>


When I determine how much prices samples we have for each of the shared products I find that I have, at best, only 6 price samples. What is more, the date ranges of the sampled prices do not cover the COVID-19 period (for example it goes from 2020-04-27 to 2020-05-25).

```R
for (i in 1:nrow(combined_nzus)) {
    nz_timeprices = product_prices(as.numeric(combined_nzus$nz_prod_id[i]))
    us_timeprices = product_prices(as.numeric(combined_nzus$us_prod_id[i]))
    print(paste(nrow(nz_timeprices), nrow(us_timeprices)))
}    
```
<br>

## Conclusion of this section
Oops! Not enough data for this analysis.

<br>
<br>
<br>

#### [1] [endcoronavirus.org](https://www.endcoronavirus.org/)
