---
title: "Debugging public data - county codes in Texas"
date: "2025-05-16"
tags:
  - "analytics"
  - "data"
  - "project"
---

> If you sort a list of Texas counties by county name, the counties will not be in order by county number.  
> [Texas Center for Healthcare Statistics](https://www.dshs.texas.gov/center-health-statistics/texas-county-numbers-public-health-regions)

Every county in the United States has a FIPS code. These standards codes are usful for mapping and joining additional datasets.

Texas state agencies do not bother with these FIPS codes. Instead, each state agency publishes data using their own county codes. The county codes of one state agency may not match other agencies' county codes. I recently spent a few hours extremely confused by this, because I used the formula published by the Texas Department of State Health Services on data published by the Texas Health and Human Services, resulting in incorrect data.

I understand that imperfect solutions often come about because the success criteria is something that works, not something that meets everyone's needs in every situation.

And since they work, the additional amount of time needed to make them better often isn't worth it given all the things that don't work. For many tasks, that is probably fine - if you know the limitations of your solution and adjust accordingly.

But each Texas state agency using its own subtly different coding system for counties is the type of issue that deserves fixing because it can lead to bigger problems. Because they publish these codes to the public, and people disseminate data without guidance from the original authors. And so it surprises me that rather than settle on a standard set of county codes that could be used across all Texas state agencies, they each implement their own guidance.

## How do they assign codes?

The majority sort counties alphabetically by county name and use the row index number as the ID. You'll see this on the [Texas Comptroller codes](https://comptroller.texas.gov/taxes/resources/county-codes.php), and in the data file in question: the _Medicaid and CHIP Enrollment by Risk Group by County, Preliminary_ from the [Healthcare Statistics page on the Texas Health and Human Services website](https://www.hhs.texas.gov/about/records-statistics/data-statistics/healthcare-statistics) (HHS).

![Logo for HHS Website](/images/TX_HHS.png)

This data file is released at hhs.texas.gov, under the Texas Health and Human Services website.

However, this convenient solution doesn't give an easy conversion to FIPs codes.

When I found these county codes and wanted to get match them to the FIPs codes to join Census data, I [found a page](https://www.dshs.texas.gov/center-health-statistics/texas-county-numbers-public-health-regions) at the Texas Department of State Health Services (DSHS) website dshs.texas.gov that gives a helpful formula.

![Logo for DSHS Website](/images/TX_HHS_DSHS.png)

DSHS gives a formula to translate county IDs to FIPs codes:

> The county FIPS code can be calculated from the Texas county number: FIPS_code = 48000 + (county_number \* 2) - 1.

However, the County IDs published in the HHS Medicaid data file do not match the county numbers on the DSHS conversion table. This is because an alphabetic sort index doesn't match the order of the IDs:

- spaces change the order (Ellis after El Paso)
- prefixes ("Mc..") are sorted before other "M..." counties

So if I use the DSHS formula on the HHS data, I will get the incorrect FIPs code since the input County ID is not correct. In the published file, Martin county had the county ID of 156, which suggests a FIPS code of 48 + (156 \* 2) - 1 = 48311. The FIPS code 48311 actually corresponds with McMullen county.

I made the mistake of thinking that the advice published on the DSHS website applied to the data on the HHS website. From their logos, I thought the DSHS was a sub-office of the HHS.

Since this sorting error only switches a few of the counties, a majority of the data county IDs are correct. As someone not familiar with the data practices of various Texas state agencies, it was only when I normalized enrollment counts against population (from Census data) that I realized some counties had enrollment over 100%.

Intuitively I worked backwards, which cost me a few hours of work - I thought my join must be wrong, or the Census data I pulled, or when I calculated the FIPs codes. I also foolishly checked other state agency data - which only further confused me, because since the other agency did use the alphabetic sort method, the data across both files matched. I also checked the data in the file by county name, which held up, since the county names and counts were always correct!

Correcting this meant pulling the published HHS Healthcare Statistics numbers and joining (based on name) to the DSHS table online to pull the correct ID and FIPs codes.

## Overall

Texas state agencies use county IDs that do not align with the federal standard. I am curious as to why this was implemented - it is easy to think that doing the alphabetic sort by name for them was just easier and they didn't think about the consequences, but perhaps I am being cynical.

## Alphabetic Texas County IDs to FIPs Codes

The following table links the Texas County IDs (obtained through the row index from a standard alphabetic sort) to their county FIPs codes.

| Alphabetic Sort County ID | DSHS Number | County Name   | FIPS Code |
| ------------------------- | ----------- | ------------- | --------- |
| 1                         | 1           | Anderson      | 48001     |
| 2                         | 2           | Andrews       | 48003     |
| 3                         | 3           | Angelina      | 48005     |
| 4                         | 4           | Aransas       | 48007     |
| 5                         | 5           | Archer        | 48009     |
| 6                         | 6           | Armstrong     | 48011     |
| 7                         | 7           | Atascosa      | 48013     |
| 8                         | 8           | Austin        | 48015     |
| 9                         | 9           | Bailey        | 48017     |
| 10                        | 10          | Bandera       | 48019     |
| 11                        | 11          | Bastrop       | 48021     |
| 12                        | 12          | Baylor        | 48023     |
| 13                        | 13          | Bee           | 48025     |
| 14                        | 14          | Bell          | 48027     |
| 15                        | 15          | Bexar         | 48029     |
| 16                        | 16          | Blanco        | 48031     |
| 17                        | 17          | Borden        | 48033     |
| 18                        | 18          | Bosque        | 48035     |
| 19                        | 19          | Bowie         | 48037     |
| 20                        | 20          | Brazoria      | 48039     |
| 21                        | 21          | Brazos        | 48041     |
| 22                        | 22          | Brewster      | 48043     |
| 23                        | 23          | Briscoe       | 48045     |
| 24                        | 24          | Brooks        | 48047     |
| 25                        | 25          | Brown         | 48049     |
| 26                        | 26          | Burleson      | 48051     |
| 27                        | 27          | Burnet        | 48053     |
| 28                        | 28          | Caldwell      | 48055     |
| 29                        | 29          | Calhoun       | 48057     |
| 30                        | 30          | Callahan      | 48059     |
| 31                        | 31          | Cameron       | 48061     |
| 32                        | 32          | Camp          | 48063     |
| 33                        | 33          | Carson        | 48065     |
| 34                        | 34          | Cass          | 48067     |
| 35                        | 35          | Castro        | 48069     |
| 36                        | 36          | Chambers      | 48071     |
| 37                        | 37          | Cherokee      | 48073     |
| 38                        | 38          | Childress     | 48075     |
| 39                        | 39          | Clay          | 48077     |
| 40                        | 40          | Cochran       | 48079     |
| 41                        | 41          | Coke          | 48081     |
| 42                        | 42          | Coleman       | 48083     |
| 43                        | 43          | Collin        | 48085     |
| 44                        | 44          | Collingsworth | 48087     |
| 45                        | 45          | Colorado      | 48089     |
| 46                        | 46          | Comal         | 48091     |
| 47                        | 47          | Comanche      | 48093     |
| 48                        | 48          | Concho        | 48095     |
| 49                        | 49          | Cooke         | 48097     |
| 50                        | 50          | Coryell       | 48099     |
| 51                        | 51          | Cottle        | 48101     |
| 52                        | 52          | Crane         | 48103     |
| 53                        | 53          | Crockett      | 48105     |
| 54                        | 54          | Crosby        | 48107     |
| 55                        | 55          | Culberson     | 48109     |
| 56                        | 56          | Dallam        | 48111     |
| 57                        | 57          | Dallas        | 48113     |
| 58                        | 58          | Dawson        | 48115     |
| 59                        | 59          | Deaf Smith    | 48117     |
| 60                        | 60          | Delta         | 48119     |
| 61                        | 61          | Denton        | 48121     |
| 62                        | 62          | DeWitt        | 48123     |
| 63                        | 63          | Dickens       | 48125     |
| 64                        | 64          | Dimmit        | 48127     |
| 65                        | 65          | Donley        | 48129     |
| 66                        | 66          | Duval         | 48131     |
| 67                        | 67          | Eastland      | 48133     |
| 68                        | 68          | Ector         | 48135     |
| 69                        | 69          | Edwards       | 48137     |
| 70                        | 71          | El Paso       | 48141     |
| 71                        | 70          | Ellis         | 48139     |
| 72                        | 72          | Erath         | 48143     |
| 73                        | 73          | Falls         | 48145     |
| 74                        | 74          | Fannin        | 48147     |
| 75                        | 75          | Fayette       | 48149     |
| 76                        | 76          | Fisher        | 48151     |
| 77                        | 77          | Floyd         | 48153     |
| 78                        | 78          | Foard         | 48155     |
| 79                        | 79          | Fort Bend     | 48157     |
| 80                        | 80          | Franklin      | 48159     |
| 81                        | 81          | Freestone     | 48161     |
| 82                        | 82          | Frio          | 48163     |
| 83                        | 83          | Gaines        | 48165     |
| 84                        | 84          | Galveston     | 48167     |
| 85                        | 85          | Garza         | 48169     |
| 86                        | 86          | Gillespie     | 48171     |
| 87                        | 87          | Glasscock     | 48173     |
| 88                        | 88          | Goliad        | 48175     |
| 89                        | 89          | Gonzales      | 48177     |
| 90                        | 90          | Gray          | 48179     |
| 91                        | 91          | Grayson       | 48181     |
| 92                        | 92          | Gregg         | 48183     |
| 93                        | 93          | Grimes        | 48185     |
| 94                        | 94          | Guadalupe     | 48187     |
| 95                        | 95          | Hale          | 48189     |
| 96                        | 96          | Hall          | 48191     |
| 97                        | 97          | Hamilton      | 48193     |
| 98                        | 98          | Hansford      | 48195     |
| 99                        | 99          | Hardeman      | 48197     |
| 100                       | 100         | Hardin        | 48199     |
| 101                       | 101         | Harris        | 48201     |
| 102                       | 102         | Harrison      | 48203     |
| 103                       | 103         | Hartley       | 48205     |
| 104                       | 104         | Haskell       | 48207     |
| 105                       | 105         | Hays          | 48209     |
| 106                       | 106         | Hemphill      | 48211     |
| 107                       | 107         | Henderson     | 48213     |
| 108                       | 108         | Hidalgo       | 48215     |
| 109                       | 109         | Hill          | 48217     |
| 110                       | 110         | Hockley       | 48219     |
| 111                       | 111         | Hood          | 48221     |
| 112                       | 112         | Hopkins       | 48223     |
| 113                       | 113         | Houston       | 48225     |
| 114                       | 114         | Howard        | 48227     |
| 115                       | 115         | Hudspeth      | 48229     |
| 116                       | 116         | Hunt          | 48231     |
| 117                       | 117         | Hutchinson    | 48233     |
| 118                       | 118         | Irion         | 48235     |
| 119                       | 119         | Jack          | 48237     |
| 120                       | 120         | Jackson       | 48239     |
| 121                       | 121         | Jasper        | 48241     |
| 122                       | 122         | Jeff Davis    | 48243     |
| 123                       | 123         | Jefferson     | 48245     |
| 124                       | 124         | Jim Hogg      | 48247     |
| 125                       | 125         | Jim Wells     | 48249     |
| 126                       | 126         | Johnson       | 48251     |
| 127                       | 127         | Jones         | 48253     |
| 128                       | 128         | Karnes        | 48255     |
| 129                       | 129         | Kaufman       | 48257     |
| 130                       | 130         | Kendall       | 48259     |
| 131                       | 131         | Kenedy        | 48261     |
| 132                       | 132         | Kent          | 48263     |
| 133                       | 133         | Kerr          | 48265     |
| 134                       | 134         | Kimble        | 48267     |
| 135                       | 135         | King          | 48269     |
| 136                       | 136         | Kinney        | 48271     |
| 137                       | 137         | Kleberg       | 48273     |
| 138                       | 138         | Knox          | 48275     |
| 139                       | 142         | La Salle      | 48283     |
| 140                       | 139         | Lamar         | 48277     |
| 141                       | 140         | Lamb          | 48279     |
| 142                       | 141         | Lampasas      | 48281     |
| 143                       | 143         | Lavaca        | 48285     |
| 144                       | 144         | Lee           | 48287     |
| 145                       | 145         | Leon          | 48289     |
| 146                       | 146         | Liberty       | 48291     |
| 147                       | 147         | Limestone     | 48293     |
| 148                       | 148         | Lipscomb      | 48295     |
| 149                       | 149         | Live Oak      | 48297     |
| 150                       | 150         | Llano         | 48299     |
| 151                       | 151         | Loving        | 48301     |
| 152                       | 152         | Lubbock       | 48303     |
| 153                       | 153         | Lynn          | 48305     |
| 154                       | 157         | Madison       | 48313     |
| 155                       | 158         | Marion        | 48315     |
| 156                       | 159         | Martin        | 48317     |
| 157                       | 160         | Mason         | 48319     |
| 158                       | 161         | Matagorda     | 48321     |
| 159                       | 162         | Maverick      | 48323     |
| 160                       | 154         | McCulloch     | 48307     |
| 161                       | 155         | McLennan      | 48309     |
| 162                       | 156         | McMullen      | 48311     |
| 163                       | 163         | Medina        | 48325     |
| 164                       | 164         | Menard        | 48327     |
| 165                       | 165         | Midland       | 48329     |
| 166                       | 166         | Milam         | 48331     |
| 167                       | 167         | Mills         | 48333     |
| 168                       | 168         | Mitchell      | 48335     |
| 169                       | 169         | Montague      | 48337     |
| 170                       | 170         | Montgomery    | 48339     |
| 171                       | 171         | Moore         | 48341     |
| 172                       | 172         | Morris        | 48343     |
| 173                       | 173         | Motley        | 48345     |
| 174                       | 174         | Nacogdoches   | 48347     |
| 175                       | 175         | Navarro       | 48349     |
| 176                       | 176         | Newton        | 48351     |
| 177                       | 177         | Nolan         | 48353     |
| 178                       | 178         | Nueces        | 48355     |
| 179                       | 179         | Ochiltree     | 48357     |
| 180                       | 180         | Oldham        | 48359     |
| 181                       | 181         | Orange        | 48361     |
| 182                       | 182         | Palo Pinto    | 48363     |
| 183                       | 183         | Panola        | 48365     |
| 184                       | 184         | Parker        | 48367     |
| 185                       | 185         | Parmer        | 48369     |
| 186                       | 186         | Pecos         | 48371     |
| 187                       | 187         | Polk          | 48373     |
| 188                       | 188         | Potter        | 48375     |
| 189                       | 189         | Presidio      | 48377     |
| 190                       | 190         | Rains         | 48379     |
| 191                       | 191         | Randall       | 48381     |
| 192                       | 192         | Reagan        | 48383     |
| 193                       | 193         | Real          | 48385     |
| 194                       | 194         | Red River     | 48387     |
| 195                       | 195         | Reeves        | 48389     |
| 196                       | 196         | Refugio       | 48391     |
| 197                       | 197         | Roberts       | 48393     |
| 198                       | 198         | Robertson     | 48395     |
| 199                       | 199         | Rockwall      | 48397     |
| 200                       | 200         | Runnels       | 48399     |
| 201                       | 201         | Rusk          | 48401     |
| 202                       | 202         | Sabine        | 48403     |
| 203                       | 203         | San Augustine | 48405     |
| 204                       | 204         | San Jacinto   | 48407     |
| 205                       | 205         | San Patricio  | 48409     |
| 206                       | 206         | San Saba      | 48411     |
| 207                       | 207         | Schleicher    | 48413     |
| 208                       | 208         | Scurry        | 48415     |
| 209                       | 209         | Shackelford   | 48417     |
| 210                       | 210         | Shelby        | 48419     |
| 211                       | 211         | Sherman       | 48421     |
| 212                       | 212         | Smith         | 48423     |
| 213                       | 213         | Somervell     | 48425     |
| 214                       | 214         | Starr         | 48427     |
| 215                       | 215         | Stephens      | 48429     |
| 216                       | 216         | Sterling      | 48431     |
| 217                       | 217         | Stonewall     | 48433     |
| 218                       | 218         | Sutton        | 48435     |
| 219                       | 219         | Swisher       | 48437     |
| 220                       | 220         | Tarrant       | 48439     |
| 221                       | 221         | Taylor        | 48441     |
| 222                       | 222         | Terrell       | 48443     |
| 223                       | 223         | Terry         | 48445     |
| 224                       | 224         | Throckmorton  | 48447     |
| 225                       | 225         | Titus         | 48449     |
| 226                       | 226         | Tom Green     | 48451     |
| 227                       | 227         | Travis        | 48453     |
| 228                       | 228         | Trinity       | 48455     |
| 229                       | 229         | Tyler         | 48457     |
| 230                       | 230         | Upshur        | 48459     |
| 231                       | 231         | Upton         | 48461     |
| 232                       | 232         | Uvalde        | 48463     |
| 233                       | 233         | Val Verde     | 48465     |
| 234                       | 234         | Van Zandt     | 48467     |
| 235                       | 235         | Victoria      | 48469     |
| 236                       | 236         | Walker        | 48471     |
| 237                       | 237         | Waller        | 48473     |
| 238                       | 238         | Ward          | 48475     |
| 239                       | 239         | Washington    | 48477     |
| 240                       | 240         | Webb          | 48479     |
| 241                       | 241         | Wharton       | 48481     |
| 242                       | 242         | Wheeler       | 48483     |
| 243                       | 243         | Wichita       | 48485     |
| 244                       | 244         | Wilbarger     | 48487     |
| 245                       | 245         | Willacy       | 48489     |
| 246                       | 246         | Williamson    | 48491     |
| 247                       | 247         | Wilson        | 48493     |
| 248                       | 248         | Winkler       | 48495     |
| 249                       | 249         | Wise          | 48497     |
| 250                       | 250         | Wood          | 48499     |
| 251                       | 251         | Yoakum        | 48501     |
| 252                       | 252         | Young         | 48503     |
| 253                       | 253         | Zapata        | 48505     |
| 254                       | 254         | Zavala        | 48507     |

Thanks to TablesGenerator through https://www.tablesgenerator.com/markdown_tables.
