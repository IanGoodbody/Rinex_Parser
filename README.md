# Rinex Parser
### Tools for converting RINEX files to more easily managed formats

RINEX (receiver independent exchange format) is a standard GNSS file format for
sharing GNSS receiver information; 
while many GNSS receivers will offer a 
variety of output formats for storage and post processing, RINEX logs are almost
universally available from commercial receivers and online datasets.
This collection of scripts is designed to convert RINEX files into more 
accessible CSV formats that can be more easily read into matrix or spreadsheet
software.  

This software was designed to comply with the RINEX 2.11 standard available 
[here](https://igscb.jpl.nasa.gov/igscb/data/format/rinex211.txt).  The software
has only been tested on GPS observation files with the `.??O` file extension.
Support for GLONASS or Galileo systems may require modifications to the entry
parsing algorithm.  
GPS navigation (ephemeris) file support has not been researched and may require
larger changes to the script.

## Sample Data Files
The two files `cat20010.16o` and `cat20010.16n` are provided as sample 
observation and navigation files respectively.  GNSS measurements, such as
these, are publicaly available and can be downloaded from the
[NOAA CORS](https://geodesy.noaa.gov/CORS/data.shtml) website.
The `cat2` site is located in Avalon, California.

## Usage
#### Observation RINEX to CSV
`RinexObsToCSV.py` converts a RINEX observation to a comma separated value 
format composed of user specified fields.  
Available fields are time in GPS week-seconds (`time`), PRN/satellite number 
(`prn`), code measurements in meters, carrier phase measurements in meters, 
Doppler measurements Hertz, and SNR.  The measurements for each frequency band
are specified using the two character codes (`C1`, `S5`, etc) specified in 
section 10.1.1 and Table A.1 of the 
[RINEX 2.11 specification](https://igscb.jpl.nasa.gov/igscb/data/format/rinex211.txt).
This script requires Python 2.7.

`python RinexObsToCSV.py [Rinex_File [CSV_File]] [-al] [-f Fields]`
- `Rinex_File`: RINEX observation file (`.??o` extension)
- `CSV_File`: Output file name, the output name is generated from `Rinex_File` 
by default if this argument is absent.
- `-a`: Append the program output to the output file, overwrites by default
- `-l`: Parse only legacy L1 and L2 C/A and P measurements, overrides `-f`
- `-f Fields`: User specified fields, parses all available if this or `-l` is
absent

If no arguments are given the program is set to parse the sample file by 
default.
The following will parse the sample observation file for the code measurements
of the C/A and P signals. 
```
python RinexObsToCSV.py cat20010.16o pseudoranges.csv -f time prn C1 P1 
```
