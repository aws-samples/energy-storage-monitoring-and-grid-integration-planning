    PUT batteries-feeder-measurements
        {
        "mappings": {
        "properties": {
        "measureddatetime": {
        "type": "date",
        "format": "yyyy-MM-dd HH:mm:ss"
        },
        "batteryid": {
        "type": "integer"
        },
        "becapacitykwh": {
        "type": "double"
        },
        "bmaxpowerkw": {
        "type": "double"
        },
        "bnomdcvoltagevolts": {
        "type": "double"
        },
        "bmaxcurrentamperes": {
        "type": "double"
        },
        "bcurchargelevelkwh": {
        "type": "double"
        },
        "bnewchargelevelkwh": {
        "type": "double"
        },
        "bnewcurrentamperes": {
        "type": "double"
        },
        "bnewvoltagevolts": {
        "type": "double"
        },
        "binstalllat": {
        "type": "double"
        },
        "binstalllong": {
        "type": "double"
        },
        "installlocation": {
        "type": "geo_point"
        },
        "bfullmessagestring": {
        "type": "text"
        },
        "thismessagetype": {
        "type": "text"
        },
        "feederid": {
        "type": "integer"
        },
        "freadpowerkw": {
        "type": "double"
        },
        "freadpowerkva": {
        "type": "double"
        },
        "fkwhunits": {
        "type": "integer"
        },
        "ffrequency": {
        "type": "double"
        },
        "connectedsubstation": {
            "type": "keyword"
        },
        "fvoltagekv": {
        "type": "double"
        },
        "fcurrentamperes": {
        "type": "double"
        },
        "fkvahunits": {
        "type": "integer"
        },
        "fpf": {
        "type": "double"
        },
        "fratedpowerkva": {
        "type": "double"
        },
        "finstalllat": {
        "type": "double"
        },
        "finstalllong": {
        "type": "double"
        },
        "rfarmid": {
        "type": "integer"
        },
        "rcapacitykw": {
        "type": "double"
        },
        "rcapacitykva": {
        "type": "double"
        },
        "rreadvoltagevolts": {
        "type": "double"
        },
        "rccurrentamperes": {
        "type": "double"
        },
        "rinstalllat": {
        "type": "double"
        },
        "rinstalllong": {
        "type": "double"
        },
        "rpeakkw": {
        "type": "double"
        },
        "rpeakkva": {
        "type": "double"
        },
        "rkwhunits": {
        "type": "double"
        },
        "rkvahunits": {
        "type": "double"
        }
        }
        } }
        