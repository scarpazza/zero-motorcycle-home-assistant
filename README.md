# Zero Motorcycles integration into Home Assistant

Caveat: I tested this on my 2023 SR/F.
Please report malfunctions.


## Step 1

Determine your motorcycle's unit number

This is done by submitting the following query:

```
curl -s --http2 -G https://mongol.brono.com/mongol/api.php?commandname=get_units -d format=json -d pass=<your password -d user=<your user name>
```

The output will look like the following
```
[{"unitnumber":"1234567","name":"538ZFDZ00ABC12345","address":"+1",
"vehiclemodel":"","vehiclecolor":"","unittype":"5","icon":"0",
"active":"1","unitmodel":"6","regnumber":"","platenumber":null,
"custom":[]}]
```
Make a record of your unit number.

## Step 2

Test the REST API:

```
curl -s --http2 -G https.php?commandname=get_last_transmit -d format=json \
     -d user=<your username> -d pass=<your password> -d unitnumber=<your unit number>
```

Ensure you receive a reply similar to the following:

```
[{"unitnumber":"1234567", "name":"538ZFDZ00ABC12345", "unittype":"5",
"unitmodel":"6", "analog1":0.09, "analog2":0.09, "mileage":"12345.6",
"software_version":"221208", "logic_state":"1", "reason":"5", "response":"0",
"driver":"0", "longitude":-74.5678, "latitude":45.6789, "altitude":"123",
"gps_valid":"1", "gps_connected":"1", "satellites":"7", "velocity":"0",
"heading":"110", "emergency":"0", "shock":"", "ignition":"0", "door":"0",
"hood":"0", "volume":"0", "water_temp":"", "oil_pressure":"0", "main_voltage":13.28,
"fuel":"", "analog3":0.09, "siren":"0", "lock":"0", "int_lights":"0",
"datetime_utc":"20230902135246", "datetime_actual":"20230902135240",
"address":"PA, Towanda, Main Street", "perimeter":"", "color":2, "soc":95,
"tipover":0, "charging":0, "chargecomplete":1, "pluggedin":1,
"chargingtimeleft":0, "storage":0}]
```
(Spaces and linebreaks were added for clarity.)

## Step 3 - add secrets

Edit file `secrets.yaml` and add the following lines:

```
zero_motorcycles_unitnumber: 1234567                    
zero_motorcycles_username: your_usename@domain.com               
zero_motorcycles_password: your_password
```
(with your own data, of course, replacing the sample data above)

## Step 4 - add rest configuration

In `configuration.yaml` add a line:
```
rest: !include rest.yaml
```
Then, create a new file called [rest.yaml](rest.yaml) with the contents provided here.

## Step 5 - add template sensors

In `configuration.yaml`, under section `sensor:` add lines like the following:

```
sensor:
 - platform: template
   sensors: !include zero_motorcycle_sensors.yaml
```
Then, create a new file called [zero_motorcycle_sensors.yaml](zero_motorcycle_sensors.yaml) with the contents provided here.

## Step 6 - add a new UI view 

Edit your desired dashboard, clicking on the "..." menu and selecting "Raw configuration editor".
Add the contents of [lovelace_raw_additions.yaml](lovelace_raw_additions.yaml).

Boom. You are done.

