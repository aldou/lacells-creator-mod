lacells-creator-mod
===================

Script for generating a cell tower database for µg UnifiedNlp (UnifiedNlp)

### About µg UnifiedNlp
µg UnifiedNlp is a FLOSS (Free/Libre Open Source Software) tool for geolocating android phones without Google's Geolocation service. It allows apps that use Android's coarse or network locating features to geolocate the phone which is usually faster and less battery consuming then GPS. 
The real location work is done by backends (plug-ins) that can be configured through the UnifiedNlp UI. 
The LocalGsmNlpBackend backend performs no network data. All data acquired by the phone stays on the phone and no queries are made to a centralized AP location provider.
Therefor a database must be generated and placed in the .nogapps folder of your internal phone storage.

### About this script
This script is a modified version of https://github.com/n76/lacells-creator.  
This mod creates the database for the LocalGsmNlpBackend, including woldwide tower information from OpenCellId and Mozilla Location Services.

### Folder for the generated database
/storage/emulated/0/.nogapps/lacells.db

### Required downloads besides this script
1. https://f-droid.org/repository/browse/?fdid=com.google.android.gms
2. https://f-droid.org/repository/browse/?fdid=org.fitchfamily.android.gsmlocation

µg UnifiedNlp is part of the μg Project at https://github.com/microg 

### Requirements

1. bash - Main script is written in bash
2. wget - Used to pull CSV files from OpenCellId and Mozilla Location Services
3. OpenCellID API Key - Needed to pull CSV files from OpenCellID. Get one for free at http://wiki.opencellid.org/wiki/How_to_join
4. sqlite3 - Used to generate the actual database for the phone

### Setup
The gen_lacells_mergedMod script needs to be edited to use your OpenCellId API key.

### How To Run
1. From the bash shell prompt enter:

```
	cd gen_lacells_directory
	./gen_lacells_mergedMod
```
or if you already downloaded the raw data

```
	cd gen_lacells_directory
	./gen_lacells_mergedModWithoutDownload
```
2. Push the the generated database to /storage/emulated/0/.nogapps/lacells.db
```
	adb push lacells.db /storage/emulated/0/.nogapps/lacells.db
```

The gen_lacells_merged version of the script will merge duplicate tower information where both OpenCellID and Mozilla Location Services have information on the same tower. The gen_lacells script will leave the duplicates in place.

### Other Considerations

The OpenCellId project limits downloads to one per day per API key. So this script will only run correctly the first time it is run per day.

### Comment on database sizes (20Dec2014)
The database generated by this script has a significant size.

Reasons:

The 95,171,584 byte cells-world.db contains 2,486,708 cell towers. As of 20Dec2014, a database from OpenCellId for the whole world will contain 6,891,208 towers (177% increase in tower count).

To the second point, adding Mozilla data increases the non-duplicate tower count to 8,456,327 a total 240% greater than the original database.

To the last point, adding indices for faster run time access increase the non-duplicate combined database from 423,677,952 to 729,165,824 bytes (72% increase).

