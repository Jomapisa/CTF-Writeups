# Pure Geoguessrrr

## Challenge

i dont like geoguessers because it's too complicated for me. (i have a terrible sense of direction plz help) Fortunately i came across something on the streets and found this piece of paper. ^_^

Attachment: [pure-geoguesserrr.txt](https://github.com](https://github.com/Jomapisa/CTF-Writeups/blob/dc53285f1af08aaf5c39ebb5c5a79a8677f54aa4/2024/Firebird%20CTF%202024/pure-geoguesserrr.txt)

(PS: u need to add 'firebird{}' after solving and all characters are upper-case alphabets with underscores :D )

## Writeup

From the challenge name I assumed the codes were related to some geographical data, but couldn't find anything. However, "i came across something on the streets" suggests it could be something specific to Hong Kong. After some trial and error I found this database: https://portal.csdi.gov.hk/geoportal/#metadataInfoPanel containing the location of lamp posts.

We can see that the codes match some values in the LAMP_NO column, so I used pandas to filter the dataset.

```python

import pandas as pd


ids = ["CF8411","CF8410","CF8409","CF8408","E8768","E8767","E8766","AB5355","O3933","AA9366","AA1454","E8763","AF1757","AF1759","AF1760","E8777","GF2016","E8775","AA9228","GF4810","AA9231","E8755","AA9375","E8772","E8770","AB4827","AA3907","AB4829","E8752","E8750","E8747","AA2457","AA3863","E8746","AA9383","AA9384","AF1755","AF1756","E8745","GF2480","E8744","AA9385","AA1788","AA9387","AA3656","AB0406","BF1500","K7747","GF2014","AB5684","AA1791","BF3217","BF0589","AA3502","AA3503","AA3504","AA3505","AA3506","AB1433","AB1431","AB1429","AA3508","GF1199","AA3257","AA3657","AA1799","BF0906","E8736","E8737","E8735","E8734","E8733","E8732","AA4511","AB1371","AA3513","AA3514","AA1805","AA1806","BF0902","AA9332","AA9333","AA9335","CF0059","AA9339","AB5529","AA3086","AA3519","AA3518","AA1809","AA3082","E8724","E8723","E8722","AA3083","AA1811","AA1812","AA6475","AA3085","AA8386","AA8387","AA3176","E8720","E8718","E8717","AB0398","AA9613","E8716","K7929","K7930","K7925","K7898","AB2759","AA3131","AF1078","AB2761","K7916","K7819","K7933","K7934","AA4792","AA4791","E8574","E8575","AA3135","AA3134","AA3133","E9265","BF0594","BF0595","BF0596","AB1842","E9261","E9262","AF2018","AA4508","AA3137","AA3139","AA3576","AB5711","AB5710","CF1391","AA9561","AB0660","AB5511","E0836","AA9938","AB5508","AF2287","K7471","E8651","AA9076","AA9078"]
df = pd.read_csv('Lamppost.csv')
df1 = pd.DataFrame(columns=df.columns)
for id in ids:
    try:
        df1.loc[len(df1.index)] = df.loc[df['LAMP_NO'] == id].values[0]
    except:
        print(id) #there's a lamp post missing...
df1.to_csv("coords.csv")
```

Then I imported the csv into google earth. The points seemed to make characters when connected to each other, so I used this website https://www.gpsvisualizer.com/map_input?form=googleearth to turn the csv into a KML file. Opening it in google earth we can find the flag.

! [flag screenshot]([https://github.com/](https://github.com/Jomapisa/CTF-Writeups/blob/dc53285f1af08aaf5c39ebb5c5a79a8677f54aa4/2024/Firebird%20CTF%202024/flag%20screenshot.png)

firebird{LAMP_POSTS_ARE_USEFUL}

